# 🐰 Racer - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/3c855b52-2367-425e-a3fc-2525a4e74ed3" width="250" alt="Racer Pet" />
</p>

- **Espécie:** Coelho (*Rabbit*)
- **Domínio AWS:** Conceitos de Nuvem e Arquitetura (*Cloud Concepts & Architecture*)
- **Tópico Principal:** Princípios de Arquitetura em Nuvem e AWS Well-Architected Framework
- **Total de Quizzes:** 12
- **Status:** Adotado ✅

---

### Quiz 1: O Pilar de Excelência Operacional (*Operational Excellence*)
**Enunciado:** Uma equipe de engenharia deseja automatizar a implantação de infraestrutura por meio de código (*Infrastructure as Code - IaC*), realizar alterações pequenas e reversíveis e aprender continuamente com as falhas operacionais. Qual pilar do AWS Well-Architected Framework abrange essas práticas?

- [x] **A)** Excelência Operacional (*Operational Excellence*)
- [ ] **B)** Eficiência de Desempenho (*Performance Efficiency*)
- [ ] **C)** Confiabilidade (*Reliability*)
- [ ] **D)** Sustentabilidade (*Sustainability*)

> **Por que esta é a resposta correta?**
> * O pilar de **Excelência Operacional** se concentra na execução e monitoramento de sistemas para agregar valor ao negócio e na melhoria contínua de processos e procedimentos operacionais (como automação com IaC e resposta a eventos).
>
> 📖 **Documentação:** [Pilar Excelência Operacional - AWS Well-Architected](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/operational-excellence-pillar/welcome.html)

---

### Quiz 2: O Pilar de Segurança (*Security*)
**Enunciado:** Aplicar o princípio do menor privilégio no IAM, criptografar dados em repouso e em trânsito, manter rastreabilidade de ações com logs e proteger a infraestrutura em camadas são princípios norteadores de qual pilar do Framework?

- [ ] **A)** Confiabilidade (*Reliability*)
- [x] **B)** Segurança (*Security*)
- [ ] **C)** Otimização de Custos (*Cost Optimization*)
- [ ] **D)** Excelência Operacional (*Operational Excellence*)

> **Por que esta é a resposta correta?**
> * O pilar de **Segurança** foca na proteção de informações, sistemas e ativos, oferecendo uma abordagem baseada em gerenciamento de identidades, confidencialidade, integridade dos dados e proteção de rede.
>
> 📖 **Documentação:** [Pilar Segurança - AWS Well-Architected](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/security-pillar/welcome.html)

---

### Quiz 3: O Pilar de Confiabilidade (*Reliability*)
**Enunciado:** Uma empresa projetou sua aplicação para se recuperar automaticamente de falhas de hardware, testar procedimentos de recuperação de desastres (DR) e dimensionar horizontalmente para aumentar a disponibilidade agregada do sistema. Qual pilar orienta essas decisões de resiliência?

- [x] **A)** Confiabilidade (*Reliability*)
- [ ] **B)** Eficiência de Desempenho (*Performance Efficiency*)
- [ ] **C)** Sustentabilidade (*Sustainability*)
- [ ] **D)** Otimização de Custos (*Cost Optimization*)

> **Por que esta é a resposta correta?**
> * O pilar de **Confiabilidade** garante que uma carga de trabalho execute suas funções pretendidas corretamente e de forma consistente quando esperado, incluindo a capacidade de operar e se recuperar de falhas de infraestrutura ou rede.
>
> 📖 **Documentação:** [Pilar Confiabilidade - AWS Well-Architected](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/reliability-pillar/welcome.html)

---

### Quiz 4: O Pilar de Eficiência de Desempenho (*Performance Efficiency*)
**Enunciado:** Uma equipe escolheu usar bancos de dados NoSQL (DynamoDB) em vez de bancos relacionais para consultas de sub-milissegundo, além de utilizar uma CDN (CloudFront) para aproximar o conteúdo dos usuários finais. Qual pilar foca no uso eficiente de recursos computacionais?

- [ ] **A)** Otimização de Custos (*Cost Optimization*)
- [x] **B)** Eficiência de Desempenho (*Performance Efficiency*)
- [ ] **C)** Confiabilidade (*Reliability*)
- [ ] **D)** Excelência Operacional (*Operational Excellence*)

> **Por que esta é a resposta correta?**
> * O pilar de **Eficiência de Desempenho** foca na utilização eficiente de recursos de computação para atender aos requisitos do sistema e na manutenção dessa eficiência à medida que a demanda muda e as tecnologias evoluem.
>
> 📖 **Documentação:** [Pilar Eficiência de Desempenho - AWS Well-Architected](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/performance-efficiency-pillar/welcome.html)

