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
