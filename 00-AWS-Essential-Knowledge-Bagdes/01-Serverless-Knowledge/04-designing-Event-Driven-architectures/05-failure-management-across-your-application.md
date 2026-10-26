# 05 - Failure Management Across Your Application

Em arquiteturas orientadas a eventos e desacopladas, existem múltiplos componentes interagindo de forma assíncrona.
Gerenciar falhas de forma resiliente, desde o código até a infraestrutura, é indispensável para evitar gargalos,
perda de dados ou comportamento imprevisível no sistema.

## 1. Gestão de Falhas no Código e nas Funções

**A. Monitoramento com CloudWatch**

- CloudWatch & CloudWatch Logs: Todo stdout ou erro gerado na função Lambda é automaticamente capturado e agregado pelo Amazon CloudWatch Logs.

- Métricas Customizadas: Utilize a biblioteca de logs nativa do seu runtime para estruturar logs e gerar métricas customizadas
para alarmes no CloudWatch.

**B. Retentativas no SDK da AWS**

- O AWS SDK possui mecanismos nativos de backoff e retentativa (exponential backoff with jitter).

- Quando você gera o SDK a partir de um stage do Amazon API Gateway, o código gerado já inclui esses mecanismos de
retentativa embutidos para facilitar a integração pelos clientes.

---

## 2. Tipos de Erros em Funções Lambda

Quando uma chamada falha, o erro se enquadra em uma de duas categorias:
```

 ┌─────────────────────────────────────────────────────────────┐
 │ 1. FUNCTION ERROR (Erro de Função)                          │
 │    O Lambda entregou o evento com sucesso, mas a função   │
 │    lançou uma exceção/erro ou estourou o Timeout.           │
 └──────────────────────────────┬──────────────────────────────┘
                                │
                                ▼
 ┌────────────────────────────────────────────────────────────────┐
 │ 2. INVOCATION ERROR (Erro de Invocação)                        │
 │    A requisição foi rejeitada ANTES da função receber o        │
 │    evento (Ex: payload gigante, sem permissão IAM, Throttling) │
 └────────────────────────────────────────────────────────────────┘

```

---

## 3. Tratamento de Erros por Modelo de Invocação

**A. Eventos Síncronos (Ex: API Gateway)**

- Retentativas: Não há retentativas automáticas embutidas.

- Ação Obrigatória: O código do cliente/aplicação deve capturar o erro e tratar a retentativa.

- Timeout: O API Gateway possui um timeout fixo de 30 segundos. Se o Lambda não responder nesse tempo, um erro 504 é retornado.

**B. Eventos Assíncronos (Ex: Amazon S3, Amazon SNS)**

- Fila Interna do Lambda: A AWS confirma o recebimento do evento para a fonte, coloca o evento em uma fila
interna do Lambda e tenta invocar a função em segundo plano.

- Retentativas Padrão para Function Errors: Tenta 2 vezes adicionais por padrão (total de 3 tentativas). Configurável de 0 a 2.

- Retentativas para Invocation Errors / Throttling: Tenta novamente por até 6 horas por padrão (ajustável via Maximum Event Age).

 Comportamento Especial do Amazon SNS: Possui política de entrega estendida própria para o Lambda (3 tentativas imediatas,
2 separadas por 1s, 10 com backoff de 1s a 20s, e até 100.000 retentativas a cada 20s).

**C. Eventos baseados em Streams (Ex: Kinesis Data Streams, DynamoDB Streams)**

Fontes de stream precisam manter a ordem exata dos registros por shard. Por padrão, se um lote falha, o Lambda
bloqueia o shard e tenta reprocessar o mesmo lote até o sucesso ou expiração do dado (Iterator-Age alto).

Para resolver isso, utilize os 4 recursos de gestão de falhas em streams:

```
 ┌──────────────────────────────────────────────────────────────────┐
 │ 1. BisectBatchOnFunctionError: Divide o lote com erro em 2.      │
 │ 2. MaximumRetryAttempts: Limita o número de tentativas.          │
 │ 3. MaximumRecordAge: Limita o tempo de vida do registro.         │
 │ 4. On-Failure Destination: Envia o registro falho para SQS/SNS.  │
 └──────────────────────────────────────────────────────────────────┘

```

**Exemplo Prático de Divisão de Lotes (Bisect):**





