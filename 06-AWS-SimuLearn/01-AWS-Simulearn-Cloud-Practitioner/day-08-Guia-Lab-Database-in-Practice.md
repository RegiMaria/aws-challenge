# AWS SimuLearn: Databases in Practice

## 📋 Descrição

A empresa de seguros quer ajudar seus administradores de banco de dados a gastar menos tempo com tarefas operacionais, como aplicação de patches e gerenciamento de infraestrutura de banco de dados. Eles também querem uma solução que melhore a disponibilidade e a eficiência do banco de dados.

## 🗣️ Contexto do Cenário

**Diálogo com a Dra. Dee Tah (cliente):**

- Os administradores de banco de dados da empresa reclamam que gastam tempo demais com tarefas operacionais (patching, gerenciamento de infraestrutura) em vez de inovar para os clientes.
- A empresa quer testar o Amazon RDS em ambientes de dev/test.
- **Amazon RDS (Relational Database Service)** cuida do patching e do gerenciamento de infraestrutura do banco de dados automaticamente.
- O Amazon RDS pode realizar **backups de rotina**, com um período de retenção configurável.
- Para resiliência em caso de desastre, o Amazon RDS pode implantar o banco de dados em **múltiplas Availability Zones (AZs)**. Se uma AZ tiver problema, o RDS troca automaticamente para outra AZ, mantendo os dados sincronizados e as aplicações disponíveis.
- ⚠️ **Importante:** implantar em múltiplas AZs serve para **alta disponibilidade e recuperação de desastres**, e **não** dobra a performance do banco.
- O time de Data Analytics roda consultas em tempo real e análises de big data, o que impacta a performance a cada carga grande de leitura.
- Para workloads de leitura intensiva, o Amazon RDS oferece **read replicas** (réplicas de leitura): cópias do banco primário que assumem as operações de leitura, liberando o banco primário para as operações de escrita.
- Para migração, o **AWS Database Migration Service (AWS DMS)** oferece migrações seguras e simples, mantendo o banco de origem totalmente operacional durante o processo.

**Anotações técnicas da solução:**

1. Criar uma instância de banco de dados Amazon RDS.
2. Habilitar backups no banco de dados.
3. Habilitar múltiplas AZs na implantação do Amazon RDS.
4. Criar uma read replica do Amazon RDS.

---

### Solução proposta: Amazon RDS

**Solicitação de solução:** migrar para o Amazon RDS para automatizar as tarefas rotineiras de administração de banco de dados, implementando implantação Multi-AZ e read replicas para fornecer alta disponibilidade e melhorar a performance.

---

## 🧩 Visão Geral da Solução

1. Esta solução usa o Amazon RDS para hospedar o banco de dados de uma aplicação de clientes em uma instância RDS de AZ única (single-AZ).
2. O Amazon RDS fornece serviços de banco de dados totalmente gerenciados, automatizando o provisionamento de infraestrutura, backups, snapshots e substituição de hosts, minimizando a sobrecarga administrativa.
3. A alta disponibilidade é obtida através da implantação **Multi-AZ**, na qual o Amazon RDS replica os dados de forma **síncrona** da instância primária para uma instância standby em uma AZ diferente.
4. Em caso de falha da instância primária, o Amazon RDS realiza o **failover automaticamente** para a instância standby e redireciona as requisições sem intervenção manual.
5. O registro DNS da instância do banco de dados **permanece o mesmo** durante o failover, permitindo recuperação automática da aplicação sem ação administrativa.
6. Réplicas somente leitura (**read-only replicas**) podem ser implementadas para workloads de leitura intensa, distribuindo o tráfego de leitura da aplicação entre várias cópias dos dados para aumentar a taxa de transferência agregada (throughput).

---

## 🎯 Prática Guiada - Conceito e Objetivos

**Conceito:** nesta prática guiada (*practice lab*), você vai:

- Explorar as opções de banco de dados da AWS.
- Lançar uma instância Amazon RDS.
- Configurar uma implantação Multi-AZ.
- Configurar backups no Amazon RDS.

**Objetivos da Prática:**

