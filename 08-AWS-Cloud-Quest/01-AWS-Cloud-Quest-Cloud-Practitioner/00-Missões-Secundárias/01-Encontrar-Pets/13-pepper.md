# 🐧 Pepper - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/4c84fed2-65e0-4184-bec3-a8c373c401cd" width="250" alt="Pepper Pet" />
</p>

- **Espécie:** Pinguim (*Penguin*)
- **Domínio AWS:** Tecnologia e Serviços em Nuvem (*Cloud Technology & Services*)
- **Tópico Principal:** Computação Serverless, Microsserviços e Mensageria (AWS Lambda, API Gateway e Step Functions)
- **Total de Quizzes:** 12
- **Status:** Adotado ✅

---

### Quiz 1: Conceito Fundamental do AWS Lambda
**Enunciado:** Uma empresa deseja executar código de backend para processar imagens carregadas no S3 sem precisar provisionar, gerenciar ou atualizar sistemas operacionais de servidores. A cobrança deve ocorrer apenas pelo tempo exato de execução do código e pela quantidade de solicitações. Qual serviço atende a essa necessidade?

- [ ] **A)** Amazon EC2 com Auto Scaling
- [x] **B)** AWS Lambda
- [ ] **C)** Amazon ECS com instâncias EC2
- [ ] **D)** AWS Elastic Beanstalk

> **Por que esta é a resposta correta?**
> * O **AWS Lambda** é um serviço de computação *serverless* (sem servidor) que permite executar códigos em resposta a eventos sem gerenciar servidores. Você paga apenas pelos milissegundos de computação consumidos e pelo número de execuções.
> * *Incorretas:* EC2 exige gerenciamento de SO e instâncias ligadas cobrando por hora, mesmo ociosas.
>
> 📖 **Documentação:** [O que é o AWS Lambda?](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/welcome.html)

---

### Quiz 2: Limitações e Características de Execução do Lambda
**Enunciado:** Um desenvolvedor criou uma função AWS Lambda para processar relatórios massivos, mas o script falha quando atinge 16 minutos de execução contínua. Qual é o limite máximo de tempo de execução (*timeout*) configurável para uma única invocação de função Lambda?

- [ ] **A)** 60 segundos
- [ ] **B)** 5 minutos
- [ ] **C)** 15 minutos
- [ ] **D)** 2 horas

> **Por que esta é a resposta correta?**
> * O tempo máximo de execução (*timeout*) para uma única invocação do AWS Lambda é de **15 minutos** (900 segundos). Para cargas de trabalho que exigem processamento mais longo, recomenda-se usar o AWS Step Functions ou migrar para o Amazon ECS / EC2.
>
> 📖 **Documentação:** [Limites do AWS Lambda](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/gettingstarted-limits.html)

---

### Quiz 3: Exposição de APIs HTTP/REST (Amazon API Gateway)
**Enunciado:** Como um desenvolvedor pode expor funções AWS Lambda de forma segura como uma API REST pública na internet para ser consumida por aplicativos móveis e sites de clientes, contando com recursos nativos de controle de acesso, limitação de taxa (*throttling*) e cache?

- [ ] **A)** Amazon CloudFront
- [x] **B)** Amazon API Gateway
- [ ] **C)** AWS Direct Connect
- [ ] **D)** Elastic Load Balancer (ELB) clássico

> **Por que esta é a resposta correta?**
> * O **Amazon API Gateway** é um serviço totalmente gerenciado que facilita para desenvolvedores a criação, publicação, manutenção, monitoramento e segurança de APIs em qualquer escala, integrando-se nativamente com o AWS Lambda.
>
> 📖 **Documentação:** [O que é o Amazon API Gateway?](https://docs.aws.amazon.com/pt_br/apigateway/latest/developerguide/welcome.html)

---

### Quiz 4: Orquestração de Fluxos de Trabalho (AWS Step Functions)
**Enunciado:** Uma aplicação de e-commerce precisa coordenar múltiplos passos sequenciais e paralelos baseados em serverless: cobrar o pagamento (Lambda 1), atualizar o inventário (Lambda 2) e enviar um e-mail de confirmação (Lambda 3). Se uma etapa falhar, o fluxo deve realizar reversões compensatórias (*rollback*). Qual serviço gerencia essa lógica de estado visual?

- [ ] **A)** Amazon Simple Queue Service (SQS)
- [x] **B)** AWS Step Functions
- [ ] **C)** Amazon SNS
- [ ] **D)** AWS CloudTrail

