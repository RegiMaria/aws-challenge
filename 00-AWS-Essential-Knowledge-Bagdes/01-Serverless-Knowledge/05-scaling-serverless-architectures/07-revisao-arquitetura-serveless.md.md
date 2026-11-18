# 📝 Revisão & Knowledge Check: Arquiteturas Serverless em Escala

Este documento reúne questões práticas e explicações técnicas detalhadarquitetura-seveleas sobre **Arquiteturas Orientadas a Eventos**, **Processamento de Streams** e **Escalonamento de Aplicações Serverless na AWS**.

## 1. Arquiteturas Orientadas a Eventos & Processamento de Dados

### Questão 1.1: Pico de Tráfego e Pedidos Duplicados em E-commerce

**Cenário:** Uma empresa possui um site de e-commerce que vende pijamas para cães. Sempre que um pedido é realizado, a empresa deseja enviar um alerta por e-mail ao cliente e, ao mesmo tempo, processar o pedido e atualizar o banco de dados de estoque. Após um apresentador famoso mencionar o site, a empresa começou a enfrentar problemas no processamento devido ao alto volume e está gerando **pedidos duplicados** para os clientes. Qual arquitetura atende aos requisitos e melhora a experiência do cliente?

* ✅ **Resposta Correta:** **Usar o padrão *fan-out* do Amazon SNS. Quando o pedido for realizado, enviar uma mensagem para o tópico SNS, tendo os endereços de e-mail dos clientes e uma fila Amazon SQS FIFO (*First-In-First-Out*) como assinantes. Usar o AWS Lambda para consultar a fila, filtrar a mensagem e atualizar o banco de dados.**

* **💡 Explicação Técnica:**

  * **Padrão SNS Fan-Out:** Permite disparar **duas ações paralelas e desacopladas** a partir de um único evento (notificação por e-mail e enfileiramento para atualização do banco de dados).

  * **Fila SQS FIFO:** Garante o processamento exatamente uma vez (*Exactly-once processing*) e preserva a ordem estrita das transações, eliminando a criação de pedidos duplicados durante picos repentinos de tráfego.

### Questão 1.2: Ausência de Retentativas Automáticas no AWS Lambda

**Cenário:** Uma equipe de desenvolvedores percebeu que uma de suas funções AWS Lambda continua falhando, e um membro da equipe questiona por que o serviço não realizou retentativas automáticas da requisição. O que explica esse comportamento?

* ✅ **Resposta Correta:** **A equipe está usando uma fonte de eventos síncrona (*synchronous event source*) para invocar a função, a qual não possui retentativas embutidas para requisições com falha ou *throttled*.**

* **💡 Explicação Técnica:**

  * **Invocação Síncrona** (ex: Amazon API Gateway ou chamadas diretas `RequestResponse`): O Lambda executa a função e retorna o erro imediatamente ao cliente, deixando a lógica de retentativa sob responsabilidade do chamador.

  * **Por que as outras estão erradas:** Fontes *assíncronas* possuem 2 retentativas automáticas; *SQS* e *Streams (Kinesis)* possuem mecanismos contínuos de retentativa gerenciados pelo *event source mapping*.

### Questão 1.3: Comunicação de Status em Transações de Longa Duração

**Cenário:** Qual padrão de comunicação de atualizações de status você deve usar para obter informações de progresso sobre uma transação de longa duração (*long-running transaction*)?

* ✅ **Resposta Correta:** **Client polling pattern**

* **💡 Explicação Técnica:**

  * No **Client Polling Pattern**, o cliente recebe um identificador da tarefa (`Job ID`) ao iniciar a operação assíncrona e realiza requisições HTTP periódicas a um endpoint de status até que o processamento seja finalizado.

  * *Nota:* **Webhooks** (SNS) usam modelo *push*; **WebSockets** (AppSync) mantêm conexões persistentes bidirecionais; e o **Saga Pattern** (Step Functions) gerencia transações distribuídas e rollbacks compensatórios.

### Questão 1.4: Atributos de Serviços de Streaming no Processamento Serverless

**Cenário:** Quais das opções a seguir são atributos do uso de serviços de *streaming* (como Amazon Kinesis Data Streams) para processamento de dados serverless? *(Selecione DUAS)*

* ✅ **Respostas Corretas:**

  1. **Os dados no *stream* permanecem armazenados por um período determinado e depois são removidos, independentemente do comportamento dos consumidores.**

  2. **Cada consumidor do *stream* precisa manter um ponteiro (*offset / sequence number*) para uma posição específica no fluxo e avançá-lo após processar cada registro.**

* **💡 Explicação Técnica:**

  * Diferente de filas de mensageria (como o Amazon SQS), onde a mensagem é deletada logo após ser consumida, nos serviços de *streaming* os registros ficam retidos em um buffer imutável (padrão de 24h) permitindo que múltiplos consumidores independentes leiam o mesmo fluxo de dados no seu próprio ritmo.

## 2. Scaling Serverless Architectures

### Questão 2.1: Aumento de `IteratorAge` no Amazon Kinesis Data Streams