- Explorar as opções de banco de dados da AWS.
- Lançar uma instância Amazon RDS.
- Configurar uma implantação Multi-AZ.
- Configurar backups no Amazon RDS.

---

## 🛠️ Prática Guiada - Passo a Passo

### 1. Introdução ao lab

Revise as tarefas do lab na seção de Conceito, clique em **Start Lab** ou **Open AWS Console** para começar, e siga as instruções usando as setas para navegar entre os passos.

> ⚠️ Serviços da AWS não usados neste lab ficam desabilitados no ambiente. Além disso, as capacidades dos serviços usados são limitadas ao necessário para o lab.

### 2. Acessar o console do EC2

Na barra de busca do console, digite `ec2` e clique em **EC2** nos resultados.

> 💡 **Conceito:** o Amazon EC2 (Elastic Compute Cloud) é um serviço web que fornece capacidade computacional segura e escalável na nuvem AWS.

### 3. Acessar o AMI Catalog

No menu à esquerda, clique em **AMI Catalog**.

> 💡 **Conceito - O que é uma AMI e por que revisá-la:** uma **AMI (Amazon Machine Image)** é uma "imagem" pronta que contém o software necessário para configurar e inicializar uma instância EC2 - ou seja, o sistema operacional já configurado, e opcionalmente pacotes adicionais (como um servidor de banco de dados) já instalados.
>
> Revisamos o AMI Catalog aqui porque é importante entender que existe **outro caminho** para se ter um banco de dados na AWS: você poderia lançar uma instância EC2 comum a partir de uma AMI (por exemplo, uma AMI com Linux) e instalar/configurar o banco de dados manualmente por conta própria (self-hosted). O AMI Catalog mostra justamente essa opção "crua", que exige mais trabalho manual - e é o contraponto que ajuda a entender o valor do Amazon RDS, que veremos a seguir.

### 4. Revisar AMIs com banco de dados (self-hosted) e entender a diferença para o RDS

1. Na caixa de busca de AMIs, digite `sql` e pressione Enter.
2. Na aba **Quick Start AMIs**, revise as AMIs disponíveis no Amazon EC2.
   - Dependendo da necessidade, você pode encontrar AMIs fornecidas diretamente pela AWS, por terceiros confiáveis no AWS Marketplace, ou pela comunidade AWS.

> 💡 **Conceito - EC2 self-hosted vs. Amazon RDS:** ao hospedar um banco de dados em uma instância EC2 (usando uma dessas AMIs, por exemplo), a AWS gerencia apenas a infraestrutura física, o hardware e o sistema operacional. **Você** fica responsável por gerenciar a instância, o próprio banco de dados, a otimização de consultas (queries) e os dados dos clientes.
>
> Já o **Amazon RDS** e outros serviços gerenciados reduzem essas responsabilidades (a AWS cuida de patching, backups, infraestrutura do banco, etc.), mas em troca oferecem menos opções de customização. É exatamente essa troca (menos trabalho operacional x menos controle fino) que motiva a proposta de solução deste lab.

### 5. Buscar os serviços de banco de dados

1. Na barra de busca superior, digite `database`.
2. Nos resultados, clique em **Show more**.

### 6. Conhecer as opções de banco de dados da AWS

Revise a lista de opções de banco de dados disponíveis e clique em **Aurora and RDS**.

> 💡 **Conceito:** os serviços de banco de dados da AWS atendem casos de uso diferentes:
> - **Amazon RDS**: gerenciamento de dados estruturado, baseado em SQL, para consultas e transações complexas.
> - **Amazon DynamoDB**: armazenamento flexível e sem schema fixo, para dados que mudam rapidamente e aplicações em grande escala.
> - **Amazon MemoryDB**: acesso a dados ultrarrápido, em memória, para aplicações em tempo real.

### 7. Criar o banco de dados

1. No menu à esquerda, clique em **Databases**.
2. Na página **Databases**, clique em **Create database**.
3. Na janela pop-up, escolha **Full configuration**.

> 💡 **Conceito:** o Amazon RDS ajuda a gerenciar bancos de dados relacionais na nuvem, automatizando o provisionamento de hardware, a configuração do banco, o patching e os backups. Ele oferece capacidade redimensionável com custo eficiente.

