# Cronograma de estudos práticos: Developing on AWS (DVA-C02)

## 🟢 Semana 1: Configuração do Ambiente, Segurança & Armazenamento (S3)
Baseado nos Módulos 1, 2, 3, 4, 5 e 6 do ementário (Lab 1 e Lab 2)

1. Teoria para Estudar:

- Ambiente & SDKs: Como configurar o AWS CLI v2 e os SDKs (Python/Boto3 ou Node.js). Entender o arquivo ~/.aws/credentials e perfis de autenticação (--profile).

- IAM para Desenvolvedores: Roles de execução (Execution Roles), Princípio do Menor Privilégio, sintaxe JSON de IAM Policies, credenciais temporárias com AWS STS.

- Amazon S3 Básico e Avançado: Buckets, Objetos, Metadata (x-amz-meta-*), Criptografia (SSE-S3, SSE-KMS, SSE-C), Presigned URLs, CORS, Hospedagem de Site Estático, Multipart Upload (>5GB) e exceções comuns do SDK (NoSuchBucket, AccessDenied).

🛠️ Projeto Prático Semanal (Lab 1 + Lab 2 Replicado):

1. Configuração Local: Instale o AWS CLI na sua máquina (ou Ubuntu VM) e configure o profile de desenvolvedor.

2. Automação S3 via SDK (Python/Boto3):

- Escreva um script em Python que crie um bucket S3 com bloqueio de acesso público habilitado.

- Faça upload de uma imagem com metadados personalizados agregados.

- Gere uma Presigned URL com expiração de 15 minutos para permitir download seguro sem expor o bucket.

3. Site Estático + Policy: Configure o bucket para Static Website Hosting, envie os arquivos da aplicação web e aplique a Bucket Policy permitindo acesso de leitura do site.

## 🔵 Semana 2: Persistência de Dados em Larga Escala (Amazon DynamoDB)
Baseado nos Módulos 7 e 8 do ementário (Lab 3)

1. Teoria para Estudar:
- Conceitos de DynamoDB: Partition Key (HASH), Sort Key (RANGE), tipos de dados NoSQL.

- Operações de Leitura/Escrita: GetItem, PutItem, UpdateItem (com Conditional Expressions), DeleteItem, Query vs. Scan (e por que evitar Scan em produção), Paginação (LastEvaluatedKey).

- Índices Secundários: LSI (Local Secondary Index) vs. GSI (Global Secondary Index).

- Consultas com PartiQL: Acessar dados NoSQL usando sintaxe estilo SQL.

- Otimização & Caching: DynamoDB Accelerator (DAX) para resposta em microssegundos.

🛠️ Projeto Prático Semanal (Lab 3 Replicado):
1. Criação de Tabela NoSQL: Crie uma tabela Produtos com CategoriaId (Partition Key) e ProdutoId (Sort Key).

2. Carga em Lote (Batch): Escreva um script Python/Boto3 que leia um arquivo dados.json e faça o carregamento dos itens na tabela usando BatchWriteItem.

3. CRUD com Expressões Condicionais:

- Escreva um script que atualize o preço do produto apenas se o preço atual for maior que R$ 50,00 (ConditionalExpression).

- Crie uma consulta (Query) usando expressões de filtro (FilterExpression) e simule paginação de resultados.

## 🟡 Semana 3: Lógica de Aplicação (Lambda) & Gerenciamento de APIs (API Gateway)
Baseado nos Módulos 9 e 10 do ementário (Lab 4 e Lab 5)

1. Teoria para Estudar:
- AWS Lambda: Estrutura do handler(event, context), Variáveis de Ambiente, Roles de
execução, limites de timeout (15 min) e memória (128MB a 10GB), empacotamento em arquivos .zip e Lambda Layers.

- Amazon API Gateway:

    - Arquitetura REST API vs. HTTP API.

    - Conceito de Proxy Integration vs. Non-Proxy Integration.

    - Estágios (Stages) e Stage Variables.

    - Configuração de CORS (Cross-Origin Resource Sharing) para chamadas do front-end.

    - Mapping Templates (VTL - Velocity Template Language) para transformação de payload.

🛠️ Projeto Prático Semanal (Lab 4 + Lab 5 Replicado):
 1. API de Upload Seguro:

