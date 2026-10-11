# 🦍 Mrs. Gorilla - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/a05676a5-5184-4f65-9f74-bd16547aab8d" width="250" alt="Mrs Gorilla Pet" />
</p>

- **Espécie:** Gorila (*Gorilla*)
- **Domínio AWS:** Tecnologia e Serviços em Nuvem (*Cloud Technology & Services*)
- **Tópico Principal:** Computação (Amazon EC2, AMIs, Tipos de Instâncias e Modelos de Compra)
- **Total de Quizzes:** 12
- **Status:** Adotada ✅

---

### Quiz 1: Conceito do Amazon EC2
**Enunciado:** Uma empresa precisa implantar uma aplicação web legada que exige controle total sobre o sistema operacional (Linux ou Windows), acesso root/administrador e instalação de dependências customizadas. Qual serviço de computação em nuvem fornece essas máquinas virtuais?

- [ ] **A)** AWS Lambda
- [x] **B)** Amazon Elastic Compute Cloud (Amazon EC2)
- [ ] **C)** AWS Fargate
- [ ] **D)** Amazon Elastic Container Service (Amazon ECS)

> **Por que esta é a resposta correta?**
> * O **Amazon EC2** é o serviço que fornece capacidade de computação reconfigurável na nuvem na forma de máquinas virtuais (instâncias). Ele garante controle total sobre o sistema operacional, permissões de acesso e configurações de rede.
>
> 📖 **Documentação:** [O que é o Amazon EC2?](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/concepts.html)

---

### Quiz 2: Modelo de Compra sob Demanda (On-Demand)
**Enunciado:** Uma startup está desenvolvendo um novo produto com padrão de tráfego imprevisível e curtas sessões de teste. A equipe não quer firmar compromissos de longo prazo nem pagar taxas antecipadas. Qual modelo de cobrança do EC2 deve ser utilizado?

- [x] **A)** Instâncias sob demanda (*On-Demand Instances*)
- [ ] **B)** Instâncias reservadas (*Reserved Instances*)
- [ ] **C)** Instâncias Spot (*Spot Instances*)
- [ ] **D)** Hosts dedicados (*Dedicated Hosts*)

> **Por que esta é a resposta correta?**
> * As **Instâncias On-Demand** permitem pagar pela capacidade de computação por segundo ou por hora, sem compromissos de longo prazo nem pagamentos antecipados. É ideal para cargas de trabalho de curta duração, imprevisíveis ou com picos que não podem ser interrompidos.
>
> 📖 **Documentação:** [Instâncias sob demanda do Amazon EC2](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/ec2-on-demand-instances.html)

---

### Quiz 3: Redução de Custos com Cargas Continuas (Reserved Instances & Savings Plans)
**Enunciado:** Uma empresa possui um banco de dados e um servidor de aplicação que rodam continuamente 24 horas por dia, 7 dias por semana, com previsão de uso pelos próximos 3 anos. Como obter o maior desconto possível sobre o preço padrão On-Demand para essa carga de trabalho previsível?

- [ ] **A)** Usar exclusivamente Instâncias Spot.
- [x] **B)** Adquirir *Savings Plans* ou *Instâncias Reservadas (Reserved Instances)* com compromisso de 1 a 3 anos.
- [ ] **C)** Executar as máquinas em múltiplos Locais de Borda (*Edge Locations*).
- [ ] **D)** Criar um grupo de Auto Scaling configurado para zero instâncias no fim de semana.

> **Por que esta é a resposta correta?**
> * Os **Compute Savings Plans** e as **Instâncias Reservadas** oferecem descontos significativos (de até 72%) em comparação com os preços On-Demand, em troca do compromisso de uso de um volume consistente de computação por um período de 1 ou 3 anos.
>
> 📖 **Documentação:** [Instâncias reservadas do Amazon EC2](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/ec2-instance-purchasing-options.html)

---

### Quiz 4: Cargas Toleras a Interrupções ao Menor Custo (Spot Instances)
**Enunciado:** Uma equipe de ciência de dados precisa rodar trabalhos pesados de renderização de vídeo e processamento em lote (*batch processing*) que podem ser pausados e reiniciados a qualquer momento. O objetivo principal é minimizar o custo ao máximo. Qual modelo de compra do EC2 é recomendado?

- [ ] **A)** Instâncias sob demanda (*On-Demand*)
- [ ] **B)** Hosts dedicados (*Dedicated Hosts*)
- [x] **C)** Instâncias Spot (*Spot Instances*)
- [ ] **D)** Instâncias reservadas conversíveis (*Convertible RIs*)

