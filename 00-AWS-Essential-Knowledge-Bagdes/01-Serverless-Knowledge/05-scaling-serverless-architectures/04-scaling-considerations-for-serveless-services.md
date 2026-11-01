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

## 2. Considerações de Escala no Amazon API Gateway
O Amazon API Gateway atua como a "porta de entrada" (front door) da sua aplicação.
Ele oferece diversos recursos nativos para gerenciar os padrões de acesso e proteger
os sistemas downstream contra sobrecarga.


**A. Recursos do API Gateway para Gerenciamento de Tráfego**

- Endpoints Otimizados para Borda (Edge-Optimized Endpoints):

- Possuem uma distribuição do Amazon CloudFront integrada automaticamente.

- Reduzem a latência para clientes geograficamente dispersos.

- Cache da API (API Caching):

- Armazena em cache as respostas das requisições para evitar chamadas desnecessárias aos serviços de backend (como Lambda ou DynamoDB).

- Planos de Uso e Chaves de API (Usage Plans & API Keys):

- Permitem aplicar limites de taxa (Throttling / Burst Rate) por cliente/consumidor individual.

- Evitam que um único cliente consuma toda a capacidade do seu backend.


B. O Impacto dos Lambda Authorizers na Concorrência

Se você utiliza um Lambda Authorizer para autenticação customizada (ex: validação de tokens OAuth/JWT):

- Contagem no Limite de Concorrência: Cada execução da função do Authorizer conta diretamente para o limite total de concorrência da sua conta ou da região.

- Risco: Se o tráfego de autenticação for muito alto, a função do autorizador pode esgotar a concorrência disponível antes mesmo de executar a função que processa a regra de negócio.

- Solução de Mitigação (Auth Caching): Ative o cache de autorização (Authorization Caching). O resultado da autorização fica em cache por um período configurável (de 5 minutos a 1 hora), evitando chamadas repetidas à função autorizadora para o mesmo usuário.

