---

### Quiz 5: O Pilar de Otimização de Custos (*Cost Optimization*)
**Enunciado:** Uma startup analisa continuamente seus gastos na nuvem, elimina recursos ociosos, adota instâncias Spot para tarefas tolerantes a falhas e utiliza Savings Plans para cobrir cargas de trabalho contínuas. Qual pilar prioriza a entrega de valor ao menor preço?

- [ ] **A)** Sustentabilidade (*Sustainability*)
- [ ] **B)** Eficiência de Desempenho (*Performance Efficiency*)
- [x] **C)** Otimização de Custos (*Cost Optimization*)
- [ ] **D)** Confiabilidade (*Reliability*)

> **Por que esta é a resposta correta?**
> * O pilar de **Otimização de Custos** enfatiza a capacidade de executar sistemas para entregar valor de negócio pelo menor preço possível, evitando custos desnecessários e utilizando modelos de pagamento adequados.
>
> 📖 **Documentação:** [Pilar Otimização de Custos - AWS Well-Architected](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/cost-optimization-pillar/welcome.html)

---

### Quiz 6: O Pilar de Sustentabilidade (*Sustainability*)
**Enunciado:** Uma corporação deseja minimizar o impacto ambiental direto de suas cargas de trabalho na nuvem, reduzindo o consumo de energia, maximizando a utilização dos recursos contratados e adotando regiões AWS abastecidas por energia renovável. Qual pilar aborda esse impacto a longo prazo?

- [x] **A)** Sustentabilidade (*Sustainability*)
- [ ] **B)** Otimização de Custos (*Cost Optimization*)
- [ ] **C)** Excelência Operacional (*Operational Excellence*)
- [ ] **D)** Segurança (*Security*)

> **Por que esta é a resposta correta?**
> * O pilar de **Sustentabilidade** concentra-se em minimizar os impactos ambientais de executar cargas de trabalho na nuvem. Os tópicos incluem o modelo de responsabilidade compartilhada para sustentabilidade e o alinhamento com metas de redução de pegada de carbono.
>
> 📖 **Documentação:** [Pilar Sustentabilidade - AWS Well-Architected](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/sustainability-pillar/welcome.html)

---

### Quiz 7: Avaliação de Cargas de Trabalho (AWS Well-Architected Tool)
**Enunciado:** Um arquiteto de soluções precisa documentar e revisar o estado das arquiteturas de suas aplicações em relação às melhores práticas da AWS, identificando riscos de alto/médio impacto (*High/Medium Risks*) e obtendo um plano de ação para correção. Qual ferramenta oficial no console realiza essa auditoria?

- [ ] **A)** AWS Trusted Advisor
- [x] **B)** AWS Well-Architected Tool
- [ ] **C)** AWS Config
- [ ] **D)** AWS Systems Manager

> **Por que esta é a resposta correta?**
> * A **AWS Well-Architected Tool** é um serviço gratuito que ajuda a revisar o estado de suas cargas de trabalho e as compara com as melhores práticas arquitetônicas mais recentes da AWS, fornecendo um plano de orientação para mitigar riscos identificados.
>
> 📖 **Documentação:** [O que é a AWS Well-Architected Tool?](https://docs.aws.amazon.com/pt_br/wellarchitected/latest/userguide/intro.html)

---

### Quiz 8: Conceito de Design de Nuvem — Escalabilidade Horizontal (*Scale Out*)
**Enunciado:** Em vez de aumentar o tamanho de um único servidor muito grande e caro (*escalonamento vertical*), a melhor prática na nuvem recomenda adicionar múltiplos servidores menores em paralelo à medida que a demanda cresce (*escalonamento horizontal*). Qual é o benefício dessa abordagem?

- [ ] **A)** Elimina a necessidade de utilizar grupos de segurança.
- [x] **B)** Aumenta a resiliência do sistema, reduz o ponto único de falha (*SPOF*) e permite elasticidade sob demanda.
- [ ] **C)** Garante que todas as chamadas de API sejam executadas sem autenticação.
- [ ] **D)** Impede o uso de balanceadores de carga.

