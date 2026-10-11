# 🐊 Crocky - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/102642c6-ee29-4e65-b1e1-a0fe2ce970a1" width="250" alt="Crocky Pet" />
</p>

- **Espécie:** Crocodilo / Jacaré (*Crocodile*)
- **Domínio AWS:** Tecnologia e Serviços em Nuvem (*Cloud Technology & Services*)
- **Tópico Principal:** Bases de Dados (Amazon RDS, Amazon Aurora e Amazon DynamoDB)
- **Total de Quizzes:** 12
- **Status:** Adotado ✅

---

### Quiz 1: Banco de Dados Relacional Gerenciado (Amazon RDS)
**Enunciado:** Uma empresa quer migrar seu banco de dados MySQL para a AWS sem ter que se preocupar com tarefas administrativas repetitivas, como aplicação de patches no sistema operacional, backups automáticos e provisionamento de hardware. Qual serviço atende a essa necessidade?

- [ ] **A)** Amazon DynamoDB
- [x] **B)** Amazon RDS (Relational Database Service)
- [ ] **C)** Amazon Redshift
- [ ] **D)** Amazon DocumentDB

> **Por que esta é a resposta correta?**
> * O **Amazon RDS** é um serviço totalmente gerenciado de banco de dados relacional que suporta múltiplos mecanismos (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server). Ele automatiza tarefas operacionais pesadas, como backups, correções de segurança e provisionamento de infraestrutura.
>
> 📖 **Documentação:** [O que é o Amazon RDS?](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/Welcome.html)

---

### Quiz 2: Alta Disponibilidade com Implantação Multi-AZ
**Enunciado:** Para garantir resiliência contra falhas no centro de dados físico, uma empresa precisa que seu banco de dados relacional no Amazon RDS realize failover automático e transparente para uma réplica secundária caso a instância primária falhe. Qual funcionalidade deve ser habilitada?

- [ ] **A)** Réplicas de Leitura (*Read Replicas*)
- [x] **B)** Implantação Multi-AZ (*Multi-AZ Deployment*)
- [ ] **C)** Tabelas Globais (*Global Tables*)
- [ ] **D)** Auto Scaling de Armazenamento

> **Por que esta é a resposta correta?**
> * A funcionalidade **Multi-AZ** cria uma cópia síncrona do banco de dados em uma Zona de Disponibilidade diferente. Em caso de falha na instância principal, a AWS realiza um failover automático para a réplica de standby sem necessidade de intervenção manual.
>
> 📖 **Documentação:** [Implantações Multi-AZ do Amazon RDS para alta disponibilidade](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)

---

### Quiz 3: Performance de Leitura com Read Replicas
**Enunciado:** Uma aplicação de e-commerce lê dados de produtos com muita frequência, causando alta carga na instância principal do banco de dados relacional. Como o arquiteto de soluções pode descarregar o tráfego de consulta/leitura mantendo a alta performance?

- [x] **A)** Criar Réplicas de Leitura (*Read Replicas*) no Amazon RDS.
- [ ] **B)** Habilitar a criptografia de dados em repouso.
- [ ] **C)** Aumentar o tamanho da sub-rede no Amazon VPC.
- [ ] **D)** Ativar o suporte a IPv6 na instância.

> **Por que esta é a resposta correta?**
> * As **Read Replicas** utilizam replicação assíncrona para permitir que requisições exclusivas de leitura sejam direcionadas para instâncias secundárias, aliviando a carga da instância primária (que fica dedicada às gravações e atualizações).
>
> 📖 **Documentação:** [Trabalhar com réplicas de leitura do Amazon RDS](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/USER_ReadRepl.html)

---

### Quiz 4: Amazon Aurora (Alta Performance Relacional)
**Enunciado:** Uma startup precisa de um banco de dados relacional compatível com MySQL/PostgreSQL que ofereça até 5 vezes o desempenho do MySQL padrão e replique dados automaticamente em 6 cópias distribuídas por 3 Zonas de Disponibilidade. Qual serviço é a melhor escolha?

- [ ] **A)** Amazon ElastiCache
- [ ] **B)** Amazon DynamoDB
- [x] **C)** Amazon Aurora
- [ ] **D)** Amazon Neptune

