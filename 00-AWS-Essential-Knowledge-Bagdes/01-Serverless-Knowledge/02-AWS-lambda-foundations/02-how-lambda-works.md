# 02 - How AWS Lambda Works

## Modelos de Invocação

Para entender a arquitetura orientada a eventos do AWS Lambda, 
é essencial compreender como as fontes de eventos (event sources) iniciam as funções para executar o código.

Existem 3 modelos principais de invocação (Invocation Models), cada um atendendo a necessidades específicas de aplicação e desenvolvimento.

## 1. Invocação Síncrona (Synchronous Invocation)

Na invocação síncrona, a fonte do evento chama a função Lambda e aguarda imediatamente pela resposta.

- Como funciona: O Lambda envia o evento diretamente para a função e retorna a resposta do código 
(junto com metadados, como a versão executada) diretamente para quem fez a chamada.

- Tratamento de Erros: Não possui retentativas (retries) embutidas. 
A estratégia de tentar novamente deve ser implementada manualmente no código da aplicação do cliente.

- Exemplo de fluxo: O cliente faz uma requisição HTTP via API Gateway--> O API Gateway chama o Lambda --> O Lambda processa e devolve a resposta para o cliente na mesma hora.


Serviços AWS Síncronos:
- Amazon API Gateway

- Amazon Cognito

- AWS CloudFormation

- Amazon Alexa

- Amazon Lex

- Amazon CloudFront

---

## 2. Invocação Assíncrona (Asynchronous Invocation)

Na invocação assíncrona, os eventos são colocados em uma **fila interna** e o solicitante não espera a função terminar de rodar.

- Como funciona: O Lambda recebe o evento, confirma o recebimento para o chamador e processa a função em segundo plano.
É ideal para cenários onde o cliente não precisa de uma resposta imediata.

- Recurso de Destinos (Destinations): Permite enviar o registro do resultado de invocações assíncronas diretamente para outro
serviços sem precisar escrever código adicional:

  -- Destino de Sucesso: Envia dados quando a função processa com êxito (ex: para um barramento do Amazon EventBridge).

  -- Destino de Falha: Envia registros quando todas as tentativas de processamento falham (ex: para uma fila do Amazon SQS ou SNS).

Serviços AWS Assíncronos:
- Amazon SNS

- Amazon S3

- Amazon EventBridge

3. Invocação por Sondagem (Polling / Event Source Mapping)

Projetado para integrar com serviços de streaming e filas da AWS sem gerenciar servidores ou escrever código de conexão.

- Como funciona: A própria AWS gerencia um "sondador" (poller) que monitora o serviço de origem,
busca os eventos novos e invoca a sua função Lambda de forma síncrona em seu nome.

- Mapeamento de Fonte de Eventos (Event Source Mapping): É a configuração que conecta o serviço de origem à função Lambda e concede as permissões necessárias via IAM.

- Tratamento de Erros: As regras de retentativa variam de acordo com o serviço de origem e sua configuração específica.

Serviços Suportados pelo Event Source Mapping:
- Amazon SQS

- Amazon Kinesis

- Amazon DynamoDB Streams

- Amazon MQ

- Amazon Managed Streaming for Apache Kafka (MSK) / Apache Kafka auto-gerenciado


Resumo Comparativo dos Modelos de Invocação

| **Modelo de Invocação** | **O solicitante espera resposta?** | **Retentativas automáticas?** | **Principais Serviços Integrados** |
| ----------------------- | ---------------------------------- | --------------------------------------------- | ------------------------------------------- |
| **Síncrono** | **SIM** | Não (gerenciado na aplicação) | API Gateway, Cognito, CloudFormation, Alexa |
| **Assíncrono** | **NÃO** | Sim (gerenciado pelo Lambda) + *Destinations* | Amazon S3, SNS, EventBridge |
| **Sondagem (Polling)** | **NÃO** (AWS faz a sondagem) | Depende da fonte de eventos | SQS, Kinesis, DynamoDB Streams, MSK |

---

## Ambiente de Execução do AWS Lambda & Otimização de Performance

O AWS Lambda executa as suas funções dentro de um Ambiente de Execução (Execution Environment),
que é um ambiente seguro, isolado e totalmente gerenciado.
Ele é responsável por gerenciar os recursos de computação,
o ciclo de vida do runtime (mecanismo de execução da linguagem) e as extensões externas associadas à função.


1. O Ciclo de Vida do Ambiente de Execução

O ciclo de vida do ambiente de execução do Lambda é dividido em 3 fases principais:

<div align="center">

