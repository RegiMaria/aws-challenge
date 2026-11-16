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
