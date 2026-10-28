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
