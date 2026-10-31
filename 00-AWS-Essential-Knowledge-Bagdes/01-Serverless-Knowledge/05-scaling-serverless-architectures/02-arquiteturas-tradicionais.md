## 2. Arquiteturas Tradicionais (EC2) vs. Arquiteturas Serverless na Escala

Enquanto em arquiteturas tradicionais a escalabilidade exige o provisionamento de 
instâncias e balanceadores de carga, no modelo serverless o foco muda para a análise 
de limites e comportamento de eventos.

```
  ┌─────────────────────────────────────────────────────────────┐
  │ 1. TRADICIONAL (Amazon EC2 + Auto Scaling)                  │
  │    • Exige configuração de subredes, grupos de Auto Scaling │
  │      e instâncias EC2/RDS.                                  │
  │    • Foco na capacidade de instâncias e recursos ociosos.   │
  └──────────────────────────────┬──────────────────────────────┘
                                 │
                                 ▼
  ┌─────────────────────────────────────────────────────────────┐
  │ 2. SERVERLESS (AWS Lambda + Serviços Gerenciados)          │
  │    • Escala horizontal automática e alta disponibilidade    │
  │      nativas.                                               │
  │    • Foco na concorrência, no volume de eventos e nos       │
  │      limites operacionais de serviço end-to-end.            │
  └─────────────────────────────────────────────────────────────┘

```

## Quadro de conceitos

| **Conceito**              | **Arquiteturas Tradicionais (EC2)**                      | **Arquiteturas Serverless (Lambda)**                                  |
| ------------------------- | -------------------------------------------------------- | --------------------------------------------------------------------- |
| **Mecanismo de Escala**    | Auto Scaling Groups (métrica de CPU/Memória/Rede).       | Escalonamento horizontal automático orientado a eventos.              |
| **Gerenciamento de Host**  | Manual (atualização de SO, segurança, *health checks*).  | Totalmente abstraído pela AWS.                                        |
| **Custo de Ociosidade**    | Alto (paga pelas instâncias paradas aguardando tráfego). | **Zero** (paga apenas enquanto o código é executado).                 |
| **Gargalos Principais**    | Limites físicos da instância / tempo de provisionamento.| Limites de concorrência da conta/função e conexões a bancos de dados. |
