# 04 - Scaling Considerations for Serverless Services

Para dimensionar uma arquitetura serverless com sucesso, não basta entender o comportamento de um único serviço.
É necessário conhecer as capacidades, os limites operacionais (service quotas) e os pontos de estrangulamento (bottlenecks)
em todas as integrações do fluxo ponta a ponta.

## 1. Os 4 Pilares da Consideração de Escala
Ao avaliar como cada serviço gerenciado afetará o dimensionamento da sua aplicação, analise sempre estes quatro fatores:
```

 ┌─────────────────────────────────────────────────────────────┐
 │ 1. TIMEOUTS (Tempo limite de execução)                      │
 │    Qual é o tempo máximo que cada componente aguarda?       │
 └──────────────────────────────┬──────────────────────────────┘
                                │
                                ▼
 ┌─────────────────────────────────────────────────────────────┐
 │ 2. RETRY BEHAVIORS (Comportamentos de Retentativa)          │
 │    Como os componentes lidam com falhas intermediárias?     │
 └──────────────────────────────┬──────────────────────────────┘
                                │
                                ▼
 ┌─────────────────────────────────────────────────────────────┐
 │ 3. THROUGHPUT (Vazão/fluxo de requisições)                        │
 │    Quantas requisições/segundo o backend suporta?           │
 └──────────────────────────────┬──────────────────────────────┘
                                │
                                ▼
 ┌─────────────────────────────────────────────────────────────┐
 │ 4. PAYLOAD SIZE (Tamanho da carga útil)                     │
 │    Qual é o menor limite aceito entre dois serviços?        │
 └─────────────────────────────────────────────────────────────┘
```

## 2. Considerações de Escala no Amazon API Gateway
O Amazon API Gateway atua como a "porta de entrada" (front door) da sua aplicação.
Ele oferece diversos recursos nativos para gerenciar os padrões de acesso e proteger
os sistemas downstream contra sobrecarga.


**A. Recursos do API Gateway para Gerenciamento de Tráfego**

- Endpoints Otimizados para Borda (Edge-Optimized Endpoints):

- Possuem uma distribuição do Amazon CloudFront integrada automaticamente.

- Reduzem a latência para clientes geograficamente dispersos.

- Cache da API (API Caching):

- Armazena em cache as respostas das requisições para evitar chamadas desnecessárias aos serviços de backend (como Lambda ou DynamoDB).

- Planos de Uso e Chaves de API (Usage Plans & API Keys):

- Permitem aplicar limites de taxa (Throttling / Burst Rate) por cliente/consumidor individual.

- Evitam que um único cliente consuma toda a capacidade do seu backend.


B. O Impacto dos Lambda Authorizers na Concorrência

Se você utiliza um Lambda Authorizer para autenticação customizada (ex: validação de tokens OAuth/JWT):

- Contagem no Limite de Concorrência: Cada execução da função do Authorizer conta diretamente para o limite total de concorrência da sua conta ou da região.

- Risco: Se o tráfego de autenticação for muito alto, a função do autorizador pode esgotar a concorrência disponível antes mesmo de executar a função que processa a regra de negócio.

- Solução de Mitigação (Auth Caching): Ative o cache de autorização (Authorization Caching). O resultado da autorização fica em cache por um período configurável (de 5 minutos a 1 hora), evitando chamadas repetidas à função autorizadora para o mesmo usuário.

## 3. Decisões Arquiteturais e Trade-offs

Ao projetar arquiteturas serverless, você deve ponderar entre simplicidade e capacidade de escala:

```
Opção A: Integração com Fila (Maior Desacoplamento)
┌─────────────┐       ┌────────────┐       ┌──────────────┐       ┌────────────────┐
│ API Gateway │ ───►  │ Amazon SQS │ ───►  │  AWS Lambda  │ ───►  │ Step Functions │
└─────────────┘       └────────────┘       └──────────────┘       └────────────────┘

Opção B: Integração Direta (Menor Complexidade)
┌─────────────┐       ┌────────────────┐
│ API Gateway │ ───►  │ Step Functions │
└─────────────┘       └────────────────┘
```

- Direct Service Integrations (Integrações Diretas): Conectar o API Gateway diretamente ao AWS Step Functions (removendo SQS e Lambda da intermediação) reduz a complexidade da arquitetura.

- Trade-off de Vazão: No entanto, essa abordagem direta exige verificar se a API de destino (ex: StartExecution do Step Functions) suporta o volume de requisições por segundo que o API Gateway enviará.

- Princípio Arquitetural: "Conheça os direcionadores reais do seu negócio e construa o que você precisa, em vez de projetar para uma escala infinita."

## 4. Compatibilidade de Limites de Payload (Payload Size)

Ao conectar serviços em uma esteira serverless, o menor limite de payload entre os serviços conectados dita o limite máximo da sua integração:

