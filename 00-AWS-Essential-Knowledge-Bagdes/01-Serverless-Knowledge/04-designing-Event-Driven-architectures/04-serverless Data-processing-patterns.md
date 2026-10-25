# 04- Serverless Data Processing Patterns

Este módulo aborda como usar serviços serverless para ingestão, processamento
e análise de dados em tempo real ou quase real (near real-time). Explora a diferença
entre processamento em fluxo (Streaming) com a família Amazon Kinesis e mensageria
pub/sub com Amazon SNS, Amazon SQS e Amazon EventBridge.


## 1. Processamento de Dados em Fluxo com Amazon Kinesis

O Amazon Kinesis permite ingerir e processar grandes volumes de dados de forma contínua e assíncrona.

**A. Conceitos-Chave: Produtores e Consumidores**

```

 ┌─────────────────┐                      ┌─────────────────┐                      ┌─────────────────┐
 │   PRODUTORES    │ ── (Put Records) ──► │  KINESIS STREAM │ ── (Get Records) ──► │   CONSUMIDORES  │
 │ (KPL, SDK, CLI) │                      │    (Shards)     │                      │ (Lambda, KCL)   │
 └─────────────────┘                      └─────────────────┘                      └─────────────────┘

```

- Produtores (Producers): Enviam dados para o fluxo utilizando a Kinesis Producer Library (KPL),
AWS SDK ou ferramentas de terceiros.

- Consumidores (Consumers): Leem e processam os dados utilizando o AWS Lambda,
a Kinesis Client Library (KCL) ou outros fluxos.

- Estrutura do Shard: Cada shard é uma unidade de capacidade que fornece uma
sequência única de registros identificados por um Sequence Number e organizados por uma Partition Key.

**Limites de Capacidade por Shard (Kinesis Data Streams):**

| Operação | Limite de Taxa (Rate Limit) | Limite de Dados (Data Limit) |
| --- | --- | --- |
| Escrita (Writes / Put) | 1.000 registros por segundo | 1 MB por segundo |
| Leitura (Reads / Get) | 5 transações por segundo (até 10.000 registros) | 2 MB por segundo |