> **Por que esta é a resposta correta?**
> * O **AWS Step Functions** permite coordenar componentes de aplicações distribuídas e microsserviços usando fluxos de trabalho visuais baseados em máquinas de estado, gerenciando tratamento de erros, novas tentativas (*retries*) e ramificações complexas.
>
> 📖 **Documentação:** [O que é o AWS Step Functions?](https://docs.aws.amazon.com/pt_br/step-functions/latest/dg/welcome.html)

---

### Quiz 5: Desacoplamento de Mensageria com Filas (Amazon SQS)
**Enunciado:** Um microsserviço de upload envia dados mais rápido do que um banco de dados de destino consegue processar, gerando gargalos e falhas de conexão. Qual serviço de fila gerenciada (*message queue*) deve ser inserido entre os componentes para desacoplar a aplicação e armazenar mensagens temporariamente?

- [ ] **A)** Amazon SNS (Simple Notification Service)
- [x] **B)** Amazon SQS (Simple Queue Service)
- [ ] **C)** AWS Direct Connect
- [ ] **D)** Amazon Route 53

> **Por que esta é a resposta correta?**
> * O **Amazon SQS** é um serviço de enfileiramento de mensagens totalmente gerenciado que permite desacoplar microsserviços, sistemas distribuídos e aplicações sem servidor, garantindo que as mensagens fiquem seguras numa fila até serem processadas.
>
> 📖 **Documentação:** [O que é o Amazon SQS?](https://docs.aws.amazon.com/pt_br/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)

---

### Quiz 6: Notificações Pub/Sub (Amazon SNS)
**Enunciado:** Uma empresa precisa enviar um alerta instantâneo via SMS e e-mail para centenas de assinantes diferentes sempre que um alarme de segurança for disparado na conta AWS. Qual serviço de mensagens baseado no modelo Publish/Subscribe (Pub/Sub) deve ser utilizado?

- [ ] **A)** Amazon SQS
- [x] **B)** Amazon SNS (Simple Notification Service)
- [ ] **C)** AWS CloudTrail
- [ ] **D)** Amazon EventBridge

> **Por que esta é a resposta correta?**
> * O **Amazon SNS** gerencia a entrega e o envio de mensagens para assinantes finais ou endpoints (como e-mail, SMS, funções Lambda ou Webhooks) utilizando o padrão de publicação e assinatura (*Pub/Sub*).
>
> 📖 **Documentação:** [O que é o Amazon SNS?](https://docs.aws.amazon.com/pt_br/sns/latest/dg/welcome.html)

---

### Quiz 7: Processamento Assíncrono e Event-Driven com Lambda
**Enunciado:** Quando um ficheiro PDF é depositado em um bucket Amazon S3, a intenção é disparar uma função AWS Lambda de forma assíncrona para gerar uma miniatura (*thumbnail*). Qual componente da arquitetura armazena os eventos temporariamente caso o Lambda atinja o limite de concorrência?

- [ ] **A)** Fila de eventos interna gerenciada automaticamente pelo Lambda (*Asynchronous Event Queue*)
- [ ] **B)** Tabela do DynamoDB dedicada
- [ ] **C)** Elastic Load Balancer
- [ ] **D)** AWS Storage Gateway

> **Por que esta é a resposta correta?**
> * Inovações assíncronas no Lambda colocam os eventos em uma **fila interna gerenciada pela AWS** de forma automática, tentando novamente a execução da função por até 24 horas em caso de falhas iniciais.
>
> 📖 **Documentação:** [Invocação assíncrona do AWS Lambda](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/invocation-async.html)

---

### Quiz 8: Conectividade de Funções Lambda em VPCs Privadas
**Enunciado:** Uma função AWS Lambda precisa consultar dados sensíveis armazenados em um banco de dados relacional RDS que reside estritamente em uma sub-rede privada de uma Amazon VPC. Como configurar o Lambda para acessar essa base de dados com segurança?

- [ ] **A)** Atribuir um endereço IP público ao banco de dados RDS.
- [x] **B)** Configurar os parâmetros de rede da VPC (`VpcConfig`) na função Lambda para associá-la às sub-redes e grupos de segurança privados.
- [ ] **C)** Instalar um Internet Gateway diretamente no código do Lambda.
- [ ] **D)** Utilizar um balanceador de carga clássico na internet pública.