| **Serviço AWS**        | **Limite Máximo de Payload**                            |
| ---------------------- | ------------------------------------------------------- |
| **Amazon API Gateway** | **10 MB**                                               |
| **Amazon SQS**         | **256 KB**                                              |
| **AWS Lambda**         | **6 MB** (Invocação Síncrona) / **256 KB** (Assíncrona) |


> ⚠️ Atenção: Em uma integração direta do API Gateway (10 MB) para uma fila Amazon SQS (256 KB),
> o limite efetivo da sua API passa a ser 256 KB. Tentar enviar um payload maior que 256 KB resultará
> em erro na integração.

## 5. Scaling Considerations for Amazon SQS & AWS Lambda

1. Considerações de Escala no Amazon SQS

Quando o Amazon SQS é configurado como fonte de eventos do AWS Lambda, 
a própria AWS gerencia os processos de consulta (pollers), mas cabe a você ajustar 
as configurações para otimizar o desempenho e evitar gargalos.

**Comportamento de Escalonamento Automático do Polling**

- Início do Polling: Por padrão, o serviço do Lambda inicia com 5 processos paralelos fazendo polling na fila SQS.

- Crescimento Automático: Se a fila cresce, o Lambda adiciona até 60 novos pollers por minuto, atingindo no máximo
1.000 lotes simultâneos (ou até o limite de concorrência da conta/função).

- Redução em Erros (Back-off): Se a função Lambda começar a retornar erros, a AWS diminui automaticamente a taxa de polling para evitar sobrecarregar os sistemas downstream.

> ⚠️ Atenção aos Métricas: Se os logs mostrarem um aumento no tamanho da fila (QueueDepth) acompanhado por uma queda nas invocações do Lambda, isso indica que o Lambda está
> desacelerando o polling devido a erros na função.

**Tabela de Parâmetros: SQS como Fonte de Evento do Lambda**

### Tabela de Parâmetros: SQS como Fonte de Evento do Lambda

| **Parâmetro**                      | **Valor ou Limite**                        | **Quem/Como é Configurado**                                                         |
| ---------------------------------- | ------------------------------------------ | ----------------------------------------------------------------------------------- |
| **Tamanho do Lote (*Batch Size*)** | 1 a 10 mensagens                           | Configurado no gatilho da função Lambda.                                            |
| **Pollers Iniciais Padrão**        | 5 processos paralelos                      | Gerenciado automaticamente pelo Lambda.                                             |
| **Taxa de Incremento de Pollers**  | Até 60 por minuto                          | Gerenciado automaticamente pelo Lambda.                                             |
| **Lotes Simultâneos Máximos**      | Até 1.000                                  | Gerenciado automaticamente pelo Lambda.                                             |
| **Concorrência Máxima de Funções** | Menor valor entre 1.000 ou limite da conta | Definido via *Reserved Concurrency* na função.                                      |
| **Visibility Timeout**             | 0s a 12 horas                              | Configurado na **fila SQS** (Boa prática: **$6 \times \text{Timeout da Função}$**). |
| **Número de Retentativas**         | 1 a 1.000 (`maxReceiveCount`)              | Configurado na *Redrive Policy* da **fila SQS**.                                    |
| **Timeout da Função**              | 1s a 15 minutos                            | Configurado na função Lambda.                                                       |


**2. Considerações de Escala no AWS Lambda**

A. Limites de Concorrência e Estratégia Multi-Conta

```
  ┌─────────────────────────────────────────────────────────────┐
  │ 1. RESERVED CONCURRENCY (Concorrência Reservada)            │
  │    • Garante uma quantia fixa de concorrência para a função.│
  │    • Impede que a função estoure os recursos da conta.      │
  └──────────────────────────────┬──────────────────────────────┘
                                 │
                                 ▼
  ┌─────────────────────────────────────────────────────────────┐
  │ 2. EMERGENCY BRAKE (Freio de Emergência)                    │
  │    • Definir a concorrência para 0 BLOQUEIA imediatamente   │
  │      qualquer invocação da função em caso de desastre.      │
  └─────────────────────────────────────────────────────────────┘
```

- Segregação por Contas AWS: Manter ambientes de desenvolvimento, 
teste e produção na mesma conta cria competição por concorrência. 
Uma função de teste "desgovernada" pode derrubar a produção.

- AWS Organizations & Control Tower: A boa prática para evitar essa competição
é adotar uma estratégia multi-conta, isolando ambientes em contas AWS separadas 
organizadas hierarquicamente.

**B. Comportamento sob Picos Repentinos de Tráfego (Burst Behavior)**

Toda a concorrência da sua conta não fica disponível instantaneamente:

1.Aumento Imediato (Immediate Burst): Em um pico repentino de tráfego, o Lambda eleva a
concorrência instantaneamente até o limite da região.

2. Escalonamento Gradual: Após o limite inicial, o Lambda adiciona 500 invocações por
minuto até atender a demanda ou atingir o limite da conta.
