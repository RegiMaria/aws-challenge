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