### 8. Escolher o método de criação e a engine

1. Em **Choose a database creation method**, escolha **Full configuration**.
2. Em **Engine type**, escolha **MariaDB**.

> 💡 **Conceito:** o Amazon RDS suporta múltiplas engines de banco de dados, tanto comerciais quanto open source (MariaDB, MySQL, PostgreSQL, Oracle, SQL Server, entre outras).

### 9. Configurar template, versão e identificador

1. Em **Templates**, escolha ou mantenha **Dev/Test**.
2. Em **Engine version**, mantenha a versão padrão do MariaDB sugerida na lista suspensa.
   - A versão padrão no seu ambiente de prática pode ser diferente da mostrada no exemplo.
3. Em **DB instance identifier**, digite: `my-database`
   - Além de hífens, nenhum outro caractere especial é aceito no identificador da instância.

> 💡 **Conceito:** o Amazon RDS suporta versões major e minor da engine. Versões major recebem suporte por no mínimo 3 anos a partir do lançamento inicial. Versões minor recebem suporte por no mínimo 1 ano. O Amazon RDS descontinua versões quando elas chegam ao fim de vida da comunidade (community end of life) ou deixam de receber correções de software e atualizações de segurança.

### 10. Configurar credenciais

1. Em **Credential settings**, para **Master username**, mantenha o padrão `admin`.
2. Em **Credentials management**, escolha **Self managed**.
3. Em **Master password**, digite: `ILoveLearning!123`
4. Em **Confirm master password**, digite a senha novamente.

> 💡 **Conceito:** você pode usar o **AWS Secrets Manager** para gerenciar credenciais em produção. O Secrets Manager armazena credenciais de banco de dados, senhas, chaves de API e outros textos sensíveis, evitando que esses dados fiquem hardcoded na aplicação.

### 11. Configurar a classe da instância e o armazenamento

1. Na seção **Instance configuration**, em **DB instance class**, escolha **Burstable classes**.
2. Logo abaixo, na lista suspensa, escolha **db.t3.xlarge**.
   - Apenas as classes t3 são suportadas neste lab de prática.
3. Em **Storage type**, escolha **General Purpose SSD (gp3)**.
4. Em **Allocated storage**, digite: `20`

> ⚠️ **Nota (console em português):** se o console da AWS estiver em português, ao configurar a instância você vai encontrar a opção **Intermitente** em vez de **Burstable**. Isso pode confundir, pois a documentação e os labs em inglês usam o termo *Burstable*.
>
> **Burstable = Intermitente.** São as classes de instância (como a família t3, usada neste lab) que operam com um nível de performance de CPU baseline e acumulam "créditos de CPU" quando estão ociosas, podendo "estourar" (burst / usar de forma intermitente) acima do baseline em momentos de pico de demanda. É um bom custo-benefício para workloads que não precisam de uso intenso e constante de CPU - como é o caso de um banco de dados de Dev/Test.
>
> Portanto, para seguir exatamente o laboratório em português, selecione: ✅ **Intermitente**

### 12. Configurar armazenamento adicional e Multi-AZ

1. Clique para expandir **Additional storage configuration**.
2. Em **Storage autoscaling**, mantenha ou escolha o padrão **Enable storage autoscaling**.
3. Em **Maximum storage threshold**, mantenha ou escolha o padrão de **1000 GiB**.
4. Em **Multi-AZ deployment**, escolha **Create a standby instance**.

> 💡 **Conceito:** o Amazon RDS mantém uma instância standby síncrona em uma Availability Zone diferente, para alta disponibilidade e failover automático do banco de dados primário.

### 13. Configurar a rede (VPC)

1. Em **Virtual private cloud (VPC)**, mantenha a **Default VPC**.
2. Em **DB subnet group**, mantenha o padrão.
3. Em **Public access**, mantenha o padrão **No**.
4. Em **VPC security group (firewall)**, mantenha o padrão **Choose existing**.