**Cenário:** Sua função AWS Lambda não está processando os registros do Amazon Kinesis Data Streams em tempo hábil, e a métrica `IteratorAge` está aumentando continuamente no painel do Lambda. Qual é a melhor estratégia para resolver esse problema?

* ✅ **Resposta Correta:** **Fazer *resharding* para aumentar o número de *shards* no seu *stream*.**

* **💡 Explicação Técnica:**

  * A concorrência do Lambda ao consumir um *stream* do Kinesis é diretamente proporcional ao número de *shards* (por padrão, 1 invocação concorrente por *shard*).

  * O aumento contínuo do `IteratorAge` indica que a taxa de entrada de dados supera a velocidade de processamento do consumidor. Realizar o **resharding** divide os novos registros entre mais *shards*, aumentando o paralelismo e a vazão (*throughput*).

### Questão 2.2: Boas Práticas de Escalonamento na Nuvem

**Cenário:** Quais são algumas das boas práticas recomendadas para escalar arquiteturas serverless? *(Selecione DUAS)*

* ✅ **Respostas Corretas:**

  1. **Aproveitar a Infraestrutura Global da AWS (*Take advantage of the AWS Global Infrastructure*).**

  2. **Monitorar utilizando percentis (*Monitor for percentile*).**

* **💡 Explicação Técnica:**

  * **Infraestrutura Global:** Utilizar recursos como múltiplas Zonas de Disponibilidade e pontos de presença na borda (*Edge Locations* com CloudFront e *Edge-Optimized Endpoints* no API Gateway) entrega alta disponibilidade nativa e baixa latência global.

  * **Monitoramento por Percentis:** Avaliar métricas como `p95` e `p99` reflete a experiência real da base de usuários, evitando que valores extremos (*outliers*) distorçam a análise como ocorre em médias simples.

### Questão 2.3: Fatores de Escalonamento no AWS Lambda

**Cenário:** Quais são as principais considerações ao planejar o escalonamento de funções AWS Lambda? *(Selecione TRÊS)*

* ✅ **Respostas Corretas:**

  1. **Comportamento de pico (*Burst behavior*)**

  2. **Configuração de memória (*Memory configuration*)**

  3. **Limites de concorrência (*Concurrency limits*)**

* **💡 Explicação Técnica:**

  * **Burst Behavior:** Toda a concorrência da conta não fica disponível no primeiro segundo de um pico; após o limite inicial da região, o Lambda adiciona **500 invocações por minuto**.

  * **Memory Configuration:** CPU e largura de banda de rede/disco escalam linearmente com a memória alocada.

  * **Concurrency Limits:** Gerenciamento do pool da conta e uso de *Reserved Concurrency* para isolar funções críticas ou proteger recursos *downstream*.

### Questão 2.4: Escalonamento de Bancos de Dados Serverless

**Cenário:** Quais afirmações refletem abordagens recomendadas para escalar bancos de dados em arquiteturas serverless? *(Selecione DUAS)*

* ✅ **Respostas Corretas:**

  1. **O modo *On-Demand* do Amazon DynamoDB é uma boa escolha se você precisa rastrear o custo de transações individuais.**

  2. **A capacidade provisionada (*Provisioned capacity*) pode ser a melhor escolha se você tiver uma carga de trabalho muito consistente e previsível.**

* **💡 Explicação Técnica:**

  * No modo **On-Demand**, a cobrança é feita por leitura/escrita individual, facilitando o cálculo direto do custo por transação.

  * No modo **Provisioned**, você define RCUs e WCUs fixas (com ou sem Auto Scaling), o que reduz o custo total quando o padrão de tráfego é estável e previsível.

  * *Nota:* Para bancos relacionais tradicionais, a recomendação não é apenas limitar a concorrência do Lambda, mas usar um **proxy de conexões** (como o *Amazon RDS Proxy*).

### Questão 2.5: Otimização de Custos em Fluxos Serverless Previsíveis

**Cenário:** Uma empresa está projetando um fluxo de trabalho no AWS Step Functions composto por funções AWS Lambda e uma tabela do Amazon DynamoDB. A aplicação possui uma carga de trabalho **muito consistente e previsível**, mas o custo atual está elevado. Quais opções podem reduzir o custo dessa arquitetura? *(Selecione TRÊS)*

* ✅ **Respostas Corretas:**

  1. **Usar *wait states* e *callbacks* no AWS Step Functions.**

  2. **Usar o *AWS Lambda Power Tuning*.**

  3. **Usar capacidade provisionada (*Provisioned capacity*) para o Amazon DynamoDB.**

* **💡 Explicação Técnica:**

  * **Wait States & Callbacks:** Evitam que funções Lambda fiquem ociosas aguardando respostas externas (pagando por tempo de duração sem processamento).

  * **AWS Lambda Power Tuning:** Ferramenta *open-source* que testa a função em diferentes níveis de memória para encontrar a configuração com o menor custo total (muitas vezes, mais memória executa mais rápido e custa menos).

  * **DynamoDB Provisioned Capacity:** Como o enunciado especifica uma carga de trabalho **consistente e previsível**, trocar o modo *On-Demand* por capacidade provisionada reduz significativamente os custos operacionais.