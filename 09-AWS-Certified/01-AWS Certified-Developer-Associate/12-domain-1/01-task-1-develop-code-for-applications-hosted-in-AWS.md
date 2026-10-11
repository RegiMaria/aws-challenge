# 📚 Guia de Estudos DVA-C02: Domínio 1 - Task 1.1 (Develop Code for Applications Hosted in AWS)

---

## 1. Fundamentos & Well-Architected Framework

O foco principal para o desenvolvimento de aplicações na AWS está no **Pilar de Excelência Operacional** (*Operational Excellence Pillar*).

* **Infrastructure as Code (IaC):** Aplique a mesma disciplina usada para o código da aplicação à sua infraestrutura.
Automatize a execução usando eventos para disparar deployments e compartilhe padrões entre equipes.
* **Prática de Falhas e Experimentação:** A nuvem permite testar procedimentos operacionais, simular falhas e criar ambientes de testes seguros sem alto custo inicial.
* **Ferramentas de IaC para Desenvolvedores:**
  * **AWS CloudFormation:** Provisionamento declarativo de toda a infraestrutura AWS via arquivos YAML/JSON.
  * **AWS SAM (Serverless Application Model):** Extensão do CloudFormation simplificada para definir aplicações serverless (Lambda, API Gateway, DynamoDB) em poucas linhas de código.

---

## 2. Arquiteturas Orientadas a Eventos (Event-Driven) & Desacoplamento

Uma arquitetura orientada a eventos é baseada no **desacoplamento fraco** (*loose coupling*) 
dos componentes, promovendo resiliência, escalabilidade e agilidade.

### Tightly-Coupled vs. Loosely-Coupled
* **Tightly-Coupled (Fortemente Acoplado):** Componentes dependem diretamente da disponibilidade e
resposta imediata dos outros (ex.: chamadas síncronas diretas). Se um falhar, o sistema inteiro falha.
* **Loosely-Coupled (Fracamente Acoplado):** Componentes se comunicam por meio de intermediários (filas, barramentos de eventos).
Se um componente falhar, os eventos ficam retidos no intermediário para processamento posterior.

### Serviços Essenciais para Event-Driven
* **Amazon SQS:** Fila de mensagens para desacoplamento e buffer.
* **Amazon SNS:** Pub/Sub para envio de mensagens *fan-out* para múltiplos assinantes.
* **Amazon EventBridge:** Barramento de eventos para rotear eventos entre serviços AWS, SaaS e aplicações próprias.
* **AWS Lambda:** Executa código em resposta a eventos (funciona como fonte de evento ou consumidor).

---

## 3. Resiliência, Tratamento de Erros e Padrões de Mensageria

### Exponential Backoff com Jitter
* **O problema:** Reagendar chamadas imediatamente (*immediate retry*) após um erro pode sobrecarregar ainda mais um sistema já saturado (*congestão*).
* **Exponential Backoff:** Aumenta exponencialmente o tempo de espera entre tentativas consecutiveis de erro (ex.: 1s, 2s, 4s, 8s...).
* **Jitter:** Adiciona uma variação aleatória ao tempo de espera para evitar que todas as instâncias concorrentes tentem reconectar
exatamente no mesmo milissegundo (*thundering herd problem*).
* **SDKs AWS:** Toda essa lógica de *Retry + Exponential Backoff + Jitter* já vem pré-configurada por padrão nos SDKs oficiais da AWS.

### Tratamento de Falhas em Mensagens
* **Dead-Letter Queues (DLQ):** Filas secundárias usadas pelo SQS, SNS ou Lambda para armazenar mensagens
que não puderam ser processadas após um número máximo de tentativas (*maxReceiveCount*).
* **Idempotência em Lambdas:** Projetar a lógica para processar o mesmo evento múltiplas vezes sem causar
duplicidade de dados ou efeitos colaterais.

---

## 4. Orquestração vs. Coreografia (Step Functions vs. EventBridge)

### AWS Step Functions (Orquestração)
* **Quando usar:** Workflows complexos com lógica de ramificação (*branching logic*), tratamento fino de erros,
retentativas e execução longa (até 1 ano).
* **Vantagens:** Mantém o estado da execução (*state machine*), oferece suporte a versões e reduz a necessidade
de código customizado para gerenciar tentativas e timeouts.

### Amazon EventBridge (Coreografia)
* **Quando usar:** Coordenação de mudanças de estado entre múltiplos microserviços/sistemas heterogêneos de forma reativa e baseada em eventos.

