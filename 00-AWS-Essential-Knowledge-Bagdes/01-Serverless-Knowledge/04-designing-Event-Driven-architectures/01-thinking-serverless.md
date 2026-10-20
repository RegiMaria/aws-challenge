# Thinking Serverless: Arquiteturas Orientadas a Eventos

Este módulo apresenta a evolução arquitetural em direção aos microserviços e arquiteturas
orientadas a eventos (Event-Driven Architectures - EDA), destacando a importância das
conexões assíncronas e o papel do AWS Lambda junto a outros serviços gerenciados.

## 1. Abstração de Infraestrutura e Evolução para Microserviços

* *Conceito de Serverless:* Abstrai a infraestrutura de computação ao ponto de eliminar qualquer
responsabilidade sobre o gerenciamento dos servidores onde o código roda.

* *Sem custo ocioso:* Quando a aplicação não está em execução, não há custos com servidores parados.

* *Flexibilidade e Agilidade:* Reflete uma evolução na indústria em direção a microserviços desacoplados, 
permitindo lançar novas funcionalidades e escalar com muito mais agilidade.   

## 2. O que são Arquiteturas Orientadas a Eventos (EDA)?

Em uma arquitetura orientada a eventos, os microserviços utilizam eventos para se
comunicarem e invocarem uns aos outros de forma totalmente desacoplada e agnóstica de tecnologia.

* *O que é um evento?* É uma simples mudança ou atualização de estado
(ex: um item adicionado ao carrinho de compras de um e-commerce).

* *Desacoplamento:* O estado e o código são separados; a integração entre
os componentes é feita tipicamente via mensageria assíncrona.

* *Padrões Consagrados:* Padrões de integração orientados a eventos foram documentados em
2003 no livro "Enterprise Integration Patterns". No entanto, antigamente eram caros de
construir e gerar acoplamento (lock-in). Com a nuvem Serverless, esses padrões podem
ser implementados com custos e sobrecarga operacional mínimos.

## 3. O Ecossistema Serverless (além do Lambda)
O AWS Lambda é naturalmente orientado a eventos - ele executa em resposta a uma fonte de evento,
recebe os dados via objeto de evento (event payload) e pode produzir novos eventos para outros serviços consumirem.

o entanto, aplicações Serverless não são compostas apenas por funções Lambda. O ecossistema combina o Lambda
com diversos outros serviços gerenciados:

| **Serviço AWS** | **Função na Arquitetura Serverless** |
|---|---|
| AWS Lambda | Processa a lógica de negócio principal. |
| Amazon SQS / SNS / Kinesis | Mensageria, filas e processamento de fluxos de dados (*streaming*) para integrações assíncronas. |
| Amazon API Gateway | Gerenciamento de portas de entrada e saída HTTP e APIs REST. |
| Amazon DynamoDB | Banco de dados NoSQL gerenciado para persistência de dados. |
| AWS Step Functions | Orquestração de fluxos de trabalho e chamadas de funções. |


> Objetivo: Permitir que as funções Lambda fiquem focadas estritamente na lógica
> de negócio, enquanto os serviços gerenciados da AWS assumem o "trabalho pesado não diferenciado"
>  (undifferentiated heavy lifting) na integração entre os componentes.

## 4. Padrão Serverless Simples (Comunicação Síncrona)

Um padrão simples para processar pedidos em um fluxo de e-commerce é a RESTful Microservice clássica:

```
 ┌──────────┐ HTTP  ┌─────────────────┐       ┌───────────────┐       ┌─────────────────┐
 │ Cliente  │ ────► │ Amazon API      │ ────► │ AWS Lambda    │ ────► │ Amazon          │
 │          │       │ Gateway         │       │ (Lógica)      │       │ DynamoDB        │
 └──────────┘       └─────────────────┘       └───────────────┘       └─────────────────┘

 ```
1. A informação do pedido chega via chamada API sobre HTTP
2. O Amazon API Gateway gerencia as requisições e respostas da API.
3. O AWS Lambda executa a lógica de negócio para validar e processar a chamada
4. O Amazon DynamoDB armazena os dados do pedido de forma persistente.

## O Desafio das APIs Síncronas

Embora este padrão seja simples de implementar, APIs síncronas em arquiteturas
serverless trazem desafios: aumentam o tempo de espera do usuário, criam acoplamento
rígido entre os serviços e elevam o risco de timeouts ou acúmulo de requisições pendentes.
Por isso, para criar sistemas altamente resilientes e escaláveis, o ideal é evoluir do padrão 
síncrono para padrões assíncronos.
