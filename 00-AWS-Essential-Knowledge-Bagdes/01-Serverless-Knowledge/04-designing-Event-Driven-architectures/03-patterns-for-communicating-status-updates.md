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

