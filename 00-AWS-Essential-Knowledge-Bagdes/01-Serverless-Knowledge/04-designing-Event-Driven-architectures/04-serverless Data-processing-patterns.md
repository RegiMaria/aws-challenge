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

**B. Kinesis Data Streams vs. Kinesis Data Firehose**

A escolha entre Data Streams e Data Firehose depende do nível de controle e dos destinos necessários:
```

┌───────────────────────────────┐               ┌───────────────────────────────┐
│     Kinesis Data Streams      │               │     Kinesis Data Firehose     │
├───────────────────────────────┤               ├───────────────────────────────┤
│ • Controle manual de shards.  │               │ • Auto-managed (sem shards).  │
│ • Múltiplos consumidores.     │               │ • Destino único por stream.   │
│ • Ordem e Exactly-Once OK.    │               │ • Entrega direta em S3, Red-  │
│ • Bloqueia o shard em erro.   │               │   shift, OpenSearch, Splunk.  │
└───────────────────────────────┘               └───────────────────────────────┘


```

| Característica | Amazon Kinesis Data Streams | Amazon Kinesis Data Firehose |
| --- | --- | --- |
| Gerenciamento de Capacidade | Você define e gerencia o número de shards. | Totalmente gerenciado pela AWS com base no volume. |
| Consumidores | Múltiplos consumidores e tipos simultâneos. | Destino único configurado na criação (S3, Redshift, OpenSearch, Splunk). |
| Garantia de Ordem | Garantida dentro do shard (ordem exata e exactly-once). | Ordem não garantida; mensagens podem ser entregues mais de uma vez. |
| Comportamento de Falha | Falhas bloqueiam o shard até que o erro seja corrigido ou o tempo de retenção expire. | Faz 3 tentativas e descarta/envia os registros com falha para uma pasta `processing_failed` no S3. |
| Transformação de Dados | Requer código customizado no consumidor. | Permite transformação via Lambda ou conversão direta para Parquet / ORC antes de salvar. |


**C. Análise em Tempo Real com Kinesis Data Analytics**

O Kinesis Data Analytics permite executar consultas SQL ou aplicações Apache
Flink diretamente sobre o fluxo de dados em movimento (sliding windows).

- Pré-processamento: Pode acionar uma função Lambda para enriquecer, filtrar ou transformar os dados antes do SQL ser executado.

- Fontes e Destinos: Aceita entradas vindas de Kinesis Data Streams ou Kinesis Data Firehose e pode enviar os
resultados para esses mesmos serviços ou para uma função Lambda.

---

## 2. Filtros de Mensagens, Fan-out e Aplicações Aninhadas com Amazon SNS