> **Por que esta é a resposta correta?**
> * As **Instâncias Spot** aproveitam a capacidade ociosa do EC2 disponível na nuvem AWS com descontos de até 90% em relação ao On-Demand. A contrapartida é que a AWS pode interromper a instância com um aviso prévio de 2 minutos se precisar da capacidade de volta.
>
> 📖 **Documentação:** [Instâncias Spot do Amazon EC2](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/using-spot-instances.html)

---

### Quiz 5: Requisitos Estreitos de Licenciamento e Conformidade (Dedicated Hosts)
**Enunciado:** Uma empresa precisa migrar um software corporativo que possui licenças vinculadas ao número de soquetes ou núcleos de processadores físicos (*BYOL - Bring Your Own License*), além de exigir isolamento físico completo no nível de hardware. Qual opção atende a essa conformidade?

- [ ] **A)** Instâncias On-Demand compartilhadas.
- [ ] **B)** Instâncias Spot em uma sub-rede privada.
- [x] **C)** Hosts dedicados do EC2 (*EC2 Dedicated Hosts*)
- [ ] **D)** Grupos de Auto Scaling Multi-AZ.

> **Por que esta é a resposta correta?**
> * O **EC2 Dedicated Host** é um servidor físico dedicado para uso exclusivo do cliente. Ele permite controle total sobre a alocação de instâncias dentro do servidor físico e ajuda a cumprir requisitos de conformidade rigorosos e políticas de licenciamento por soquete/núcleo.
>
> 📖 **Documentação:** [Hosts dedicados do Amazon EC2](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/dedicated-hosts-overview.html)

---

### Quiz 6: Famílias de Instâncias — Otimizadas para Memória
**Enunciado:** Um arquiteto precisa selecionar a família de instâncias EC2 adequada para hospedar um banco de dados em memória (*in-memory database* como Redis/Memcached) e aplicações de análise de Big Data em tempo real que exigem grande capacidade de RAM. Qual família de instâncias deve ser escolhida?

- [ ] **A)** C (Compute Optimized - ex: `c6i`)
- [x] **B)** R ou X (Memory Optimized - ex: `r6g`, `x2idn`)
- [ ] **C)** I ou D (Storage Optimized - ex: `i3en`)
- [ ] **D)** T ou M (General Purpose - ex: `t4g`, `m6i`)

> **Por que esta é a resposta correta?**
> * As instâncias **Otimizadas para Memória (famílias R, X e z)** são projetadas para entregar desempenho rápido para cargas de trabalho que processam grandes conjuntos de dados na memória RAM.
>
> 📖 **Documentação:** [Tipos de instâncias EC2 otimizadas para memória](https://aws.amazon.com/pt/ec2/instance-types/#Memory_Optimized)

---

### Quiz 7: Imagens de Máquina da Amazon (AMIs)
**Enunciado:** Uma equipe de infraestrutura deseja padronizar a criação de novos servidores de aplicação na AWS, garantindo que todas as novas instâncias iniciem com o mesmo sistema operacional, patches de segurança e softwares pré-instalados. Qual componente serve como esse "molde" ou modelo básico?

- [x] **A)** Amazon Machine Image (AMI)
- [ ] **B)** Amazon Elastic Block Store (EBS) Snapshot isolado
- [ ] **C)** Modelo do AWS CloudFormation
- [ ] **D)** Script de User Data

> **Por que esta é a resposta correta?**
> * Uma **AMI (Amazon Machine Image)** fornece as informações necessárias para iniciar uma instância, incluindo o modelo do sistema operacional, servidor de aplicação, volumes de dados e permissões associadas. Funciona como um modelo pré-configurado da máquina.
>
> 📖 **Documentação:** [Imagens de Máquina da Amazon (AMI)](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/AMIs.html)

---

### Quiz 8: Automação no Boot da Instância (EC2 User Data)
**Enunciado:** Ao lançar uma nova instância EC2 Linux, o desenvolvedor precisa que o servidor execute automaticamente um script de inicialização de shell para atualizar os pacotes do SO e instalar o servidor web Apache (`httpd`) sem intervenção manual. Onde esse script deve ser inserido?

- [ ] **A)** No arquivo de configuração do Security Group.
- [x] **B)** No campo de Dados do Usuário (*EC2 User Data*).
- [ ] **C)** Na Tabela de Roteamento da VPC.
- [ ] **D)** Como parâmetro de tag da AMI.

> **Por que esta é a resposta correta?**
> * O **User Data** permite especificar scripts ou dados de configuração que são passados para a instância EC2 no momento da inicialização. Por padrão, os scripts de User Data são executados no primeiro boot da instância com privilégios de root.
>
> 📖 **Documentação:** [Executar comandos na instância EC2 na inicialização usando User Data](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/user-data-scripts.html)

