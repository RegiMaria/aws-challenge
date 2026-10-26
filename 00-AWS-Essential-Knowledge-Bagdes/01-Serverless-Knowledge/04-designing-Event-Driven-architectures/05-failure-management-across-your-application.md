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

1. lote original de 10 registros falha na 3ª posição.

2. O Lambda divide o lote em dois lotes de 5 e reseta os contadores de retry.

3. O primeiro lote de 5 falha novamente; o Lambda o divide em um lote de 2 e outro de 3.

4. O lote de 2 processa com sucesso! O lote de 3 falha e é dividido em 1 (o registro ruim) e 2.

5. O registro ruim esgota o Maximum Retry Attempts e é enviado para a On-Failure Destination (SNS/SQS).

6. O Lambda avança o ponteiro e processa os lotes restantes sem travar o stream.

> ⚠️ Idempotência é Obrigatória: Como a divisão de lotes reprocessa registros válidos que estavam no mesmo lote original,
> seu código deve ser idempotente (capaz de reprocessar o mesmo evento sem duplicar dados).

**D. Eventos baseados em Filas (Ex: Amazon SQS)**

- Visibilidade: Se a função falha ou estoura o timeout, o lote de mensagens volta a ficar visível na fila quando o Visibility Timeout expira.

- Configuração Recomendada de Timeout:
```
Visibility Timeout do SQS = 6 x Timeout da Função Lambda

```
- Por que multiplicar por 6? Garante tempo suficiente para o Lambda tentar novamente em caso de throttling
sem que as mensagens fiquem visíveis para outros consumidores prematuramente.

- Dead-Letter Queue (DLQ): Configure uma DLQ na fila SQS de origem com um `maxReceiveCount` para capturar mensagens com falhas persistentes.

---

## 4. On-Failure Destinations vs. Dead-Letter Queues (DLQ)

Para fontes assíncronas e de stream, a AWS recomenda usar On-Failure
Destinations em vez de DLQs tradicionais.

| **Característica** | **Dead-Letter Queue (DLQ)** | **On-Failure Destination** |
|---|---|---|
| **Destinos Suportados** | SQS ou SNS | SQS, SNS, EventBridge ou outra função Lambda |
| **Conteúdo do Payload** | Contém apenas o objeto do evento de entrada | Contém o evento de entrada + metadados detalhados de execução e stack trace de erro |
| **Flexibilidade** | Configuração fixa, acoplada à versão da função | Permite roteamento flexível e configurações de On-Success e On-Failure sem alterar o código |

**Tabela Resumo do Tratamento de Erros por Modelo**

| **Modelo / Evento** | **Timeout Padrão / Máximo** | **Comportamento de Retentativa** | **Onde Tratar Falhas Persistentes?** |
|---|---|---|---|
| **API Gateway (Síncrono)** | 30s | **Nenhuma retentativa automática.** | Tratar no SDK / Código do Cliente. |
| **Amazon SNS / S3 (Assíncrono)** | 15 min (Lambda) | 2 retentativas automáticas (até 6h em fila). | **On-Failure Destination** (SQS, SNS, EventBridge, Lambda) ou DLQ. |
| **Kinesis / DynamoDB Streams** | Retenção do Stream (ex.: 24h) | Repete até ter sucesso ou expirar o dado. | Usar **BisectBatch**, **MaxRetryAttempts** e **On-Failure Destination**. |
| **Amazon SQS (Polling)** | 15 min (Lambda) | Repete até o `maxReceiveCount` ser atingido. | Configurar **DLQ na fila SQS de origem**. |

---

À medida que uma aplicação serverless se expande e se torna um sistema distribuído de microsserviços,
a gestão de falhas precisa ir além de uma única função Lambda e cobrir o fluxo ponta a ponta da aplicação.

## 1. AWS Step Functions para Gestão de Falhas

Utilizar o AWS Step Functions permite orquestrar retentativas e tratamentos de erro de
forma declarativa na Amazon States Language (ASL), eliminando a necessidade de escrever
código manual de retry/backoff dentro das funções Lambda.

**A. Campos Retry e Catch**

Nos estados do tipo `Task` e `Parallel`, você pode configurar tratamento de erros nativo:

```

  ┌─────────────────────────────────────────────────────────────┐
  │ 1. RETRY (Tenta Novamente)                                  │
  │    Configura tentativas com backoff exponencial e jitter    │
  │    para erros transientes (Ex: Lambda.ServiceException).    │
  └──────────────────────────────┬──────────────────────────────┘
                                 │ (Se os retries falharem)
                                 ▼
  ┌─────────────────────────────────────────────────────────────┐
  │ 2. CATCH (Captura e Desvia)                                 │
  │    Captura a exceção específica ou genérica (States.ALL)    │
  │    e desvia o fluxo para um estado de Fallback/Compensação. │
  └─────────────────────────────────────────────────────────────┘

```
> Boa Prática: Todo código de produção no Step Functions que invoca uma função Lambda deve
> tratar explicitamente as exceções de serviço do Lambda: `Lambda.ServiceException` e `Lambda.SdkClientException`
