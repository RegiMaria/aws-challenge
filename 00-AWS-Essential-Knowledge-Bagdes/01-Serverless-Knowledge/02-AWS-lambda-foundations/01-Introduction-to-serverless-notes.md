# Tema 01: Introduction to Serverless

## 1. Comceito e abstração servless

Um dos maiores benefícios da computação em nuvem é a capacidade de abstrair (ocultar)
a camada de infraestrutura, eliminando a necessidade de gerenciar o hardware físico subjacente.

Em um ambiente serverless:

- Não existem instâncias, sistemas operacionais ou servidores virtuais para você gerenciar.

- A AWS cuida de tudo o que é necessário para executar e escalar a sua aplicação.

- Os desenvolvedores focam 100% no código que torna o negócio único.

---

## 2. Tarefas operacionais e de implantação: Tradicional vs. Serverless

A abordagem serverless reduz a carga operacional (overhead), permitindo focar, experimentar e inovar muito mais rápido.

| **Tarefa de Implantação e Operação** | **Ambiente Tradicional** | **Ambiente Serverless** |
| -------------------------------------------- | ------------------------ | ----------------------- |
| Configurar uma instância | **SIM** | **NÃO** |
| Atualizar o Sistema Operacional (OS) | **SIM** | **NÃO** |
| Instalar plataforma da aplicação | **SIM** | **NÃO** |
| Construir e implantar aplicações | **SIM** | **SIM** |
| Configurar Auto Scaling e Load Balancing | **SIM** | **NÃO** |
| Segurar e monitorar instâncias continuamente | **SIM** | **NÃO** |
| Monitorar e manter as aplicações | **SIM** | **SIM** |

---

## 3. A Plataforma AWS Serverless

A plataforma é composta por um conjunto de serviços totalmente
gerenciados que se integram perfeitamente com o AWS Lambda.
| **Categoria do Serviço** | **Serviços AWS** |
| ------------------------------------------ | ----------------------------------------------------------------------- |
| **Computação (*Compute*)** | AWS Lambda, Lambda@Edge |
| **Orquestração (*Orchestration*)** | AWS Step Functions |
| **Armazenamento (*Storage*)** | Amazon S3 |
| **Bancos de Dados (*Data Stores*)** | Amazon DynamoDB |
| **Barramento de Eventos (*Event Bus*)** | Amazon EventBridge |
| **Mensageria (*Interprocess Messaging*)** | Amazon SNS, Amazon SQS |
| **Integração de API (*API Integration*)** | Amazon API Gateway, AWS AppSync |
| **Ferramentas de Dev (*Developer Tools*)** | AWS Cloud Development Kit (CDK), AWS Serverless Application Model (SAM) |

---

## 4. O que é o AWS Lambda?
O AWS Lambda é um serviço de computação serverless orientado a eventos.
Ele executa o seu código em uma infraestrutura de alta disponibilidade 
e gerencia todos os recursos subjacentes (manutenção do servidor, provisionamento
de capacidade, escalonamento automático, monitoramento e logs).

Principais Benefícios:

- Executa código sem a necessidade de provisionar ou manter servidores.

- Inicia funções automaticamente em resposta a eventos.

- Escala de forma automática (de zero a milhares de requisições).

- Fornece monitoramento e logs integrados por meio do Amazon CloudWatch.

---

## 5. As 6 Principais Funcionalidades (Features) do AWS Lambda

1. Traga seu próprio código (Bring your own code):
Escreva em linguagens que você já conhece (Node.js, Python, Java, C#, Go).
O código não fica acoplado à AWS, facilitando a portabilidade.

2. Integra e estende outros serviços AWS: A função pode invocar SDKs da AWS ou
chamar APIs de terceiros na nuvem, no seu data center ou na internet.

3. Modelo flexível de recursos e concorrência: Você configura apenas a memória
RAM necessária, e a AWS ajusta proporcionalmente a CPU, rede e taxa de E/S (I/O).

4. Modelo de permissões flexível: Utiliza o AWS IAM para conceder acessos granulares
e seguros aos recursos necessários e controlar quem pode invocar a função.

5. Alta disponibilidade e tolerância a falhas nativas: Por ser totalmente gerenciado,
o serviço já é tolerante a falhas por padrão em múltiplas zonas sem necessidade de configuração extra.

6. Pagamento por valor/uso (Pay for value): Você só paga pelo tempo de computação consumido enquanto
o código é executado, faturado em incrementos de 1 milissegundo (ms). Se a função não é invocada, o custo é zero.

---

## 6. Arquiteturas Orientadas a Eventos (Event-Driven Architectures)

Em uma arquitetura orientada a eventos, as **ações e comunicações entre serviços desacoplados
são iniciadas por eventos** (mudança de estado, requisição de usuário ou atualização, como colocar
um item no carrinho de compras).

- Os eventos são observáveis (ex: uma nova mensagem num log) em vez de dirigidos (ex: um comando direto para fazer algo).

- O AWS Lambda é o coração desse modelo: a maioria dos serviços da AWS gera eventos que atuam como fontes (event sources).
O Lambda executa o código customizado em resposta a esses eventos e pode disparar novas ações em seguida.
