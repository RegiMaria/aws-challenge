# 🪼 Pearl - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/a0da8ab0-aa2a-44ff-94d2-63e8379162f2" width="250" alt="Pearl Pet" />
</p>

- **Espécie:** Água-Viva (*Jellyfish*)
- **Domínio AWS:** Tecnologia e Serviços em Nuvem (*Cloud Technology & Services*)
- **Tópico Principal:** Armazenamento em Bloco e Ficheiros (Amazon EBS e Amazon EFS)
- **Total de Quizzes:** 6
- **Status:** Adotado ✅

---

### Quiz 1: Armazenamento em Bloco Persistente (Amazon EBS)
**Enunciado:** Um administrador precisa anexar um disco de armazenamento de blocos de alta performance a uma instância Amazon EC2 para hospedar o sistema operacional e bases de dados transacionais. O disco deve manter os dados salvos mesmo se a instância for desligada ou reiniciada. Qual serviço deve ser utilizado?

- [ ] **A)** Amazon S3
- [x] **B)** Amazon Elastic Block Store (Amazon EBS)
- [ ] **C)** Amazon EFS
- [ ] **D)** AWS Instance Store

> **Por que esta é a resposta correta?**
> * O **Amazon EBS** fornece volumes de armazenamento em bloco persistentes e de baixa latência para uso com instâncias Amazon EC2. Cada volume EBS é replicado automaticamente na sua Zona de Disponibilidade para proteger contra falhas de componentes.
> * *Incorretas:* O Instance Store é efêmero (perde dados se a instância for encerrada); S3 é armazenamento de objetos; EFS é armazenamento de ficheiros partilhados.
>
> 📖 **Documentação:** [O que é o Amazon EBS?](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/AmazonEBS.html)

---

### Quiz 2: Criação de Cópias de Segurança com EBS Snapshots
**Enunciado:** Uma equipe de engenharia precisa criar um backup incremental de um volume Amazon EBS de produção para salvaguardar os dados de um banco de dados relacional. Onde esses snapshots são armazenados de forma segura e durável pela AWS?

- [ ] **A)** Diretamente na RAM da instância EC2
- [x] **B)** No Amazon S3
- [ ] **C)** Numa fita física da AWS Storage Gateway
- [ ] **D)** Num disco local do EBS secundário

> **Por que esta é a resposta correta?**
> * Os **EBS Snapshots** são backups incrementais salvos automaticamente no **Amazon S3**. Eles tiram uma foto ponto no tempo dos dados do volume, permitindo restaurá-los facilmente ou criar novos volumes EBS a partir deles.
>
> 📖 **Documentação:** [Instantâneos do Amazon EBS (Snapshots)](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/EBSSnapshots.html)

---

### Quiz 3: Armazenamento Compartilhado POSIX (Amazon EFS)
**Enunciado:** Uma aplicação corporativa roda distribuída em dezenas de instâncias Amazon EC2 simultaneamente e todas precisam ler e gravar dados no **mesmo conjunto de ficheiros** com padrão de acesso compatível com POSIX ao mesmo tempo. Qual serviço de armazenamento compartilhado deve ser utilizado?

- [ ] **A)** Amazon EBS
- [ ] **B)** Amazon S3
- [x] **C)** Amazon Elastic File System (Amazon EFS)
- [ ] **D)** AWS Storage Gateway

> **Por que esta é a resposta correta?**
> * O **Amazon EFS** fornece um sistema de arquivos de arquivos compartilhado, simples e totalmente gerenciado baseado no protocolo NFS (compatível com POSIX), permitindo que centenas de instâncias EC2 em várias Zonas de Disponibilidade acessem os dados simultaneamente.
>
> 📖 **Documentação:** [O que é o Amazon EFS?](https://docs.aws.amazon.com/pt_br/efs/latest/ug/whatisefs.html)

---

### Quiz 4: Desempenho do EBS - Tipos de Volumes (gp3)
**Enunciado:** Uma aplicação web comum precisa de um volume de armazenamento em bloco econômico e de uso geral (*General Purpose*) que ofereça uma linha de base consistente de IOPS e largura de banda independentemente do tamanho do disco contratado. Qual tipo de volume EBS é o padrão recomendado?

- [ ] **A)** io2 (Provisioned IOPS)
- [x] **B)** gp3 (General Purpose SSD)
- [ ] **C)** st1 (Throughput Optimized HDD)
- [ ] **D)** sc1 (Cold HDD)

> **Por que esta é a resposta correta?**
> * O **gp3** é o tipo de volume SSD de uso geral mais recente do EBS, permitindo que os utilizadores configurem IOPS e largura de banda de forma independente do tamanho do volume, oferecendo melhor custo-benefício que o modelo anterior `gp2`.
>
> 📖 **Documentação:** [Volumes SSD de uso geral (gp2 e gp3)](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/ebs-volume-types.html#gp3)

---

### Quiz 5: Alta Performance Intensiva em IOPS (Provisioned IOPS - io2)
**Enunciado:** Um banco de dados relacional de missão crítica exige desempenho ultra-alto e previsível com suporte a dezenas de milhares de operações de entrada e saída por segundo (IOPS) e baixíssima latência constante. Qual tipo de volume EBS atende a essa exigência rigorosa?

- [ ] **A)** gp2 (General Purpose SSD)
- [x] **B)** io1 / io2 Block Express (Provisioned IOPS SSD)
- [ ] **C)** st1 (Throughput Optimized HDD)
- [ ] **D)** Amazon S3 Glacier Flexible Retrieval

> **Por que esta é a resposta correta?**
> * Os volumes **Provisioned IOPS (io1 e io2)** são projetados para cargas de trabalho de bancos de dados pesados e sensíveis à latência que exigem desempenho sustentado de IOPS superior ao que os discos de uso geral conseguem entregar.
>
> 📖 **Documentação:** [Volumes SSD de IOPS provisionadas (io1 e io2)](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/ebs-volume-types.html#iobs)

---

### Quiz 6: Ciclo de Vida e Classes de Armazenamento do EFS
**Enunciado:** Uma empresa armazena terabytes de ficheiros no Amazon EFS que são acessados raramente após os primeiros 30 dias. Como automatizar a redução de custos de armazenamento sem precisar mover os dados manualmente para outra aplicação?

- [ ] **A)** Ativar o AWS Backup para apagar os ficheiros antigos.
- [x] **B)** Habilitar as Políticas de Ciclo de Vida do EFS (*EFS Lifecycle Management*) para mover dados para a classe Infrequent Access (IA).
- [ ] **C)** Configurar regras de ciclo de vida do Amazon S3 num volume EBS.
- [ ] **D)** Desligar o Amazon EFS nos fins de semana.

> **Por que esta é a resposta correta?**
> * O **EFS Lifecycle Management** monitora os padrões de acesso aos arquivos e migra automaticamente os dados que não foram acessados há um determinado período (ex: 30 ou 90 dias) para a classe de armazenamento de acesso infrequente (*EFS Infrequent Access - IA*), gerando economias de até 92%.
>
> 📖 **Documentação:** [Gerenciamento do ciclo de vida do Amazon EFS](https://docs.aws.amazon.com/pt_br/efs/latest/ug/lifecycle-management-efs.html)