<img src="https://github.com/user-attachments/assets/63866c2f-f773-47a0-ac9a-cba2ea84228e"   width="600" alt="AWS Logo"/>

</div>

---

```Bash
 ┌─────────────────────────────────────────────────────────────┐
 │ 1. INIT PHASE                                               │
 │    Extension Init ──► Runtime Init ──► Function Init        │
 └──────────────────────────────┬──────────────────────────────┘
                                │
                                ▼
 ┌─────────────────────────────────────────────────────────────┐
 │ 2. INVOKE PHASE                                             │
 │    Executa o Handler da Função                              │
 └──────────────────────────────┬──────────────────────────────┘
                                │ (Inatividade)
                                ▼
 ┌─────────────────────────────────────────────────────────────┐
 │ 3. SHUTDOWN PHASE                                           │
 │    Encerra Extensões ──► Encerra Runtime ──► Apaga Ambiente │
 └─────────────────────────────────────────────────────────────┘


```

--- 

### 1. Fase de Inicialização (Init Phase)

<div align="center">

<img src="https://github.com/user-attachments/assets/ab69a8a0-7bfa-47b7-a471-345662f5ec61"  width="600" alt="AWS Logo"/>

</div>
Ocorre quando o Lambda cria ou "descongela" um ambiente de execução com os recursos configurados (memória, variáveis de ambiente, permissões). É dividida em três subfases sequenciais:

- Extension Init: Inicia todas as extensões externas configuradas.

- Runtime Init: Inicializa o runtime da linguagem (ex: Node.js, Python, JVM).

- Function Init: Executa o código estático/global da função (todo o código que fica fora do handler principal,
como importação de bibliotecas e conexões com banco de dados).

> A fase de Init ocorre na primeira invocação da função ou antecede as
> chamadas caso o Provisioned Concurrency (Concorrência Provisionada) esteja ativado.

---

## 2. Fase de Invocação (Invoke Phase)
<div align="center">

<img src="https://github.com/user-attachments/assets/ca112081-d1f4-416b-bf9b-d7699d45f534"  width="600" alt="AWS Logo"/>

</div>

Nesta fase, o Lambda executa o manipulador (handler) da função.
Após a conclusão do processamento do evento, o ambiente fica preparado e aguardando uma próxima invocação.


---

## 3. Fase de Encerramento (Shutdown Phase)
<div align="center">

<img src="https://github.com/user-attachments/assets/258eee0c-a327-4ecd-9931-48cd0e237b5c"  width="600" alt="AWS Logo"/>

</div>

Se a função passar um período de tempo sem receber novas invocações, esta fase é disparada:

- O Lambda desliga o runtime.

- Envia um sinal de encerramento para as extensões para que finalizem de forma limpa.

- Remove e destrói o ambiente de execução.

---
## Otimização de Performance: Cold Start vs. Warm Start

Como as aplicações serverless escalam dinamicamente,
entender a diferença entre inicialização fria e quente é fundamental para reduzir latência
e aumentar o rendimento (throughput).

| Tipo de Inicialização | O que acontece nos bastidores? | Impacto na Latência |
| :--- | :--- | :--- |
| **Cold Start (Inicialização Fria)** | É necessário criar um **novo ambiente de execução**. A AWS precisa baixar o código da função, alocar memória, iniciar o *runtime*, carregar as extensões e rodar o código estático/global (*Init Phase*) antes de executar o *handler*. | **Maior latência** na primeira chamada ou durante picos repentinos de tráfego. |
| **Warm Start (Inicialização Quente)** | A AWS **reaproveita o ambiente de execução** de uma chamada anterior que já estava pronto ("aquecido"). A execução pula toda a *Init Phase* e vai direto para a *Invoke Phase*. | **Resposta ultra-rápida** (poucos milissegundos). |

<div align="center">

<img src="https://github.com/user-attachments/assets/49353c47-0896-413b-afc5-7827ecafa396" width="600" alt="AWS Logo"/>

</div>

**Dica de Otimização Prática**

- Código Fora do Handler: Todas as operações pesadas que não dependem
do evento específico (como importar pacotes, inicializar SDKs e abrir conexões com banco de dados)
devem ser colocadas fora da função Handler (no código estático/global).

- Dessa forma, esses scripts rodam apenas uma única vez durante a Init Phase do Cold Start,
permitindo que todos os Warm Starts subsequentes reutilizem essas conexões e objetos já
inicializados na memória instantaneamente!




Nesta fase, o Lambda executa o manipulador (handler) da função. Após a conclusão do processamento do evento, o ambiente fica preparado e aguardando uma próxima invocação.

