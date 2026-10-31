## 3. Concorrência no AWS Lambda (Concurrency)
A concorrência representa a quantidade de invocações
do AWS Lambda que estão sendo executadas simultaneamente
em um determinado momento. Se a demanda ultrapassar os limites
de concorrência da conta ou da função, ocorre o Throttling (estrangulamento de requisições).

Fórmula de Cálculo da Concorrência (Síncrona / Assíncrona):

<img width="902" height="285" alt="Image" src="https://github.com/user-attachments/assets/1f4dcf41-0b7d-4e41-85b7-ccd01077e57f" />


## 4. Concorrência por Modelo de Invocação

| **Modelo de Invocação**                      | **Métrica de Concorrência**                                | **Comportamento em Caso de Erro / Throttling**                                                            |
| -------------------------------------------- | ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **Síncrono** (*Synchronous*)                 | Taxa de requisições × Duração média da função.             | **Sem retentativas embutidas.** O erro/throttling retorna imediatamente ao cliente.                       |
| **Assíncrono** (*Asynchronous*)              | Taxa de requisições × Duração média da função.             | **2 retentativas automáticas** para erros de função; fila interna com retenção de até 6h para throttling. |
| **Streaming** (*Kinesis / DynamoDB Streams*) | **1 invocação por Shard** (por padrão).                    | Retenta continuamente o lote com falha até o sucesso ou expiração do dado (*poison message*).             |
| **Sondagem (Polling)** (*Amazon SQS*)        | Ajustada conforme a **profundidade de mensagens** na fila. | Retenta até o *Visibility Timeout* ou limite de `maxReceiveCount` (DLQ).                                  |


> 💡 Recurso de Fator de Paralelismo (Parallelization Factor):
> Para fontes de streaming como Kinesis e DynamoDB Streams, é possível
> configurar um fator de paralelização para permitir mais de uma invocação
> concorrente por Shard (dividindo o processamento por Partition Key).



## 5. Recursos Adicionais (Documentação e Artigos)

| **Tipo** | **Recurso** | **Descrição** |
|---|---|---|
| 📖 **Documentação** | [Managing Lambda reserved concurrency](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html) | Guia oficial sobre como configurar a concorrência reservada em funções Lambda. |
| 📰 **Blog Post** | [AWS Lambda Supports Parallelization Factor for Kinesis and DynamoDB Event Sources](https://aws.amazon.com/blogs/compute/new-aws-lambda-scaling-controls-for-kinesis-and-dynamodb-event-sources/) | Explicação sobre como aumentar o throughput de streams com o fator de paralelização. |
| 📰 **Blog Post** | [New AWS Lambda Scaling Controls for Kinesis and DynamoDB Event Sources](https://aws.amazon.com/blogs/compute/new-aws-lambda-scaling-controls-for-kinesis-and-dynamodb-event-sources/) | Detalhes sobre os controles de escalonamento para fontes de eventos em fluxo. |