> **Por que esta é a resposta correta?**
> * O **Amazon Aurora** é o banco de dados relacional proprietário da AWS, projetado para alta performance na nuvem. Ele armazena 6 cópias dos seus dados em 3 AZs e é totalmente compatível com MySQL e PostgreSQL a uma fração do custo dos bancos proprietários tradicionais.
>
> 📖 **Documentação:** [O que é o Amazon Aurora?](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)

---

### Quiz 5: Banco de Dados NoSQL de Chave-Valor (Amazon DynamoDB)
**Enunciado:** Um aplicativo mobile precisa armazenar preferências de usuários e sessões de jogos com latência consistente de milissegundos de um único dígito em qualquer escala, sem necessidade de esquema de dados rígido ou gerenciamento de servidores. Qual serviço NoSQL deve ser utilizado?

- [x] **A)** Amazon DynamoDB
- [ ] **B)** Amazon RDS para PostgreSQL
- [ ] **C)** AWS Glue
- [ ] **D)** Amazon Redshift

> **Por que esta é a resposta correta?**
> * O **Amazon DynamoDB** é um banco de dados NoSQL serverless de chave-valor e documentos que oferece desempenho escalável em milissegundos de um único dígito, sendo ideal para aplicações web, jogos e microsserviços.
>
> 📖 **Documentação:** [O que é o Amazon DynamoDB?](https://docs.aws.amazon.com/pt_br/amazondynamodb/latest/developerguide/Introduction.html)

---

### Quiz 6: DynamoDB Global Tables
**Enunciado:** Uma multinacional precisa disponibilizar dados gravados no DynamoDB para usuários localizados nas Américas, Europa e Ásia com latência local de leitura e gravação rápida em todas essas regiões. Qual recurso do DynamoDB viabiliza esse requisito?

- [ ] **A)** DynamoDB Accelerator (DAX)
- [x] **B)** Tabelas Globais (*Global Tables*)
- [ ] **C)** Streams do DynamoDB
- [ ] **D)** Backup em Ponto no Tempo (PITR)

> **Por que esta é a resposta correta?**
> * As **Global Tables** do DynamoDB fornecem um banco de dados totalmente gerenciado, multi-região e multi-ativo, replicando as atualizações automaticamente entre as regiões AWS selecionadas para garantir baixa latência aos usuários globais.
>
> 📖 **Documentação:** [Tabelas globais do DynamoDB](https://docs.aws.amazon.com/pt_br/amazondynamodb/latest/developerguide/GlobalTables.html)

---

### Quiz 7: Aceleração em Memória com DynamoDB Accelerator (DAX)
**Enunciado:** Um sistema de leitura intensiva no DynamoDB precisa responder a milhões de solicitações por segundo reduzindo o tempo de resposta do nível de milissegundos para o nível de **microssegundos**. Qual camada de cache em memória totalmente gerenciada deve ser adicionada?

- [ ] **A)** Amazon ElastiCache para Redis
- [x] **B)** Amazon DynamoDB Accelerator (DAX)
- [ ] **C)** Amazon CloudFront
- [ ] **D)** AWS Transit Gateway

> **Por que esta é a resposta correta?**
> * O **DAX (DynamoDB Accelerator)** é um cache em memória altamente disponível e dedicado ao DynamoDB que reduz os tempos de resposta de milissegundos para microssegundos sem exigir alterações na lógica de código da aplicação.
>
> 📖 **Documentação:** [Aceleração em memória com o DAX](https://docs.aws.amazon.com/pt_br/amazondynamodb/latest/developerguide/DAX.html)

---

### Quiz 8: Data Warehouse e Análise de Big Data (Amazon Redshift)
**Enunciado:** Uma equipe de Business Intelligence (BI) precisa executar consultas analíticas complexas (OLAP) sobre terabytes de dados históricos acumulados de vendas para gerar relatórios diários. Qual serviço de Data Warehouse em nuvem é adequado para essa finalidade?

- [ ] **A)** Amazon RDS
- [ ] **B)** Amazon Keyspaces
- [x] **C)** Amazon Redshift
- [ ] **D)** Amazon Athena

