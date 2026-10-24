# 03 - Patterns for Communicating Status Updates

Quando migramos de arquiteturas síncronas para assíncronas (onde a requisição aceita o pedido imediatamente e gera um `Job ID`),
surge a questão: Como notificar o cliente sobre o progresso e a conclusão do processamento?

Este módulo apresenta 3 padrões principais para comunicação de status, com suas vantagens, desvantagens e casos de uso.

## 1. Padrão Client Polling (Sondagem pelo Cliente)
O Client Polling é o método tradicional para consultar o status de transações de longa duração.
```

 ┌─────────┐  1. Envia Pedido ──► ┌─────────────┐
 │         │  ◄── Recebe ID ───── │ API Gateway │
 │         │                      └─────────────┘
 │ Cliente │  2. Polling /status?id=123 (a cada 30s)
 │         │ ────────────────────────────────────────► ┌──────────────┐
 │         │ ◄──────────────────────────────────────── │ DynamoDB /   │
 │         │  3. Status: "IN_PROGRESS" / "COMPLETE"    │ Status API   │
 └─────────┘                                           └──────────────┘

```

**Como Funciona:**

- O cliente faz a requisição inicial e recebe um `Job ID` ou Order `ID`.

- O cliente realiza chamadas HTTP periódicas (ex: a cada 30 segundos) para um endpoint dedicado de consulta (`/status?id=123`).

- O workflow do AWS Step Functions atualiza o status no Amazon DynamoDB conforme avança.

- Quando a consulta retorna o status `"COMPLETE"`, o cliente faz uma busca no endpoint de resultados para obter o dado final.

**Prós e Contras:**

✅ Vantagens:

- Extremamente simples de implementar e integrar sobre backend tradicional.

- Fácil substituição para quem está migrando de fluxos síncronos.

❌ Desvantagens:

- Latência de Consistência: Se a tarefa terminar no segundo 1 de um ciclo de 30 segundos, o cliente só saberá 29 segundos depois.

- Desperdício e Custo Elevação: Dezenas de requisições HTTP são enviadas sem alteração de estado, consumindo recursos e gerando
custos desnecessários tanto no cliente quanto na AWS.

---

## 2. Padrão Webhooks com Amazon SNS
Os Webhooks são retornos de chamada HTTP (user-defined HTTP callbacks) orientados a eventos, 
eliminando a necessidade de o cliente ficar consultando o servidor.

```

 ┌─────────┐  1. Envia Pedido + URL do Webhook ──► ┌─────────────┐
 │         │  ◄── Recebe ID ────────────────────── │ API Gateway │
 │ Cliente │                                       └─────────────┘
 │ (Servidor│  2. Notificação de Conclusão (Push)         ▲
 │ Web)    │ ◄────────────────────────────── ┌─────┴─────┐ │ (Task
 └─────────┘                                 │ Amazon SNS│ │  Complete)
                                             └───────────┘ │
                                                   │       │
                                           ┌───────┴───────┴──────┐
                                           │  AWS Step Functions  │
                                           └──────────────────────┘

```

**Como Funciona:**

- O cliente registra o seu próprio endpoint Webhook (ex: `[https://meu-site.com/api/callback](https://meu-site.com/api/callback)`).

- O serviço backend processa a tarefa em segundo plano através do Step Functions.

- Ao finalizar, o Step Functions aciona o Amazon SNS enviando uma mensagem.

- O Amazon SNS realiza uma chamada HTTP/HTTPS para o Webhook do cliente enviando o payload com o status e os dados.


**Tipos de Clientes:**

- Trusted Clients (Clientes Confiáveis): Quando você gerencia ambos os lados do sistema.
O uso do Amazon SNS com inscritos HTTP (HTTP Subscribers) fornece mecanismo nativo de retentativa
e backoff exponencial caso o endpoint do cliente esteja fora do ar.

- Untrusted Clients (Clientes Não Confiáveis/Externos): Exige um cadastro prévio do Webhook e acordos rígidos de SLA e janelas de expiração de retentativa.

**Prós e Contras:**
✅ Vantagens:

- Muito mais eficiente computacionalmente- zero chamadas desnecessárias.

- Notificação quase em tempo real.

- O Amazon SNS gerencia retentativas automáticas e exponencial backoff em falhas de entrega.

❌ Desvantagens:

- Exige que o cliente mantenha e exponha um endpoint Web público ativo.

- Maior complexidade na gestão de segurança (autenticação de requisições recebidas).

# 3. Padrão WebSockets com AWS AppSync
Para aplicações orientadas a dados e interfaces reativas em tempo real (como dashboards e aplicativos mobile),
utilizar WebSockets com GraphQL via AWS AppSync é o padrão ideal.

```

 ┌─────────┐  1. Conexão Persistente / Inscrição GraphQL ──► ┌──────────────┐
 │         │  ◄── Atualizações de Status em Tempo Real ────── │ AWS AppSync  │
 │ Cliente │                                                 │ (GraphQL +   │
 └─────────┘                                                 │  WebSockets) │
                                                             └──────┬───────┘
                                                                    │
                                                             ┌──────┴───────┐
                                                             │ AWS Step     │
                                                             │ Functions    │
                                                             └──────────────┘

```

**Como Funciona:**

- O cliente estabelece uma conexão bidirecional WebSockets com o AWS AppSync utilizando Subscriptions do GraphQL.

- À medida que o workflow no Step Functions/Lambda altera o estado dos dados, o AppSync notifica instantaneamente os clientes inscritos (*Subscribers*).

- O cliente pode usar filtros nativos do GraphQL para receber estritamente as alterações de dados que necessita.


**Comparativo: API Gateway vs. AWS AppSync para Status**

| **Recurso**             | **Amazon API Gateway (REST/WebSocket)**                                     | **AWS AppSync (GraphQL)**                                                                |
|-------------------------|-----------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| **Padrão de API**       | REST / WebSockets tradicionais.                                             | GraphQL com Subscriptions nativas.                                                       |
| **Inscrição em Dados**  | Requer gerenciamento manual de conexões em tabelas DynamoDB.                | Nativa — o cliente se inscreve e recebe atualizações de alterações de estado automaticamente. |
| **Filtragem de Dados**  | Payload fixo retornado pelo endpoint.                                       | O cliente especifica exatamente os campos que deseja receber.                            |
| **Controle de Tráfego** | Ideal para gerenciamento fino de APIs (Throttling, planos de uso, versões). | Excelente para UIs reativas e sincronização em tempo real.                              |

**Resumo Comparativo dos 3 Padrões**

| **Padrão**               | **Complexidade no Cliente**       | **Latência da Notificação**                | **Custo/Eficiência**                  | **Melhor Caso de Uso**                                     |
|--------------------------|-----------------------------------|--------------------------------------------|---------------------------------------|------------------------------------------------------------|
| **Client Polling**       | Baixa                             | Alta (limitada pelo intervalo de polling)  | Ineficiente (muitas chamadas vazias)  | Migração rápida de sistemas legados síncronos.             |
| **Webhooks (SNS)**       | Média (precisa expor API)         | Baixa (notificação quase em tempo real)    | Alta eficiência                       | Integração entre sistemas backend e serviços de terceiros. |
| **WebSockets (AppSync)** | Baixa/Média (usando SDK AppSync)  | Quase nula (comunicação contínua em tempo real) | Alta eficiência                  | Aplicações interativas, dashboards e sincronização em tempo real. |









