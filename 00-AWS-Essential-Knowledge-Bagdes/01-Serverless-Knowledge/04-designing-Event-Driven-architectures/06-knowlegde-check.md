# knowledge-check

## 1.Which of the following are true about Amazon Simple Queue Service (Amazon SQS) and AWS Lambda? (Select TWO.)


Select the correct answers and choose SUBMIT. For keyboard only accessibility, press TAB to navigate to the correct answer, and press SPACEBAR to select.
Repeat the step until all correct responses are checked, then press ENTER to submit.


01 - A best practice is to set the visibility timeout to 3 times the timeout you configure for your Lambda function.


02 - If a Lambda function returns errors when processing messages, Lambda decreases the number of processes polling the queue.


03 - Lambda has a default of five parallel processes to get things off of a queue.


04 - If there is an increase in queue size, Lambda decreases the concurrency required to reduce cost.


05 - If the visibility timeout expires before the Lambda function processes the messages, messages will be deleted by the queue.

**🟢Alternativas Corretas**
- Lambda has a default of five parallel processes to get things off of a queue.

Explicação:
Por padrão, o serviço do Lambda inicia com 5 processos de polling em paralelo (long polling) 
para ler lotes de mensagens da fila do SQS e ajustar a concorrência conforme o volume aumenta.

> (Nota sobre a primeira opção no simulado): A boa prática recomendada na documentação oficial e abordada neste curso é definir
> o Visibility Timeout para `6 vezes o timeout da função Lambda` (para cobrir eventuais retentativas por throttling do Lambda sem expirar
> a visibilidade da mensagem prematuramente).

**🔴 Alternativas Incorretas** 
❌ If a Lambda function returns errors when processing messages, Lambda decreases the number of processes polling the queue.

Incorreto. O Lambda reduz o número de pollers ou de instâncias concorrentes em resposta a throttling (falta de concorrência ou limites de taxa na conta),
e não simplesmente devido a erros de execução dentro do seu código.

❌ If there is an increase in queue size, Lambda decreases the concurrency required to reduce cost.

Incorreto. Se o tamanho/acumulo da fila aumenta, o Lambda aumenta a concorrência (escala horizontalmente) para processar as mensagens mais rápido.

❌ If the visibility timeout expires before the Lambda function processes the messages, messages will be deleted by the queue.

Incorreto. Se o tempo de visibilidade expira antes do processamento terminar, as mensagens voltam a ficar visíveis na fila para serem lidas novamente por outros processos (o que gera reprocessamento ou duplicação). Elas não são deletadas.

---

## 2. A company owns an ecommerce website that sells dog pajamas.
Whenever an order is placed for a product, the company wants to send an email alert to
the customer and at the same time process the order and update the inventory database. After a popular talk show
host talked about the website, the company is having issues processing orders as they come in.
The company often creates duplicate orders for their customers.

Which of the following architectures would meet the company's requirements and create a better customer experience?

01. Use the claim check pattern. Add the order into an Amazon S3 bucket and create a presigned URL. Use an AWS Lambda function to send a message with that URL to
a standard Amazon Simple Queue Service (Amazon SQS) queue. Use Lambda to poll the queue, filter the message, send an email to the customer, and update the database.

02. Use the claim check pattern. Add the order into an Amazon S3 bucket and create a presigned URL. Use an AWS Lambda function to send a message with that URL
to a First-In-First-Out (FIFO) Amazon Simple Queue Service (Amazon SQS) queue. Use Lambda to poll the queue, filter the message, send an email to the customer, and update the database.

03. Use the Amazon Simple Notification Service (Amazon SNS) fan-out pattern. When the order is placed, send a message to the SNS topic, with customer email addresses and
a standard Amazon Simple Queue Service (Amazon SQS) queue as the subscribers. Use AWS Lambda to poll the queue, filter the message, and update the database.

04. Use the Amazon Simple Notification Service (Amazon SNS) fan-out pattern. When the order is placed, send a message to the SNS topic, with customer email
addresses and a First-In-First-Out (FIFO) Amazon Simple Queue Service (Amazon SQS) queue as the subscribers. Use AWS Lambda to poll the queue, filter the message, and update the database.

**A resposta correta é:**

> Use the Amazon Simple Notification Service (Amazon SNS) fan-out pattern. When the order is placed, send a message to the SNS topic, with customer 
> email addresses and a First-In-First-Out (FIFO) Amazon Simple Queue Service (Amazon SQS) queue as the subscribers. Use AWS Lambda to poll the queue,
> filter the message, and update the database.


1. Padrão Fan-Out (Amazon SNS):

- A empresa precisa realizar duas ações simultâneas a partir de um único evento (pedido realizado): enviar o e-mail de confirmação ao cliente e processar
a atualização do estoque no banco de dados.

- O SNS Fan-Out publica a mensagem uma única vez no tópico e distribui a notificação em paralelo para múltiplos inscritos.

2. Fila FIFO do Amazon SQS (Fim dos Pedidos Duplicados):

- O enunciado destaca que a empresa está enfrentando um grande pico de tráfego e gerando pedidos duplicados para os clientes.

- As filas FIFO (First-In-First-Out) do SQS garantem desduplicação exata (Exactly-once processing) e preservam a ordem exata das transações, eliminando
a criação de pedidos duplicados sob alta carga.