### Padrão Fan-Out
* **Conceito:** Publicar uma única mensagem em um tópico Amazon SNS que é distribuída automaticamente para múltiplos consumidores
(ex.: filas SQS, Kinesis Data Firehose, Lambda).
* **Caso de Uso:** Duplicar dados de produção para ambientes de teste e validação em tempo real.

---

## 5. Padrões de Comunicação: Síncrono vs. Assíncrono e Estado

| Conceito | Características | Exemplo na AWS |
| :--- | :--- | :--- |
| **Síncrono** | Espera pela resposta imediata. Se o serviço falhar, os dados podem ser perdidos a menos que persistidos antes. | REST APIs via Amazon API Gateway |
| **Assíncrono** | O produtor envia o evento e continua a execução. Mensagens retidas em fila garantem durabilidade. | Amazon SQS, Amazon SNS |
| **Stateless** | Servidores não guardam sessão. Altamente escalável por meio de containers/load balancers. | Containers no Amazon ECS / Fargate |
| **Stateful** | Guarda sessão localmente. Requer direcionamento de tráfego (*sticky sessions*) ou persistência em banco. | Aplicações legadas ou bancos |

>  **Pegadinha da Prova (ECS Task Definition):** Para permitir que containers rodando no Amazon ECS acessem portas da instância hospedeira (*port mapping*),
> a configuração deve ser feita na **Task Definition** (no bloco `container definitions`).

---

## 6. Integração com Amazon API Gateway

O API Gateway é fundamental na prova. Você deve conhecer o fluxo das requisições e seus componentes:

* **Method Request:** Define quais parâmetros de consulta (*query string*), cabeçalhos ou corpo são **obrigatórios** na requisição do cliente.
* **Integration Request:** Mapeia a requisição para o backend (ex.: função Lambda) usando *Mapping Templates* (VTL).
* **Integration Response:** Recebe a resposta do backend e mapeia para a resposta do cliente.
* **Method Response:** Define os códigos de status HTTP (200, 400, 500) e os tipos de dados retornados ao cliente.

### Recursos Importantes para o Exame
* **Stage Variables:** Variáveis usadas no API Gateway para apontar para diferentes ambientes/backends (ex.: `dev`, `prod`).
* **Usage Plans & API Keys:** Controle de acesso por cliente, aplicação de limites de taxa (*throttling*) e cotas.

---

## 7. Streaming de Dados em Tempo Real

Serviços gerenciados para ingestão e processamento contínuo de dados em larga escala:

* **Amazon Kinesis Data Streams:** Processamento contínuo com retenção ajustável e ordenação por shard.
* **Amazon Data Firehose:** Ingestão *near real-time* que carrega dados automaticamente no Amazon S3, Redshift ou OpenSearch.
* **Amazon MSK:** Apache Kafka gerenciado pela AWS.

---

## 8. Testes, Deploy e Ferramentas do Desenvolvedor

* **AWS SDKs:** Reduzem drasticamente a complexidade de autenticação, retentativas automáticas e tratamento de erros.
* **AWS SAM CLI:** Permite testar e depurar funções Lambda e APIs localmente (`sam local start-api`).
* **AWS CodeBuild:** Compila código, executa testes unitários e empacota artefatos (`buildspec.yml`).

---

## Resoluções dos Cenários Práticos Mencionados no Conteúdo

### ❓ Cenário 1: Sincronização de Dados de Usuário entre Dispositivos
* **Problema:** Sincronizar perfis de usuários em múltiplos dispositivos (mobile/web) sem necessidade de gerenciar um backend complexo.
* **Solução:** **Amazon Cognito Sync** (ou AWS AppSync com Cognito).

### ❓ Cenário 2: Erros de Throttling e Hot Partition no DynamoDB
* **Problema:** Erros de *throttling* ocorrem devido a acesso desproporcional a uma única partição (*hot partition*), mesmo que a capacidade total provisionada não tenha sido excedida.
* **Solução:**
  * **A curto prazo / Aplicação:** Implementar *retries* com **exponential backoff**.
  * **Arquitetura / Otimização:** Otimizar a chave de partição (*Partition Key*), aplicar *sharding* (adicionar um sufixo aleatório à chave) ou adicionar o **DynamoDB Accelerator (DAX)** para cache de leitura.

### ❓ Cenário 3: Configuração de Porta no Amazon ECS
* **Problema:** Permitir que containers acessem portas da instância hospedeira para tráfego via *port mapping*.
* **Solução:** Configurar o *port mapping* dentro da **Task Definition**.
