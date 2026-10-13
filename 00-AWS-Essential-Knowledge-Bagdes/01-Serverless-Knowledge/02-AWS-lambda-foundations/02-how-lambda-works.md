# 02 - How AWS Lambda Works: Modelos de Invocação

Para entender a arquitetura orientada a eventos do AWS Lambda, 
é essencial compreender como as fontes de eventos (event sources) iniciam as funções para executar o código.

Existem 3 modelos principais de invocação (Invocation Models), cada um atendendo a necessidades específicas de aplicação e desenvolvimento.

## 1. Invocação Síncrona (Synchronous Invocation)

Na invocação síncrona, a fonte do evento chama a função Lambda e aguarda imediatamente pela resposta.

- Como funciona: O Lambda envia o evento diretamente para a função e retorna a resposta do código 
(junto com metadados, como a versão executada) diretamente para quem fez a chamada.

- Tratamento de Erros: Não possui retentativas (retries) embutidas. 
A estratégia de tentar novamente deve ser implementada manualmente no código da aplicação do cliente.

- Exemplo de fluxo: O cliente faz uma requisição HTTP via API Gateway--> O API Gateway chama o Lambda --> O Lambda processa e devolve a resposta para o cliente na mesma hora.


Serviços AWS Síncronos:
- Amazon API Gateway

- Amazon Cognito

- AWS CloudFormation

- Amazon Alexa

- Amazon Lex

- Amazon CloudFront

---

## 2. Invocação Assíncrona (Asynchronous Invocation)

Na invocação assíncrona, os eventos são colocados em uma **fila interna** e o solicitante não espera a função terminar de rodar.

- Como funciona: O Lambda recebe o evento, confirma o recebimento para o chamador e processa a função em segundo plano.
É ideal para cenários onde o cliente não precisa de uma resposta imediata.

- Recurso de Destinos (Destinations): Permite enviar o registro do resultado de invocações assíncronas diretamente para outro
serviços sem precisar escrever código adicional:

  -- Destino de Sucesso: Envia dados quando a função processa com êxito (ex: para um barramento do Amazon EventBridge).

  -- Destino de Falha: Envia registros quando todas as tentativas de processamento falham (ex: para uma fila do Amazon SQS ou SNS).

Serviços AWS Assíncronos:
- Amazon SNS

- Amazon S3

- Amazon EventBridge
