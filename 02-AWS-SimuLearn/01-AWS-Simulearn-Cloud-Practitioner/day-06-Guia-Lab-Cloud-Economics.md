# AWS SimuLearn: Cloud Economics - Guia Passo a Passo

## O que é o AWS SimuLearn?

O **AWS SimuLearn** é uma plataforma de aprendizado interativo da AWS que combina conceitos teóricos com simulações práticas hands-on, sem a necessidade de uma conta real na AWS. Cada módulo costuma seguir três fases:

1. **Concept** - explicações curtas sobre o serviço ou conceito da AWS envolvido.
2. **Practice Lab** - um passo a passo guiado, onde você replica uma tarefa real usando as ferramentas da AWS (nesse caso, o AWS Pricing Calculator).
3. **DIY (Do It Yourself)** - uma tarefa final que você resolve sozinho, aplicando o que aprendeu na prática guiada, validada automaticamente por um servidor de testes.

Neste módulo específico, **Cloud Economics**, o objetivo é entender como estimar custos de infraestrutura EC2 que varia conforme a demanda (tráfego), usando conceitos como *elasticidade*, *scaling* e *rightsizing* (redimensionamento de instâncias para otimizar custo x desempenho).

---

## Parte 1 - Practice Lab (Guiado)

**Objetivo:** Criar um grupo de estimativa de custos e configurar um cenário EC2 de tráfego variável (pico diário).

### Passo 1 - Acessar a calculadora
1. Abra uma nova aba no navegador e acesse: `https://calculator.aws`
2. Na página inicial, clique em **Create estimate**.

### Passo 2 - Criar um grupo lógico de estimativa
1. No menu de navegação superior (breadcrumb), clique em **My Estimate**.
2. Clique em **Create group**.
3. No campo **Group name**, digite: `Web Servers`
4. Clique em **Create group**.

### Passo 3 - Adicionar o serviço EC2
1. Na seção **Web Servers**, clique em **Add service**.
2. No campo **Find Service**, digite: `ec2`
3. No card do **Amazon EC2**, clique em **Configure**.

### Passo 4 - Configurar região e descrição
1. Em **Description**, digite: `Web Server Estimate`
2. Em **Choose a location type**, selecione: `Region`
3. Em **Choose a Region**, selecione: `US East (N. Virginia)`

### Passo 5 - Definir especificações da instância
1. Em **Tenancy**, escolha: `Shared Instances`
2. Em **Operating system**, escolha: `Linux`
3. Em **Workloads**, revise os tipos disponíveis (clique em cada um para ver detalhes).

### Passo 6 - Selecionar o padrão de carga de trabalho
1. Escolha o workload **Daily spike traffic** (tráfego com pico diário).
2. Em **Workload days**, marque todos os 7 dias (domingo a sábado).

### Passo 7 - Definir baseline e pico
1. Em **Baseline**, digite: `2`
2. Em **Peak**, digite: `4`
3. Em **Duration of peak → Hours**, digite: `8`
4. Em **Minutes**, digite: `0`

### Passo 8 - Rightsizing (escolher o tipo de instância)
1. Na seção **EC2 Instances**, em **vCPUs**, escolha: `2`
2. Em **Memory (GiB)**, escolha: `4 GiB`
3. Em **Network performance**, escolha: `Any Network Performance`
4. Selecione o tipo de instância: **t3.medium**

### Passo 9 - Modelo de pagamento
1. Em **Payment options**, escolha: `On-Demand`
2. Revise (sem selecionar) as opções de **Compute Savings Plans** e **EC2 Instance Savings Plans**.

### Passo 10 - Opções de compra adicionais (revisão)
1. Expanda **Other purchasing options**.
2. Leia sobre **Standard Reserved Instances** (maior desconto, só troca dentro da mesma família de instância) e **Convertible Reserved Instances** (menor desconto, mas permite trocar família/OS/tenancy).
3. Expanda **Show calculations** e clique no link **"estimated workload hours"** para ver o detalhamento.