> 💡 **Conceito:** você pode executar bancos de dados Amazon RDS dentro de uma VPC (Virtual Private Cloud) para controlar o acesso de rede e se conectar à sua infraestrutura através de uma VPN IPsec criptografada.

### 14. Configurar o monitoramento

1. Em **Monitoring**, escolha **Database Insights - Standard**.
2. Desmarque a caixa de seleção **Collect detailed database and per-query metrics**.
   - Se esse recurso ficar habilitado, você vai receber um erro de permissão ao tentar criar o banco de dados.

### 15. Configurar o monitoramento adicional

1. Clique para expandir **Additional monitoring settings**.
2. Em **Enhanced Monitoring**, desmarque a caixa de seleção **Enable Enhanced monitoring**.
   - Se esse recurso ficar habilitado, você vai receber um erro de permissão ao tentar criar o banco de dados.

### 16. Configurar o nome inicial do banco e revisar configurações adicionais

1. Clique para expandir **Additional configuration**.
2. Em **Initial database name**, digite: `my_database`
   - Perceba que o nome do banco de dados pode ser diferente do nome da instância configurado anteriormente.
3. Em **DB parameter group** e **Option group**, revise as opções padrão.
4. Em **Backup**, revise as opções padrão.

> 💡 **Conceito:** especifique um nome de banco de dados inicial ao criar uma instância RDS. Sem um nome de banco, a instância pode não funcionar corretamente.

### 17. Revisar criptografia e manutenção, e criar o banco

1. Em **Encryption**, revise as opções padrão de criptografia.
2. Em **Maintenance**, desmarque a caixa de seleção **Enable auto minor version upgrade**.
3. Em **Maintenance window**, mantenha o padrão **No preference**.
4. Role até o final da página e clique em **Create database**.
   - Se aparecerem pop-ups oferecendo add-ons, feche-os.
   - A instância de banco de dados RDS leva de 5 a 10 minutos para ser criada.

> 💡 **Conceito:** o Amazon RDS criptografa os dados em repouso usando AES-256, protegendo os dados armazenados, o I/O de disco e os snapshots.

### 18. Confirmar a criação do banco de dados

1. Após a criação do banco, aguarde de 5 a 10 minutos adicionais e clique no ícone de atualizar (refresh) na página **Databases**.
   - Após a criação, o status é exibido como **Modifying**.
2. Em **Status**, confirme que o status mudou para **Available**.
   - Se o status ainda não tiver mudado para Available, continue clicando no ícone de refresh a cada alguns minutos até que mude.
3. Clique em `my-database`.

### 19. Revisar as opções da instância (Actions)

1. Na seção **Summary**, em **DB identifier**, revise o identificador.
2. Clique em **Actions** para expandir a lista suspensa.
3. Revise as diferentes opções disponíveis.
   - Você vai voltar a essas opções, como **Create read replica**, na próxima seção **DIY** desta solução.

> 💡 **Conceito:** as read replicas do Amazon RDS são cópias assíncronas do banco de dados de origem, usadas para atender ao tráfego de leitura e aumentar o throughput (taxa de transferência) do sistema.

### 20. Acessar o console do AWS DMS

Na barra de busca do console, digite `dms` e clique em **Database Migration Service** nos resultados.

> 💡 **Conceito - O que é o AWS DMS e para que serve:** o **AWS Database Migration Service (AWS DMS)** é um serviço que migra dados **entre** repositórios de dados - por exemplo, de um banco de dados on-premises (ou de outro provedor de nuvem) para o Amazon RDS, ou entre engines de banco diferentes (ex.: Oracle para PostgreSQL).
>
> O grande diferencial é que o DMS realiza a migração **mantendo o banco de dados de origem totalmente operacional** durante todo o processo, minimizando o tempo de inatividade (downtime) da aplicação. É a ferramenta indicada quando a empresa decide finalmente mover seus bancos de produção para a AWS, como perguntou a Dra. Dee Tah no contexto do cenário.

### 21. Explorar a seção "Getting started" do DMS

No menu à esquerda, clique em **Getting started**.

### 22. Revisar as formas de uso do AWS DMS

