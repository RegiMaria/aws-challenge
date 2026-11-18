# Testando para carga de pico (Testing for Peak Load)

Em arquiteturas serverless, cada serviço ou microsserviço pode escalar de forma independente,
permitindo ajustar o desempenho de um componente específico sem afetar os demais. 
Para validar suas decisões de arquitetura e os trade-offs escolhidos, é fundamental 
saber o que é aceitável, o que é normal e o que exige atenção crítica.

## 1. O Ciclo: Construir, Medir, Aprender, Repetir (Build, Measure, Learn, Repeat)
```
  ┌─────────────────────────────────────────────────────────────┐
  │                        1. CONSTRUIR                         │
  │   Implante a arquitetura em uma conta AWS idêntica à de     │
  │   produção (evite mocks para serviços gerenciados).         │
  └──────────────────────────────┬──────────────────────────────┘
                                 │
                                 ▼
  ┌─────────────────────────────────────────────────────────────┐
  │                         2. MEDIR                            │
  │   Execute testes de carga com dados reais e monitore via    │
  │   CloudWatch Metrics, Logs e AWS X-Ray (foque em percentis).│
  └──────────────────────────────┬──────────────────────────────┘
                                 │
                                 ▼
  ┌─────────────────────────────────────────────────────────────┐
  │                   3. APRENDER E REPETIR                     │
  │   Identifique gargalos, ajuste limites/concorrência e       │
  │   itere até equilibrar custo e experiência do usuário.      │
  └─────────────────────────────────────────────────────────────┘
```

**Conceitos-Chave do Teste de Carga:**

- Defina o que é "Tempo Real" para o Negócio: Se você desenvolve uma aplicação em tempo real,
valide com o negócio qual latência é aceitável. Monitore percentis (ex: p95, p99) em vez de médias
brutas para entender o impacto real nos usuários.

- Orçamento de Erros (Error Budget): Construa mecanismos de tratamento de falhas para lidar com exceções
fora da curva (outliers). Saiba qual é a margem de erro tolerável pelo negócio e use o teste de carga para
comprovar que suas retentativas e DLQs funcionam sob estresse.


## 2. Boas Práticas e Dicas para Testes de Carga Serverless

Boas Práticas Gerais:

- Use dados e padrões de acesso autênticos: Simule payloads reais e a distribuição geográfica/temporal esperada dos clientes.

- Resolva gargalos de ponta a ponta (End-to-End): Ao otimizar a integração entre dois serviços, verifique se
você não está apenas empurrando o gargalo para o próximo serviço da fila.

- Mantenha os direcionadores de negócio em mente: Faça trade-offs que entreguem a melhor relação custo-benefício e a melhor experiência para o cliente.

- Valide a gestão de falhas: Teste cenários de *throttling* e falhas parciais para garantir que o sistema se recupera sem perda de dados.

**Dicas Práticas de Execução:**

1. Fique de olho nos limites de serviço (Service Quotas): Limites existem para proteger contra uso indevido. 
Muitos são soft limits (podem ser aumentados via suporte AWS), mas se não forem monitorados antes do teste, causarão degradação e throttling.

2. Use todas as ferramentas de observabilidade: Habilite métricas detalhadas, alarmes no Amazon CloudWatch e rastreamento
no AWS X-Ray durante os testes, exatamente como fará em produção.

3. Não faça mock de serviços que você não controla: Execute testes de integração e carga diretamente na nuvem AWS,
em um ambiente espelho da produção, para observar o comportamento real de rede, limites de API e cold starts.

4. Prepare a capacidade do Amazon DynamoDB: Antes de iniciar o teste de estresse, configure a tabela para o modo *On-Demand* ou ative
o Auto Scaling com capacidade inicial suficiente para absorver o ciclo do teste sem throttling prematuro.

> A Regra de Ouro: Nenhuma dica se aplica 100% a todas as situações. Mergulhe fundo na sua carga de
> trabalho específica, teste e monitore em condições reais de produção.

## 3. Recursos Adicionais e Referências

🔗 [AWS Serverless Computing](https://aws.amazon.com/pt/serverless/) (Página Oficial) — Portal central de computação serverless na AWS.

🔗[ Whitepaper: Serverless Application Lens - AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html) — Guia oficial de melhores práticas arquiteturais para aplicações serverless.

Blogs Técnicos da AWS

- [AWS Architecture Blog: 10 Things Serverless Architects Should Know](https://aws.amazon.com/pt/blogs/architecture/ten-things-serverless-architects-should-know/) — Os 10 fundamentos essenciais para arquitetos serverless.

- [AWS Compute Blog: AWS Lambda](https://aws.amazon.com/pt/blogs/compute/category/compute/aws-lambda/) — Novidades, padrões de design e otimizações do AWS Lambda.

- [AWS Compute Blog: Amazon API Gateway](https://www.google.com/search?q=https://aws.amazon.com/blogs/compute/category/networking-content-delivery/amazon-api-gateway/) — Boas práticas e recursos avançados do API Gateway.

Palestras Gravadas (Recorded Talks)

🎬 [2021 re:Invent Playlist for Serverless Breakout Sessions](https://www.youtube.com/results?search_query=AWS+reInvent+2021+Serverless+breakout+sessions) - Gravações das sessões do re:Invent focadas em arquiteturas serverless.

🎬[ AWS Lambda Customer Talks](https://www.youtube.com/results?search_query=AWS+Lambda+Customer+Talks) - Casos reais de clientes resolvendo desafios de escala e negócio com AWS Lambda.