### Passo 11 - Configurar armazenamento (EBS)
1. Expanda **Amazon Elastic Block Store (EBS)**.
2. Em **Storage for each EC2 Instance**, escolha: `General Purpose SSD (gp3)`
3. Em **IOPS**, digite: `30`
4. Em **Storage amount**, digite: `10`
5. Em **Unit**, escolha: `GB`

### Passo 12 - Snapshots do EBS
1. Em **Snapshot Frequency**, escolha: `Weekly`
2. Em **Amount changed per snapshot**, digite: `1`
3. Em **Unit**, escolha: `GB`
4. Expanda **Data transfer**.

### Passo 13 - Transferência de dados (entrada)
1. Em **Inbound Data Transfer → Data transfer from**, escolha: `Internet (free)`
2. Em **Enter Amount**, digite: `1`
3. Em **Data amount**, escolha: `TB per month`

### Passo 14 - Transferência de dados (saída)
1. Em **Outbound Data Transfer → Data transfer to**, escolha: `Internet`
2. Em **Enter Amount**, digite: `100`
3. Em **Data amount**, escolha: `GB per month`
4. Expanda **Show calculations** para revisar as taxas.

### Passo 15 - Salvar e visualizar resumo
1. Clique em **Save and add service**.
2. Clique em **View summary**.

### Passo 16 - Adicionar suporte (apenas revisão)
1. Na seção **Web Servers**, clique em **Add support**.
2. Revise as opções de planos de suporte (clique em cada um para ver detalhes).
3. Clique em **Cancel** (não é necessário adicionar suporte nesta prática).

### Passo 17 - Compartilhar a estimativa
1. Revise o **Estimate summary**.
2. Clique em **Share**.
3. Na janela pop-up, clique em **Agree and continue**.
4. Clique em **Copy public link** (guarde esse link - sua estimativa fica salva nos servidores da AWS).
5. Feche a janela clicando em **Cancel**, se necessário.

✅ **Practice Lab concluído!**

---

## Parte 2 - DIY (Faça Você Mesmo)

**Objetivo do DIY:**
1. Alterar o tipo de instância EC2 de `t3.medium` para `t2.micro`.
2. Gerar um novo link de estimativa compartilhável.

> O validador do teste vai conferir se você fez o *rightsizing* corretamente, ou seja, se a família da instância mudou de **t3** para **t2**.

### Passo 1 - Abrir a estimativa existente
1. Acesse novamente `https://calculator.aws` (ou use o link salvo da estimativa criada na prática).
2. Vá até o grupo **Web Servers** e abra o serviço **Amazon EC2** (Web Server Estimate) para edição.

### Passo 2 - Alterar o tipo de instância
1. Localize a seção **EC2 Instances**.
2. Remova/desmarque a seleção atual (`t3.medium`).
3. Selecione o tipo de instância: **t2.micro**.
   - Repare que a família muda de `t3` para `t2` - é exatamente isso que o validador confere.

### Passo 3 - Salvar as alterações
1. Role até o final da página e clique em **Save and add service** (ou **Save**, dependendo da tela).
2. Clique em **View summary** para confirmar que a alteração foi aplicada corretamente.

### Passo 4 - Gerar um novo link de compartilhamento
1. No **Estimate summary**, clique em **Share** novamente.
2. Na janela pop-up, clique em **Agree and continue**.
3. Clique em **Copy public link** para copiar o novo link, que agora reflete a instância `t2.micro`.

✅ **DIY concluído!** Envie/valide o novo link gerado conforme solicitado pela plataforma.

---

## Dica extra - por que trocar para t2.micro?

- **t3.medium**: 2 vCPUs, 4 GiB de memória - instância de propósito geral mais robusta.
- **t2.micro**: 1 vCPU, 1 GiB de memória - instância bem menor, geralmente usada para cargas de trabalho leves ou elegível ao *Free Tier* da AWS.

Essa troca ilustra o conceito de **rightsizing**: ajustar o tamanho da instância ao que a carga de trabalho realmente precisa, reduzindo custo sem comprometer (ou aceitando um trade-off consciente de) desempenho.

## Aruitetura da tarefa
<img width="875" height="577" alt="Image" src="https://github.com/user-attachments/assets/f8dc3a52-7781-4179-b451-2e194ae365ed" />