---

### Quiz 9: Grupos de Posicionamento (Placement Groups - Cluster)
**Enunciado:** Uma aplicação de simulação científica de alto desempenho (HPC) exige que dezenas de instâncias EC2 se comuniquem com latência de rede extremamente baixa e alta taxa de transferência de gigabits por segundo entre si. Qual estratégia de posicionamento deve ser configurada?

- [x] **A)** Grupo de Posicionamento de Cluster (*Cluster Placement Group*)
- [ ] **B)** Grupo de Posicionamento Espalhado (*Spread Placement Group*)
- [ ] **C)** Grupo de Posicionamento em Partição (*Partition Placement Group*)
- [ ] **D)** Distribuição Multi-Região do Route 53

> **Por que esta é a resposta correta?**
> * O **Cluster Placement Group** agrupa instâncias fisicamente próximas dentro da mesma Zona de Disponibilidade. Essa estratégia reduz drasticamente a latência e oferece a maior taxa de transferência de rede entre as instâncias do grupo.
>
> 📖 **Documentação:** [Grupos de posicionamento do Amazon EC2](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/placement-groups.html)

---

### Quiz 10: Armazenamento Temporário de Altíssima Velocidade (Instance Store)
**Enunciado:** Uma aplicação de processamento de vídeos precisa de um volume de disco local para gravar buffers temporários e arquivos de swap com a menor latência de E/S possível. Os dados não precisam ser mantidos se a instância for encerrada. Qual tipo de armazenamento atende a esse requisito?

- [ ] **A)** Volume EBS do tipo `gp3`
- [ ] **B)** Amazon Elastic File System (EFS)
- [x] **C)** Armazenamento de Bloco de Instância (*EC2 Instance Store*)
- [ ] **D)** Bucket Amazon S3 Standard

> **Por que esta é a resposta correta?**
> * O **Instance Store** fornece armazenamento em bloco temporário de alto desempenho diretamente anexado aos discos físicos do servidor hospedeiro. Por ser efêmero, os dados são perdidos se a instância for parada ou encerrada.
>
> 📖 **Documentação:** [Armazenamento de blocos de instâncias do Amazon EC2](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/InstanceStorage.html)

---

### Quiz 11: Elasticidade Automática (Amazon EC2 Auto Scaling)
**Enunciado:** Um portal de notícias sofre picos imprevisíveis de tráfego de usuários durante breaking news. O administrador deseja que novas instâncias EC2 sejam adicionadas automaticamente quando o uso médio de CPU ultrapassar 70% e removidas quando a demanda diminuir. Qual serviço realiza esse escalonamento?

- [ ] **A)** AWS Elastic Beanstalk
- [x] **B)** Amazon EC2 Auto Scaling
- [ ] **C)** Elastic Load Balancing (ELB)
- [ ] **D)** AWS Systems Manager

> **Por que esta é a resposta correta?**
> * O **Amazon EC2 Auto Scaling** ajuda a garantir que você tenha o número correto de instâncias EC2 disponíveis para lidar com a carga de sua aplicação. Ele pode aumentar (*scale out*) ou reduzir (*scale in*) o número de instâncias automaticamente com base em métricas do CloudWatch.
>
> 📖 **Documentação:** [O que é o Amazon EC2 Auto Scaling?](https://docs.aws.amazon.com/pt_br/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)

---

### Quiz 12: Distribuição de Tráfego de Entrada (Elastic Load Balancing - ELB)
**Enunciado:** Para garantir alta disponibilidade, uma aplicação web roda em 4 instâncias EC2 distribuídas em duas Zonas de Disponibilidade. Qual componente distribui automaticamente o tráfego de dados de entrada dos clientes entre essas instâncias de forma saudável?

- [ ] **A)** Amazon Route 53 Traffic Flow
- [ ] **B)** AWS Transit Gateway
- [x] **C)** Application Load Balancer (ALB) / Elastic Load Balancing
- [ ] **D)** Amazon CloudFront

> **Por que esta é a resposta correta?**
> * O **Elastic Load Balancing (ELB)** distribui automaticamente o tráfego de entrada de aplicações entre vários destinos, como instâncias EC2, contêineres e endereços IP, realizando verificações de integridade (*health checks*) para rotear o tráfego apenas para instâncias saudáveis.
>
> 📖 **Documentação:** [O que é o Elastic Load Balancing?](https://docs.aws.amazon.com/pt_br/elasticloadbalancing/latest/userguide/what-is-load-balancing.html)
