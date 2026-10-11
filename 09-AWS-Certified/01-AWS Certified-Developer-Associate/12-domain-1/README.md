# Visão Geral do Domínio 1
- Peso na prova: 32% do exame (cerca de 20 questões).
- Foco: Desenvolver, implantar e manter aplicações e código na AWS, abrangendo EC2, AWS Lambda e persistência/armazenamento de dados.

## As 3 Task Statements

**1. Task 1.1:** Desenvolver código para aplicações hospedadas na AWS

🔶Fundamentos e Configurações: Uso de SDKs, credenciais de API, chaves,
identidades para EC2, personalização de rede e gerenciamento de recursos.

🔶Padrões de Arquitetura:

- Orientado a eventos (Event-driven), Microsserviços, Coreografia (A coreografia
assim como a orquestração é um padrão de comunicação e coordenação entre microsserviços.
EDA (Arquitetura Orientada a Eventos)- serviços reagem a eventos de forma autônoma),
Orquestração(Um serviço central dita o fluxo passo a passo.) e Fanout(padrão de design de arquitetura usado
para enviar uma única mensagem ou evento para múltiplos destinos (consumidores) ao mesmo tempo).

🔶Design Tolerante a Falhas:

- Implementação de novas tentativas (retries) com exponential backoff e jitter
(*O jitter (que significa "variação" ou "ruído") é uma técnica essencial para evitar
o famoso efeito de "manada" (thundering herd problem) quando vários sistemas tentam
se reconectar ou fazer novas tentativas (retries) ao mesmo tempo.
Para entender o jitter, primeiro precisamos entender o problema que o Exponential
 Backoff tenta resolver, e onde ele falha sem o jitter*).

- Uso de Dead-Letter Queues - DLQ

🔶APIs e Serveless:

- Criação, extensão e manutenção de APIs.

- Integração profunda e conhecimentos de ações de API entre serviços:
Amazon API Gateway, AWS Lambda, DynamoDB, S3 e SQS.

- Conceitos do API Gateway: Solicitações/respostas de métodos e de integrações,
regras de validação, códigos de status personalizados, modelos de mapeamento (mapping templates),
stages, variáveis, caches e RATE LIMIT (*throttling  - No AWS API Gateway, Rate Limit e Throttling
andam de mãos dadas, mas possuem papéis complementares.Na prática, o Throttling é o nome geral
do recurso de proteção da API, e dentro dele você configura duas métricas essenciais:
 1. Rate Limit (Limite de Taxa) E 2. Burst Limit (Limite de Pico)*).

**2. Task 1.2: Desenvolver código para o AWS Lambda**

🔶Diferencial da Versão DVA-C02: A prova exige forte capacidade de codificação
e escrita de código voltado para o Lambda (mais prático do que a versão anterior C01).

🔶Parâmetros de Configuração e Ambiente:

- Definição de variáveis de ambiente.

- Ajustes de memória, concorrência, tempo limite (time-out),
ambiente de execução (runtime), manipulador (handler),
layers, extensões, gatilhos (triggers) e destinos (destinations).

🔶Boas Práticas: Tratamento de erros e padrões de codificação limpa.

**3. Task 1.3: Usar repositórios de dados no desenvolvimento de aplicações**

🔶Opções de Armazenamento: Compreender requisitos e casos de uso para cada tipo de dados.

🔶Gerenciamento e Ciclo de Vida: Uso, manutenção e gestão do ciclo de vida dos dados.

🔶Cache e Consistência:

- Serviços e estratégias de cache de dados.

- Padrões de armazenamento de dados efêmeros (ephemeral) vs. persistentes.

- Modelos de consistência de banco de dados e critérios de escolha.

🔶Migrações e Segurança:
Escolha do banco de dados adequado para necessidades específicas
e configuração de criptografia (tema que se aprofundará no Domínio 2: Segurança).

---

💡 Dicas para a Prova:
1. Código e Lógica: Lembre-se de que o DVA-C02 cobra que você saiba escrever e interpretar
trechos de código (geralmente em Python ou Node.js/JavaScript) usando SDKs (como o Boto3 para Python).

2. Resiliência: Domine como lidar com falhas de rede e limites de taxa (throttling) usando Exponential Backoff e DLQs.

3. Serveless Conectado: Entenda muito bem como o API Gateway se comunica com o Lambda e o DynamoDB (mapeamento de dados e validações).