> **Por que esta é a resposta correta?**
> * O **Amazon Redshift** é um serviço de Data Warehouse colunar em escala de petabytes, otimizado para consultas analíticas complexas e integração com ferramentas de relatórios e BI.
>
> 📖 **Documentação:** [O que é o Amazon Redshift?](https://docs.aws.amazon.com/pt_br/redshift/latest/mgmt/welcome.html)

---

### Quiz 9: Cache em Memória com Amazon ElastiCache
**Enunciado:** Uma aplicação web baseada em PHP enfrenta lentidão no carregamento de sessões de usuários gravadas no banco de dados. O desenvolvedor deseja adicionar uma camada de armazenamento em memória sub-milissegunda usando Redis ou Memcached. Qual serviço gerenciado deve ser utilizado?

- [x] **A)** Amazon ElastiCache
- [ ] **B)** Amazon MemoryDB para Redis
- [ ] **C)** Amazon EFS
- [ ] **D)** AWS Storage Gateway

> **Por que esta é a resposta correta?**
> * O **Amazon ElastiCache** permite configurar e operar facilmente ambientes em memória compatíveis com Redis ou Memcached, acelerando a performance de aplicações ao servir dados frequentemente acessados diretamente da RAM.
>
> 📖 **Documentação:** [O que é o Amazon ElastiCache?](https://docs.aws.amazon.com/pt_br/AmazonElastiCache/latest/red-ug/WhatIs.html)

---

### Quiz 10: Banco de Dados de Grafos (Amazon Neptune)
**Enunciado:** Uma rede social precisa armazenar e consultar relações complexas entre usuários, como "amigos em comum", recomendações de conexões e detecção de fraudes em cadeias de transações. Qual tipo de banco de dados gerenciado é otimizado para esse modelo de dados altamente interconectado?

- [ ] **A)** Amazon DocumentDB (compatível com MongoDB)
- [x] **B)** Amazon Neptune
- [ ] **C)** Amazon Timestream
- [ ] **D)** Amazon QLDB

> **Por que esta é a resposta correta?**
> * O **Amazon Neptune** é um serviço de banco de dados de grafos altamente disponível e otimizado para armazenar e navegar em relacionamentos complexos entre bilhões de nós e arestas com alta performance.
>
> 📖 **Documentação:** [O que é o Amazon Neptune?](https://docs.aws.amazon.com/pt_br/neptune/latest/userguide/intro.html)

---

### Quiz 11: Migração de Bancos de Dados com AWS DMS
**Enunciado:** Uma empresa precisa migrar seu banco de dados Oracle *on-premises* para um banco Amazon Aurora na AWS minimizando o tempo de inatividade (*downtime*) durante o processo. Qual serviço é projetado para realizar essa migração de dados de forma contínua?

- [ ] **A)** AWS DataSync
- [x] **B)** AWS Database Migration Service (AWS DMS)
- [ ] **C)** AWS Application Migration Service (MGN)
- [ ] **D)** AWS Snowcone

> **Por que esta é a resposta correta?**
> * O **AWS DMS** ajuda a migrar bancos de dados para a AWS de forma rápida e segura. O banco de dados de origem permanece totalmente operacional durante a migração, minimizando a interrupção das aplicações dependentes.
>
> 📖 **Documentação:** [O que é o AWS Database Migration Service?](https://docs.aws.amazon.com/pt_br/dms/latest/userguide/Welcome.html)

---

### Quiz 12: Servidores de Banco de Dados Gerenciados x Não Gerenciados (EC2)
**Enunciado:** Qual é a principal diferença entre executar um banco de dados MySQL em uma instância Amazon EC2 (*Self-managed*) versus utilizar o Amazon RDS (*Fully-managed*)?

- [ ] **A)** O EC2 não permite instalar o mecanismo MySQL.
- [x] **B)** No EC2, o usuário é responsável por patches do SO, backups e failover; no RDS, a AWS gerencia essas tarefas operacionais.
- [ ] **C)** O Amazon RDS não permite conexões vindas da internet.
- [ ] **D)** O EC2 é um serviço Serverless que não cobra por hora de execução.

> **Por que esta é a resposta correta?**
> * De acordo com o Modelo de Responsabilidade Compartilhada, rodar um banco no EC2 coloca a responsabilidade de patches de sistema operacional, atualizações do mecanismo de banco, backups e estratégias de alta disponibilidade nas mãos do usuário. No RDS, a AWS assume essa gestão de infraestrutura e plataforma.
>
> 📖 **Documentação:** [Escolhendo entre Amazon EC2 e Amazon RDS](https://aws.amazon.com/pt/relational-database/)
