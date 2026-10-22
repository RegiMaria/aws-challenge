# 02  Serverless Event Submission Patterns

Neste módulo, exploraremos como transitar de padrões síncronos para padrões assíncronos e desacoplados utilizando o 
Amazon SQS e o AWS Step Functions para gerenciar picos de tráfego tratar falhas automaticamente e orquestrar fluxos complexos sem 
sobrecarregar o código do Lambda.

## 1. Integração Amazon SQS com AWS Lambda

A introdução de uma fila do Amazon SQS entre o Amazon API Gateway e o AWS Lambda cria uma conexão assíncrona que responde imediatamente
ao cliente com um **Message ID**, permitindo processar a requisição sem se preocupar com o tempo de execução do backend.

```
 ┌─────────┐ HTTP ┌──────────────┐             ┌────────────┐               ┌──────────────┐
 │ Cliente │ ───► │ API Gateway  │ ──────────► │ Amazon SQS │ ── (Poll) ──► │  AWS Lambda  │
 └─────────┘      └──────────────┘ Msg ID <─── └────────────┘               └──────────────┘

```
**Como o Lambda Processa Filas SQS:**

- Long Polling Nativo: O serviço do Lambda (não a sua função) monitora a fila do SQS e lê as mensagens em lotes (batches).

- Visibilidade das Mensagens: Durante o processamento do lote, as mensagens ficam ocultas para outros consumidores até o término do tempo de Visibility Timeout.

- Exclusão Automática: Se o Lambda processa o lote com sucesso, o próprio serviço Lambda deleta as mensagens da fila.

- Tratamento de Falhas Parciais: Se uma mensagem falhar, todo o lote pode voltar a ficar visível.
Boa Prática: Escreva código na sua função para deletar cada mensagem processada individualmente para evitar o reprocessamento desnecessário de mensagens bem-sucedidas.

- Dead-Letter Queue (DLQ): Configure uma DLQ diretamente na fila SQS de origem (e não na função Lambda) para capturar mensagens que falharem repetidamente.

**Comparativo: Fila Standard vs. Fila FIFO no SQS**

| **Característica** | **Fila SQS Standard** | **Fila SQS FIFO** |
|---|---|---|
| **Ordem dos Registros** | Não garantida. | Garantida por `Message Group ID`. |
| **Entrega de Mensagens** | Pelo menos uma vez (*At-least-once* — pode haver duplicatas). | Processamento exatamente uma vez (*Exactly-once* — evita duplicatas no período de deduplicação). |
| **Throughput / Vazão** | Praticamente ilimitado. | Até 300 mensagens/s sem lote ou 3.000 mensagens/s com lote, por padrão. |
| **Requisito no Código** | Exige código **idempotente** para lidar com mensagens duplicadas. | Garante a ordem dentro de cada grupo de mensagens e oferece deduplicação. |

## 2. Orquestração de Fluxos com AWS Step Functions

Encadear funções Lambda diretamente (uma chamando a outra) cria acoplamento rígido,
exige código manual para retentativas e dificulta o tratamento de erros (rollback).
O AWS Step Functions resolve isso ao assumir o **papel de orquestrador de estados**.

```
  ┌────────────┐      ┌─────────────────────────┐
  │ Amazon SQS │ ───► │   AWS Step Functions    │
  └────────────┘      └────────────┬────────────┘
                                   │
         ┌─────────────────────────┼─────────────────────────┐
         ▼                         ▼                         ▼
  ┌──────────────┐          ┌──────────────┐          ┌──────────────┐
  │ Task: Save   │ ───────► │ Task: Payment│ ───────► │ Task: Notify │
  │ (Lambda)     │          │ (Lambda)     │          │ (SNS/DynDB)  │
  └──────────────┘          └──────────────┘          └──────────────┘


```
**Benefícios do Step Functions:**

- Foco no Negócio: Mantém a lógica de orquestração, tempo e sequência fora do código da função Lambda.

- Padrões de Mensageria Suportados:

1. Tarefas Sequenciais (Sequential): Executa os estados em ordem linear.

2. Escolha Condicional (Conditional Choice): Adiciona ramificações baseadas em regras de negócio.

3. Laços de Repetição (Looping): Repete tarefas até atingir um critério.

4. Try / Catch / Finally: Trata erros e exceções conhecidas ou desconhecidas de forma automática.

5. Tarefas Paralelas (Parallel): Executa múltiplos fluxos concorrentes em paralelo.

- Callback Tasks & Task Tokens: Permite pausar o fluxo indefinidamente (ex: aguardando uma aprovação humana manual
ou resposta de um sistema de pagamento externo) e só continuar após receber o token de volta via `SendTaskSuccess`.

- Wait States Gratuitos: Usar estados de espera (Wait States) no Step Functions não custa nada por tempo parado,
pois a cobrança é baseada em transição de estados - ao contrário de deixar uma função Lambda parada aguardando (sleep),
que gera custos desnecessários.

**Standard Workflows vs. Express Workflows**

| **Tipo de Workflow** | **Duração Máxima** | **Modelo de Cobrança** | **Indicado Para** |
|---|---|---|---|
| **Standard Workflows** | Até 1 ano | Cobrado por **transição de estado** | Processos longos, auditáveis e com aprovação humana |
| **Express Workflows** | Até 5 minutos | Cobrado por **número de execuções e duração** | Processos de altíssimo volume, microsserviços e ingestão de dados rápidos |