Crie uma função Lambda em Python que gere uma S3 Presigned URL para upload.

2. Exposição REST via API Gateway:

- Crie uma REST API no API Gateway apontando para essa função Lambda.

- Habilite o CORS na API.

- Implante a API em um estágio chamado dev.

- Teste os endpoints usando o Postman ou o comando curl e valide o retorno do JSON.

## 🔴 Semana 4: Arquiteturas Modernas, Autenticação (Cognito), Deploy (SAM) & Observabilidade (X-Ray)
Baseado nos Módulos 11, 12, 13 e 14 do ementário (Lab 6 Capstone e Lab 7)

1. Teoria para Estudar:
- AWS Step Functions: Orquestração de workflows com máquinas de estado JSON/ASL para conectar microserviços.

- Amazon Cognito:

User Pools: Cadastro, login de usuários e emissão de tokens JWT (ID Token, Access Token, Refresh Token).

Integração do Cognito como Authorizer oficial no API Gateway.

- AWS SAM (Serverless Application Model): Estrutura de arquivo template.yaml, comandos sam build, sam local start-api e sam deploy.

- Observabilidade (AWS X-Ray & CloudWatch):

Diferença entre monitoramento (métricas/logs) e observabilidade (rastreamento distribuído / tracing).

X-Ray SDK, Service Maps, Subsegments, Annotations e Metadata.

🛠️ Projeto Prático Semanal (Lab 6 Capstone + Lab 7 Replicado):
1. Autenticação de Usuários:

- Crie um Cognito User Pool e cadastre um usuário via AWS CLI.

- Adicione o Cognito como Authorizer nas rotas do seu API Gateway criado na Semana 3.

- Teste a rota protegida enviando o token JWT no cabeçalho Authorization.

2. IaC com AWS SAM:

- Converta toda a aplicação (API Gateway + Lambda + DynamoDB + Cognito) em um único arquivo template.yaml do AWS SAM.

- Faça o deploy automatizado via terminal executando sam deploy --guided.

3. Tracing com AWS X-Ray:

- Habilite o suporte ao X-Ray na função Lambda e no API Gateway no template SAM.

- Importe a biblioteca do X-Ray SDK no código da Lambda.

- Faça requisições para a API e analise o Service Map e o tempo de resposta das chamadas no console do AWS X-Ray.

## Por onde começar hoje?
Você já tem o ementário detalhado e o nosso guia de revisão DVA-C02, a melhor estratégia é:

- Escolha a sua linguagem principal: Defina se usará Python (Boto3) ou Node.js/JavaScript para os exercícios.

- Siga a Semana 1: Crie os scripts locais no seu computador para interagir com o S3 via AWS CLI e SDK.

- Consulte a documentação oficial da AWS: 
Ao estudar cada módulo, pesquise no Google por: "AWS Documentation [Nome do Serviço] developer guide".
 
 ---

Seguindo este cronograma com disciplina e realizando a parte prática, 
você terá uma excelente preparação para passar no exame AWS Certified Developer - Associate (DVA-C02).

O ementário é o esqueleto exato dos cursos oficiais da AWS para desenvolvedores. 
Ao executar a prática por conta própria, você ganha vantagens estratégicas para a prova:

- Fixação dos Conceitos Mais Cobrados: A prova DVA-C02 é extremamente focada nos fluxos de Serverless (API Gateway + Lambda + DynamoDB), Segurança (IAM + Cognito + KMS) e CI/CD (SAM + CodePipeline). Montar esses ambientes na prática faz você memorizar a arquitetura sem precisar decorá-la.

- Entendimento dos Erros e Exceções: Muitas questões de prova perguntam "Qual erro ocorreu e como resolver?" (como erros de Throttling no DynamoDB ou permissões de Bucket Policy no S3). Ao fazer os scripts em código e errar no console/CLI, você aprende a diagnosticar esses cenários rapidamente.

- Domínio de Parâmetros de Código/CLI: Escrever scripts com o SDK (boto3) ou usar a CLI fixa os nomes de parâmetros, variáveis de ambiente e arquivos de configuração (buildspec.yml, appspec.yml, template.yaml) que aparecem diretamente nas alternativas da prova.