1. Em **How you can use DMS**, escolha **Discover and assess**.
2. Logo abaixo, revise a descrição.
3. Repita a revisão para a outra opção disponível: **Migrate or replicate**.

> 💡 **Conceito:** o AWS DMS suporta migrações entre diferentes engines de banco de dados, incluindo bancos relacionais, data warehouses e bancos NoSQL. As duas formas principais de uso são: **descobrir e avaliar** (mapear e entender bancos de dados existentes antes de migrar) e **migrar ou replicar** (executar de fato a migração ou manter os dados replicados/sincronizados entre origem e destino).

### 23. Conclusão da Prática

Parabéns! Você concluiu a seção de Prática. O próximo passo é a seção **DIY** para completar a solução.

---

## 🧪 DIY (Do It Yourself)

### 🎯 Objetivos do DIY

1. Criar uma read replica do seu banco de dados primário, usando uma instância `db.t3.xlarge`.

### ✅ Método de Validação da Solução

O servidor de teste vai verificar se você criou uma read replica - chamada `my-database-read-replica` - a partir do banco de dados primário `my-database`.

> 💡 **Dica:** você pode usar qualquer nome para o identificador da instância da read replica (o teste aceita `my-database-read-replica` como referência, mas o importante é que ela seja criada a partir de `my-database`).

### 🔑 Informação necessária

- **Identificador do banco de dados primário:** `my-database` (criado na prática guiada).
- **Classe de instância da read replica:** `db.t3.xlarge`.

### 🛠️ Passo a Passo sugerido

Seguindo a mesma lógica aplicada na prática guiada (Passo 19, onde revisamos as opções em **Actions**), para criar a read replica:

**1. Acessar o banco de dados primário**

1. No console do Amazon RDS, acesse **Databases**.
2. Clique no identificador `my-database` para abrir os detalhes da instância.

**2. Iniciar a criação da read replica**

1. Clique em **Actions**.
2. Na lista suspensa, escolha **Create read replica**.

> 💡 **Conceito:** ao criar uma read replica, o Amazon RDS gera uma cópia assíncrona do banco de dados primário. As alterações no primário são replicadas para a réplica, mas com uma pequena latência (assíncrona, diferente da replicação síncrona usada no Multi-AZ). A read replica pode receber consultas de leitura (SELECT), liberando o banco primário para focar nas operações de escrita.

**3. Configurar a instância da read replica**

1. Em **DB instance identifier**, digite um nome de sua escolha, por exemplo: `my-database-read-replica`
2. Em **DB instance class**, escolha **Burstable classes** (Intermitente, se o console estiver em português).
3. Na lista suspensa, escolha **db.t3.xlarge**.
4. Revise as demais configurações de rede (VPC, subnet group) e mantenha os padrões, a menos que o lab indique o contrário.

**4. Criar a read replica**

1. Role até o final da página e clique em **Create read replica**.
   - A criação da read replica também pode levar alguns minutos.

**5. Confirmar a criação**

1. Volte para a página **Databases**.
2. Clique no ícone de atualizar (refresh) periodicamente até que o status da nova instância `my-database-read-replica` mude para **Available**.
3. Confirme, na coluna de detalhes, que a **Role** dessa instância aparece como **Read replica**, e que ela está associada ao banco de dados primário `my-database`.

---

## ✅ Conclusão

Com a conclusão da prática e do DIY, a empresa de seguros passa a ter:

- Um banco de dados **Amazon RDS** (`my-database`, engine MariaDB) totalmente gerenciado, eliminando a necessidade dos administradores lidarem manualmente com patching e infraestrutura.
- **Alta disponibilidade e recuperação de desastres** através da implantação **Multi-AZ**, com uma instância standby síncrona em outra Availability Zone e failover automático sem intervenção manual (o DNS da instância não muda durante o failover).
- **Melhora de performance para cargas de leitura intensa** através de uma **read replica** (`my-database-read-replica`), que assume as consultas de leitura da equipe de Data Analytics e libera o banco primário para as operações de escrita.
- Conhecimento sobre o **AWS DMS**, a ferramenta indicada para migrar bancos de dados de produção para a AWS mantendo o banco de origem operacional durante o processo.
