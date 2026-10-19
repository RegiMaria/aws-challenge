# Módulo 3: AWS Lambda Function Permissions

Este módulo aborda a segurança e o controle de acesso no AWS Lambda,
detalhando a diferença fundamental entre as permissões de
**invocação** da função e as permissões de **execução** sobre outros serviços AWS.

## 1. Os Dois Lados da Segurança no Lambda
Como o Lambda é totalmente integrado ao AWS IAM (Identity and Access Management),
o modelo de segurança é dividido em dois papéis distintos:

```
 ┌─────────────────────────────────────────────────────────────┐
 │ 1. RESOURCE-BASED POLICY (Política Baseada em Recurso)      │
 │    Quem tem permissão de INVOCAR a função Lambda?           │
 │    (Ex: Amazon S3, Amazon API Gateway, outra conta AWS)     │
 └──────────────────────────────┬──────────────────────────────┘
                                │
                                ▼
 ┌─────────────────────────────────────────────────────────────┐
 │ 2. IAM EXECUTION ROLE (Papel de Execução IAM)               │
 │    O que a função Lambda tem permissão de FAZER?           │
 │    (Ex: Inserir item no DynamoDB, ler do S3, gravar logs)   │
 └─────────────────────────────────────────────────────────────┘
```

**Princípio do Menor Privilégio (Principle of Least Privilege):**
Sempre comece com o conjunto de permissões mais restritivo possível e adicione novas permissões
somente quando estritamente necessário. O IAM Access Analyzer pode analisar os logs do AWS CloudTrail
e gerar automaticamente um modelo de política com apenas as permissões que a função realmente utilizou
em determinado período.

---

## 2. IAM Execution Role (Papel de Execução)

O Execution Role concede à função as permissões necessárias para interagir com outros serviços da AWS.
Quando a função é disparada, o serviço Lambda assume essa role.

- **Trust Policy (Política de Confiança):** Toda Execution Role precisa ter uma política de confiança permitindo
que o serviço principal do Lambda (lambda.amazonaws.com) utilize o serviço AWS STS para assumir a role (sts:AssumeRole).

- **Configuração em Tempo de Execução:** Você pode adicionar, remover permissões ou alterar a Execution Role da sua função
a qualquer momento no console ou via código/IaC.

Exemplo de Estrutura de uma Execution Role:

**1. Política IAM (Ações permitidas):**

```JSON
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "dynamodb:PutItem",
      "Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/test"
    }
  ]
}
```
> Permite gravar itens na tabela test do DynamoDB.

** 2. Trust Policy (Permissão para a AWS assumir a role):**

```JSON

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}

```
> Permite que o Lambda assuma a role para rodar o código

---
## 3. Resource-Based Policy (Política Baseada em Recurso)

A Resource-Based Policy (também conhecida como Function Policy) define quais *principals*
(usuários, roles, serviços AWS ou outras contas AWS) têm **autorização** para invocar a função Lambda.

- **Modelos de Invocação Push:** Geralmente é criada automaticamente quando você adiciona um gatilho de origem (trigger) como Amazon API Gateway ou Amazon S3.

- **Acesso Multi-Conta (Cross-Account):** Facilita a concessão de acesso a funções residentes em outras contas AWS.

- **Limite de Tamanho:** As Resource Policies possuem um limite físico de tamanho em bytes.
Se muitas contas precisarem disparar a mesma função e estourarem o limite, deve-se transitar para o uso de IAM Roles com permissão de `sts:AssumeRole`.

Exemplo de Resource-Based Policy (Permissão para o Amazon S3):

```JSON
{
  "Version": "2012-10-17",
  "Id": "default",
  "Statement": [
    {
      "Sid": "s3-invoke-permission",
      "Effect": "Allow",
      "Principal": {
        "Service": "s3.amazonaws.com"
      },
      "Action": "lambda:InvokeFunction",
      "Resource": "arn:aws:lambda:us-west-2:123456789012:function:my-s3-function"
    }
  ]
}

```
> Concede ao serviço Amazon S3 autorização para executar a ação lambda:InvokeFunction na função `my-s3-function`

## Tabela Comparativa: Resource Policy vs. Execution Role

| **Característica** | **Resource-based Policy** | **IAM Execution Role** |
|---|---|---|
| **Foco principal** | Permissão para **INVOCAR** a função. | Permissão para a função **ATUAR** em outros serviços. |
| **Criação / Associação** | Criada ao adicionar gatilhos (*triggers*) ou políticas personalizadas. | Criada ou selecionada no momento em que a função Lambda é criada. |
| **Evento associado** | Fontes do tipo *Push* (ex.: API Gateway, S3, SNS). | Fontes do tipo *Poll* ou ações internas do código (ex.: DynamoDB, SQS). |
| **Ação padrão** | `lambda:InvokeFunction` | Ações de SDK/serviços (ex.: `s3:GetObject`, `dynamodb:PutItem`). |
| **Requisito do Criador** | Requer permissões de gerenciamento na função. | Requer que o criador possua a permissão `iam:PassRole`. |

---

## 4. Acesso a Recursos dentro de uma VPC e Segurança Privada

Por padrão, as funções Lambda são executadas em uma rede de sistema VPC isolada pela AWS.
Se a sua função precisar acessar recursos internos de uma rede privada (como instâncias RDS, ElastiCache ou APIs privadas):

**A. Configuração de VPC:**
1. É necessário configurar os Subnet IDs e os Security Group IDs na função Lambda.

2. A Execution Role precisa ter a política gerenciada `AWSLambdaVPCAccessExecutionRole`,
que concede permissão para criar, descrever e excluir Interfaces de Rede Elásticas
(ENIs - Elastic Network Interfaces) dentro da sua VPC.

**B. AWS PrivateLink (Interface VPC Endpoints)**

Para conectar instâncias da sua VPC privada às APIs do Lambda de forma segura sem tráfego pela internet:

- Cria um Interface VPC Endpoint pelo AWS PrivateLink.

- Não necessita de Internet Gateway, NAT Gateway, VPN ou Direct Connect.

- O tráfego de rede nunca sai da rede privada da AWS.





