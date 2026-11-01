# 04 - Scaling Considerations for Serverless Services

Para dimensionar uma arquitetura serverless com sucesso, não basta entender o comportamento de um único serviço.
É necessário conhecer as capacidades, os limites operacionais (service quotas) e os pontos de estrangulamento (bottlenecks)
em todas as integrações do fluxo ponta a ponta.

## 1. Os 4 Pilares da Consideração de Escala
Ao avaliar como cada serviço gerenciado afetará o dimensionamento da sua aplicação, analise sempre estes quatro fatores:
```

 ┌─────────────────────────────────────────────────────────────┐
 │ 1. TIMEOUTS (Tempo limite de execução)                      │
 │    Qual é o tempo máximo que cada componente aguarda?       │
 └──────────────────────────────┬──────────────────────────────┘
                                │
                                ▼
 ┌─────────────────────────────────────────────────────────────┐
 │ 2. RETRY BEHAVIORS (Comportamentos de Retentativa)          │
 │    Como os componentes lidam com falhas intermediárias?     │
 └──────────────────────────────┬──────────────────────────────┘
                                │
                                ▼
 ┌─────────────────────────────────────────────────────────────┐
 │ 3. THROUGHPUT (Vazão/fluxo de requisições)                        │
 │    Quantas requisições/segundo o backend suporta?           │
 └──────────────────────────────┬──────────────────────────────┘
                                │
                                ▼
 ┌─────────────────────────────────────────────────────────────┐
 │ 4. PAYLOAD SIZE (Tamanho da carga útil)                     │
 │    Qual é o menor limite aceito entre dois serviços?        │
 └─────────────────────────────────────────────────────────────┘
```


