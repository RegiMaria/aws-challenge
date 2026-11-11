# 05 - Lambda Execution Environment Reuse e Banco de dados

Este módulo aborda como otimizar a performance das funções Lambda aproveitando o reuso 
do ambiente de execução (Warm Starts) e como escolher e dimensionar bancos de dados serverless (com foco no Amazon DynamoDB).

## 1. Reaproveitamento do Ambiente de Execução no Lambda

Mesmo que o código no Lambda deva ser estritamente sem estado (stateless), você pode melhorar 
drasticamente a performance aproveitando **invocações Warm** quando o mesmo ambiente de execução é reutilizado.

```
  ┌─────────────────────────────────────────────────────────────┐
  │ INICIALIZAÇÃO / FORA DO HANDLER (Executado apenas no INIT)  │
  │ • Importação de dependências / SDKs                         │
  │ • Conexões com banco de dados / Caches                      │
  │ • Leitura de parâmetros do Parameter Store                  │
  └──────────────────────────────┬──────────────────────────────┘
                                 │
                                 ▼
  ┌─────────────────────────────────────────────────────────────┐
  │ HANDLER PRINCIPAL (Executado a CADA invocação)              │
  │ • Lógica de negócio focada                                  │
  │ • Reutilização das conexões e variáveis já inicializadas    │
  └─────────────────────────────────────────────────────────────┘
```

**5 Boas Práticas de Design para Warm Starts:**

1. Busca Externa Fora do Handler: Declarar SDKs, configurações externas ou clientes HTTP
fora da função manipuladora (handler). A busca ocorre na inicialização (Init Phase) e as chamadas subsequentes reutilizam o objeto.

2. Limitar Re-inicialização de Variáveis: Variáveis globais declaradas fora do handler permanecem inicializadas entre invocações no mesmo ambiente.

3. Reutilizar Conexões Existentes: Adicionar lógica para checar se a conexão com o banco de dados já está aberta antes de tentar criar uma nova.

4. Utilizar o Espaço `/tmp` como Cache Local: O diretório temporário /tmp permanece persistido durante o tempo de vida de um ambiente reutilizado.

5. Concluir Processos em Segundo Plano: Garantir que todas as promises, threads ou callbacks terminem antes da saída do código.
Processos pendentes serão pausados no fim da execução e retomados no próximo Warm Start.

## 2. Escalonamento de Bancos de Dados Serverless: Amazon DynamoDB

O Amazon DynamoDB é a escolha primária para arquiteturas serverless devido à sua capacidade de escala horizontal
e latência de milissegundos de um único dígito.

### Comparativo: Modo On-Demand vs. Modo Provisioned

| **Característica** | **Modo On-Demand** | **Modo Provisioned** |
|:---|:---|:---|
| **Escalonamento** | Automático com base nos picos anteriores. | Definido via **RCUs** (Read Capacity Units) e **WCUs** (Write Capacity Units). |
| **Comportamento de Pico** | Suporta instantaneamente **até o dobro do pico anterior** de tráfego. | Limitado aos valores alocados; exceder a capacidade gera **Throttling**. |
| **Modelo de Cobrança** | Pago por requisição individual de leitura/escrita. | Valor fixo mensal/horário baseado na capacidade provisionada. |
| **Gestão de Custos** | Simplifica o cálculo de custos por transação. | Requer planejamento de capacidade ou uso de Auto Scaling. |
| **Caso de Uso Ideal** | Cargas imprevisíveis, microsserviços serverless ou início de projetos. | Cargas de trabalho consistentes e altamente previsíveis. |

**DynamoDB Auto Scaling (no Modo Provisioned)**

- Permite definir limites mínimos e máximos de capacidade e uma meta de utilização (entre 20% e 90%).

- O Auto Scaling ajusta as RCUs/WCUs quando a carga permanece elevada ou baixa por alguns minutos.

**Amazon DynamoDB Accelerator (DAX)**

- O que é: Um cache em memória totalmente gerenciado e altamente disponível criado especificamente para o DynamoDB.

- Casos de Uso: Aplicações com leitura intensa (read-heavy) e requisitos de latência em sub-milissegundos
(ex: leilões em tempo real, jogos, sistemas financeiros).

- Requisito de Rede: O DAX deve ser instalado dentro de uma VPC. Com as melhorias no Lambda que eliminaram a
penalidade de Cold Start em VPCs, o uso do DAX com o Lambda tornou-se uma opção altamente eficiente.

## Serverless scaling with traditional relational databases