> **Por que esta é a resposta correta?**
> * O **Escalonamento Horizontal** distribui o tráfego e o processamento em múltiplas instâncias menores. Isso reduz o risco de indisponibilidade total caso um servidor falhe e permite adaptar a capacidade dinamicamente via Auto Scaling.
>
> 📖 **Documentação:** [Princípios de design de arquitetura na nuvem AWS](https://docs.aws.amazon.com/pt_br/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html)

---

### Quiz 9: Desacoplamento de Arquitetura (*Loose Coupling*)
**Enunciado:** Duas aplicações em nuvem se comunicam de forma síncrona; se uma cair, a outra também falha imediatamente. Para tornar a arquitetura mais tolerante a falhas, o arquiteto decide introduzir uma fila Amazon SQS entre elas. Qual princípio de design arquitetônico foi aplicado?

- [x] **A)** Desacoplamento de componentes (*Loose Coupling*)
- [ ] **B)** Acoplamento rígido (*Tight Coupling*)
- [ ] **C)** Migração Lift-and-Shift
- [ ] **D)** Roteamento Estático

> **Por que esta é a resposta correta?**
> * O **Desacoplamento (Loose Coupling)** garante que os componentes de uma aplicação funcionem de forma independente. Se um componente falhar ou ficar lento, os outros continuam operando normalmente sem interromper o sistema inteiro.
>
> 📖 **Documentação:** [Desacoplamento de componentes de aplicação na AWS](https://docs.aws.amazon.com/pt_br/whitepapers/latest/architecting-for-the-cloud/loose-coupling.html)

---

### Quiz 10: Estratégia de Hospedagem Multi-AZ (Multi-Availability Zone)
**Enunciado:** Uma empresa implantou seu banco de dados Amazon RDS configurado em modo Multi-AZ. Qual é o principal benefício dessa decisão arquitetônica no pilar de Confiabilidade?

- [ ] **A)** Reduzir o custo de armazenamento do banco de dados em 50%.
- [x] **B)** Garantir Alta Disponibilidade e Failover automático para a réplica secundária se a Zona de Disponibilidade principal falhar.
- [ ] **C)** Permitir conexões de leitura ultra-rápidas para clientes em continentes diferentes.
- [ ] **D)** Desativar a necessidade de backups diários.

> **Por que esta é a resposta correta?**
> * As implantações **Multi-AZ** fornecem alta disponibilidade e durabilidade para instâncias de banco de dados. Em caso de falha da infraestrutura primária, o RDS realiza um *failover* automático para a réplica em outra Zona de Disponibilidade sem intervenção humana.
>
> 📖 **Documentação:** [Implantações Multi-AZ do Amazon RDS para alta disponibilidade](https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)

---

### Quiz 11: automação de Infraestrutura com AWS CloudFormation
**Enunciado:** Um dos princípios de design da nuvem é "Automatizar para facilitar a experimentação arquitetônica". Qual serviço da AWS permite definir toda a sua infraestrutura de rede, servidores e bancos de dados através de modelos em arquivos JSON ou YAML?

- [x] **A)** AWS CloudFormation
- [ ] **B)** AWS CodeDeploy
- [ ] **C)** AWS OpsWorks
- [ ] **D)** AWS Systems Manager Parameter Store

> **Por que esta é a resposta correta?**
> * O **AWS CloudFormation** permite que você modele, provisione e gerencie toda a sua infraestrutura AWS e de terceiros como código (*Infrastructure as Code - IaC*), garantindo automação, consistência e fácil replicação do ambiente.
>
> 📖 **Documentação:** [O que é o AWS CloudFormation?](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/Welcome.html)

---

### Quiz 12: Arquiteturas Sem Servidor (*Serverless Architectures*)
**Enunciado:** Uma empresa deseja focar seu tempo de desenvolvimento exclusivamente nas regras de negócio da aplicação, sem gastar esforço com provisionamento de SO, dimensionamento de clusters, aplicação de patches de segurança ou capacidade ociosa. Qual modelo de arquitetura atende a essa visão?

- [ ] **A)** Arquitetura baseada em instâncias EC2 sob demanda.
- [x] **B)** Arquitetura Serverless (utilizando AWS Lambda, Amazon API Gateway e DynamoDB).
- [ ] **C)** Arquitetura híbrida baseada em AWS Direct Connect.
- [ ] **D)** Arquitetura em contêineres hospedados em instâncias EC2 dedicadas.

> **Por que esta é a resposta correta?**
> * Arquiteturas **Serverless** eliminam a necessidade de gerenciar servidores. A AWS lida com a disponibilidade, dimensionamento automático e manutenção da infraestrutura, permitindo pagar apenas pelo uso real dos recursos.
>
> 📖 **Documentação:** [Computação sem servidor na AWS (Serverless)](https://aws.amazon.com/pt/serverless/)
