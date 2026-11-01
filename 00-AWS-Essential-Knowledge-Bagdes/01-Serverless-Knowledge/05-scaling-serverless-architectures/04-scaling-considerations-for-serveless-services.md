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

## 3. Decisões Arquiteturais e Trade-offs

Ao projetar arquiteturas serverless, você deve ponderar entre simplicidade e capacidade de escala:

```
Opção A: Integração com Fila (Maior Desacoplamento)
┌─────────────┐       ┌────────────┐       ┌──────────────┐       ┌────────────────┐
│ API Gateway │ ───►  │ Amazon SQS │ ───►  │  AWS Lambda  │ ───►  │ Step Functions │
└─────────────┘       └────────────┘       └──────────────┘       └────────────────┘

Opção B: Integração Direta (Menor Complexidade)
┌─────────────┐       ┌────────────────┐
│ API Gateway │ ───►  │ Step Functions │
└─────────────┘       └────────────────┘
```

- Direct Service Integrations (Integrações Diretas): Conectar o API Gateway diretamente ao AWS Step Functions (removendo SQS e Lambda da intermediação) reduz a complexidade da arquitetura.

- Trade-off de Vazão: No entanto, essa abordagem direta exige verificar se a API de destino (ex: StartExecution do Step Functions) suporta o volume de requisições por segundo que o API Gateway enviará.

- Princípio Arquitetural: "Conheça os direcionadores reais do seu negócio e construa o que você precisa, em vez de projetar para uma escala infinita."

## 4. Compatibilidade de Limites de Payload (Payload Size)

Ao conectar serviços em uma esteira serverless, o menor limite de payload entre os serviços conectados dita o limite máximo da sua integração:

| **Serviço AWS**        | **Limite Máximo de Payload**                            |
| ---------------------- | ------------------------------------------------------- |
| **Amazon API Gateway** | **10 MB**                                               |
| **Amazon SQS**         | **256 KB**                                              |
| **AWS Lambda**         | **6 MB** (Invocação Síncrona) / **256 KB** (Assíncrona) |


> ⚠️ Atenção: Em uma integração direta do API Gateway (10 MB) para uma fila Amazon SQS (256 KB),
> o limite efetivo da sua API passa a ser 256 KB. Tentar enviar um payload maior que 256 KB resultará
> em erro na integração.














