<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" width="220" alt="AWS Logo"/>

#  03 - Managed Services Connected by Functions

</div>

Neste capítulo (Managed Services Connected by Functions),
aprendemos que a arquitetura serverless vai muito além de apenas rodar funções isoladas no AWS Lambda:
trata-se de uma mudança de paradigma no design de sistemas, substituindo aplicações monolíticas
por um ecossistema de serviços totalmente gerenciados e orientados a evento.

Aqui estão os pontos centrais e aprendizados deste tópico:

**1. O Ecossistema Serverless (Além do Lambda)**

O AWS Lambda é o "cérebro" que executa o código, mas uma arquitetura serverless completa
utiliza serviços gerenciados especializados para cuidar das camadas fundamentais da aplicação:

- Gerenciamento de APIs: Amazon API Gateway (cuida de REST APIs, autenticação, autorização, controle de taxa/throttling).

- Bancos de Dados e Armazenamento: Amazon DynamoDB (banco NoSQL gerenciado) e Amazon S3 (armazenamento de objetos).

- Mensageria e Processamento de Fluxos (Streaming): Amazon Kinesis Data Streams, Amazon SNS (notificações) e Amazon SQS (filas).

- Orquestração de Fluxos Complexos: AWS Step Functions (combina múltiplas funções Lambda sem necessidade de
escrever código manual para gerenciar paralelismo, erros, tempos limite ou tentativas de reexecução).

- Automação e Deploy: Ferramentas de desenvolvedor da AWS (AWS Developer Tools) para integração e entrega contínuas (CI/CD).

---

**2. A Mudança do Monolito para "Serviços Gerenciados Conectados por Funções"**

- No modelo tradicional (Monolítico ou baseado em servidores): Você é responsável por tudo - desde a aplicação monolítica
até a implantação, configuração e manutenção dos clusters de banco de dados e servidores de backend.

- No modelo Serverless: Você atribui as preocupações da infraestrutura aos serviços gerenciados da AWS e utiliza funções
Lambda desacopladas apenas como "cola" (glue code) para conectar esses serviços e aplicar a sua lógica de negócio específica.

---

**3. Design Orientado a Eventos (Event-Driven Architecture)**

Para que o serverless funcione de forma eficiente, a mentalidade de desenvolvimento precisa mudar:

- Abordagem Tradicional: Foca em "quais dados estou armazenando e quais operações preciso executar sobre eles".

- Abordagem Serverless (Orientada a Eventos): Foca em "quais eventos devem disparar ações no meu sistema".

Nesta abordagem, requisições HTTP de entrada não são os únicos eventos: qualquer alteração de estado vira um evento.
Por exemplo, quando um registro é alterado no DynamoDB ou um arquivo é enviado para o S3, essa mudança gera um evento
que dispara automaticamente uma função Lambda, permitindo criar sistemas desacoplados e altamente reativos.

## Event-oriented Architecture

<p align="center">
  <img width="800" height="350" alt="Image" src="https://github.com/user-attachments/assets/943590df-fd9e-4272-a9a8-0b5b308b6564" />
</p>

---

**4. Fontes Nativas de Eventos (Event Sources)**
O AWS Lambda possui suporte nativo para ser disparado por uma ampla variedade de serviços,
incluindo:

- Armazenamento e Bancos de Dados: S3, DynamoDB.

- Repositórios e Infraestrutura: CloudFormation, CodeCommit.

- Serviços de Dispositivos e Voz: Alexa, AWS IoT Message Broker.

- Chamadas diretas via AWS SDK ou CLI para integrar com qualquer evento customizado da sua empresa.

---

Resumo do Aprendizado

Aprendemos que o verdadeiro poder do serverless é atingido quando deslegamos a **infraestrutura
e o gerenciamento de estado** para serviços gerenciados pela AWS e usamos o AWS Lambda como uma
ponte orientada a eventos, mantendo o código focado exclusivamente na regra de negócio.
