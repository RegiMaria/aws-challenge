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