> **Por que esta é a resposta correta?**
> * Ao configurar as propriedades de **VpcConfig** no Lambda, a função ganha ENIs (Elastic Network Interfaces) que lhe permitem acessar recursos protegidos dentro de uma VPC privada de forma segura.
>
> 📖 **Documentação:** [Configurar o Lambda para acessar recursos em uma VPC](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/configuration-vpc.html)

---

### Quiz 9: Otimização de Custos com Lambda Provisioned Concurrency
**Enunciado:** Uma aplicação serverless sofre com latências esporádicas de inicialização a frio (*Cold Start*) sempre que uma nova requisição chega após um longo período de ociosidade. Qual recurso do Lambda resolve esse problema mantendo as funções prontas para responder instantaneamente?

- [ ] **A)** AWS Lambda Layers
- [x] **B)** Concorrência Provisionada (*Provisioned Concurrency*)
- [ ] **C)** Auto Scaling de EC2
- [ ] **D)** Amazon DynamoDB Accelerator (DAX)

> **Por que esta é a resposta correta?**
> * A **Provisioned Concurrency** inicializa instâncias da sua função Lambda antecipadamente, garantindo que elas estejam sempre prontas para responder a requisições de sub-milissegundo sem o atraso do *Cold Start*.
>
> 📖 **Documentação:** [Concorrência provisionada do AWS Lambda](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/configuration-concurrency.html)

---

### Quiz 10: Compartilhamento de Dependências com Lambda Layers
**Enunciado:** Várias funções AWS Lambda diferentes desenvolvidas pela equipa precisam utilizar o mesmo conjunto pesado de bibliotecas de terceiros e arquivos de utilitários comuns. Qual recurso do Lambda permite gerenciar e carregar esse código compartilhado de forma limpa, sem duplicá-lo em cada pacote de deployment?

- [x] **A)** Camadas do Lambda (*Lambda Layers*)
- [ ] **B)** S3 Object Lock
- [ ] **C)** AWS Systems Manager Parameter Store
- [ ] **D)** VPC Endpoints

> **Por que esta é a resposta correta?**
> * As **Lambda Layers** são arquivos ZIP que contêm bibliotecas, dependências ou customizações. Ao adicionar uma Layer a uma função, o código dela fica acessível no ambiente de execução sem precisar ser empacotado junto com o código principal da função.
>
> 📖 **Documentação:** [Trabalhar com camadas do AWS Lambda](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/configuration-layers.html)

---

### Quiz 11: Integração Assíncrona entre SQS e Lambda (Event Source Mapping)
**Enunciado:** Uma fila Amazon SQS recebe mensagens de pedidos de compra de clientes. Deseja-se que uma função AWS Lambda seja acionada automaticamente para processar os lotes de mensagens assim que eles chegam à fila. Qual mecanismo realiza essa leitura contínua?

- [ ] **A)** CloudWatch Alarms
- [x] **B)** Mapeamento de Origem de Eventos (*Event Source Mapping*)
- [ ] **C)** API Gateway HTTP Proxy
- [ ] **D)** AWS Direct Connect

> **Por que esta é a resposta correta?**
> * O **Event Source Mapping** lê dados de fontes de streaming ou filas (como SQS, DynamoDB Streams e Kinesis) de forma síncrona/polled e invoca a função Lambda para processar os lotes de mensagens.
>
> 📖 **Documentação:** [Mapeamento de origem de eventos do AWS Lambda](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/invocation-eventsourcemapping.html)

---

### Quiz 12: Tratamento de Mensagens com Falha (SQS Dead-Letter Queue - DLQ)
**Enunciado:** Algumas mensagens malformadas inseridas em uma fila SQS causam falhas repetidas no processamento do Lambda. Para evitar que essas mensagens fiquem presas em loop infinito bloqueando a fila principal, para onde elas devem ser redirecionadas após esgotarem o limite de tentativas (*MaxReceiveCount*)?

- [ ] **A)** Para o Amazon S3 Glacier
- [x] **B)** Para uma Fila de Erros (*Dead-Letter Queue - DLQ*)
- [ ] **C)** Para a tabela principal do CloudTrail
- [ ] **D)** Para o Internet Gateway da VPC

> **Por que esta é a resposta correta?**
> * Uma **Dead-Letter Queue (DLQ)** é uma fila secundária para a qual o SQS encaminha mensagens que não puderam ser processadas com sucesso após um número máximo de tentativas configurado, permitindo isolar os erros para investigação posterior sem interromper o fluxo normal.
>
> 📖 **Documentação:** [Usar filas de mensagens não entregues do Amazon SQS (DLQ)](https://docs.aws.amazon.com/pt_br/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html)
