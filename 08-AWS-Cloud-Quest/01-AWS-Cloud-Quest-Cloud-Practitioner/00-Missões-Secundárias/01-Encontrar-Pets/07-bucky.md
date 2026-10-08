# 🦌 Bucky - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/a692961a-1901-4ffd-83db-ed7409c89221" width="250" alt="Bucky Pet" />
</p>

- **Espécie:** Cervo / Veado (*Deer*)
- **Domínio AWS:** Faturamento, Preços e Gestão (*Billing, Pricing & Management*)
- **Tópico Principal:** Governança, Automação e Infraestrutura como Código (AWS Organizations & AWS CloudFormation)
- **Total de Quizzes:** 3 (Expandido para 10 para estudo prático)
- **Status:** Adotado ✅

---

### Quiz 1: Infraestrutura como Código (IaC) com AWS CloudFormation
**Enunciado:** Um engenheiro de DevOps precisa criar e implantar repetidamente uma pilha completa de recursos AWS (incluindo instâncias EC2, buckets S3 e bancos de dados RDS) usando arquivos de modelo declarativos formatados em JSON ou YAML. Qual serviço AWS deve ser utilizado?

- [ ] **A)** AWS OpsWorks
- [x] **B)** AWS CloudFormation
- [ ] **C)** AWS Systems Manager
- [ ] **D)** AWS Config

> **Por que esta é a resposta correta?**
> * O **AWS CloudFormation** permite provisionar e gerenciar recursos da AWS como código (Infraestrutura como Código - IaC). Com ele, define-se a infraestrutura em modelos (*templates*) em JSON ou YAML, garantindo implantações automatizadas, consistentes e reproduzíveis.
>
> 📖 **Documentação:** [O que é o AWS CloudFormation?](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/Welcome.html)

---

### Quiz 2: Gestão de Múltiplas Contas com AWS Organizations
**Enunciado:** Uma grande empresa possui dezenas de contas AWS independentes e deseja centralizar a gestão do faturamento, consolidar descontos por uso e aplicar políticas globais de governança em todas as contas. Qual serviço atende a essa exigência?

- [x] **A)** AWS Organizations
- [ ] **B)** AWS IAM Identity Center
- [ ] **C)** AWS Control Tower
- [ ] **D)** AWS License Manager

> **Por que esta é a resposta correta?**
> * O **AWS Organizations** permite consolidar e gerenciar centralmente múltiplas contas AWS. Ele oferece faturamento consolidado (*Consolidated Billing*), criação automatizada de contas e agrupamento por Unidades Organizacionais (*OUs*).
>
> 📖 **Documentação:** [O que é o AWS Organizations?](https://docs.aws.amazon.com/pt_br/organizations/latest/userguide/orgs_introduction.html)

---

### Quiz 3: Restrição de Permissões com Service Control Policies (SCPs)
**Enunciado:** O administrador de segurança de uma organização precisa proibir que qualquer conta de desenvolvedor crie instâncias EC2 de grande porte (como a família `p4d`) para evitar custos excessivos. Qual recurso do AWS Organizations permite impor esse limite centralizado?

- [ ] **A)** Políticas do IAM (*IAM Policies*)
- [x] **B)** Políticas de Controle de Serviços (*Service Control Policies - SCPs*)
- [ ] **C)** Grupos de Segurança (*Security Groups*)
- [ ] **D)** Regras do AWS Config

> **Por que esta é a resposta correta?**
> * As **Service Control Policies (SCPs)** são políticas de governança centralizadas aplicadas a contas ou Unidades Organizacionais (OUs) dentro do AWS Organizations. Elas estabelecem limites máximos de permissões (*permission boundaries*) que nem mesmo o usuário `root` das contas membro pode ultrapassar.
>
> 📖 **Documentação:** [Políticas de controle de serviços (SCPs)](https://docs.aws.amazon.com/pt_br/organizations/latest/userguide/orgs_manage_policies_scps.html)

---

### Quiz 4: Unidades Organizacionais (OUs)
**Enunciado:** Como a AWS recomenda estruturar hierarquicamente dezenas de contas dentro do AWS Organizations para aplicar diferentes políticas de segurança e conformidade para ambientes de Desenvolvimento, Teste e Produção?

- [ ] **A)** Utilizando tags de recursos atribuídas a instâncias EC2.
- [ ] **B)** Criando múltiplos usuários root na mesma conta.
- [x] **C)** Agrupando as contas em Unidades Organizacionais (*Organizational Units - OUs*).
- [ ] **D)** Dividindo as contas por Regiões AWS diferentes.

> **Por que esta é a resposta correta?**
> * As **Unidades Organizacionais (OUs)** funcionam como pastas dentro de uma organização para agrupar contas com requisitos de segurança e governança semelhantes (ex: `OU-Producao`, `OU-Dev`). SCPs anexadas a uma OU são herdadas automaticamente por todas as contas dentro dela.
>
> 📖 **Documentação:** [Gerenciamento de Unidades Organizacionais (OUs)](https://docs.aws.amazon.com/pt_br/organizations/latest/userguide/orgs_manage_ous.html)

---

### Quiz 5: Drift Detection no AWS CloudFormation
**Enunciado:** Um administrador suspeita que um engenheiro alterou manualmente a configuração de um grupo de segurança que havia sido criado originalmente via AWS CloudFormation. Qual recurso do CloudFormation permite identificar divergências entre a configuração real do recurso e o modelo declarado?

- [ ] **A)** CloudFormation Change Sets
- [x] **B)** Detecção de Desvio (*Drift Detection*)
- [ ] **C)** CloudFormation StackSets
- [ ] **D)** Rollback Automático

> **Por que esta é a resposta correta?**
> * A **Detecção de Desvio (Drift Detection)** do CloudFormation compara as configurações reais dos recursos ativos na conta com as definições do modelo (*template*) original, apontando exatamente quais propriedades foram alteradas fora do CloudFormation.
>
> 📖 **Documentação:** [Detectar desvios de configuração em pilhas do CloudFormation](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/using-cfn-stack-drift.html)

---

### Quiz 6: Implantação Multi-Conta e Multi-Região com StackSets
**Enunciado:** Uma equipe de governança precisa implantar automaticamente o mesmo conjunto de regras de segurança e funções IAM em mais de 50 contas AWS e em 4 regiões diferentes. Qual funcionalidade do CloudFormation é indicada para esse cenário?

- [ ] **A)** CloudFormation Nested Stacks
- [ ] **B)** CloudFormation Modules
- [x] **C)** AWS CloudFormation StackSets
- [ ] **D)** AWS Service Catalog

> **Por que esta é a resposta correta?**
> * Os **StackSets** estendem a capacidade do CloudFormation para criar, atualizar ou excluir pilhas (*stacks*) em múltiplas contas AWS e múltiplas Regiões a partir de uma única operação centralizada.
>
> 📖 **Documentação:** [Conceitos de StackSets do AWS CloudFormation](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/stacksets-concepts.html)

---

### Quiz 7: Pré-visualização de Alterações com Change Sets
**Enunciado:** Antes de aplicar uma atualização importante em um modelo do CloudFormation que gerencia um banco de dados de produção, o arquiteto deseja verificar quais recursos serão modificados, substituídos ou excluídos sem afetar o ambiente atual. Qual recurso deve ser utilizado?

- [x] **A)** Conjuntos de Alterações (*Change Sets*)
- [ ] **B)** CloudFormation Stack Policies
- [ ] **C)** AWS CodeDeploy
- [ ] **D)** AWS CloudTrail Logs

> **Por que esta é a resposta correta?**
> * Os **Change Sets** permitem visualizar como as alterações propostas em um modelo afetarão os recursos ativos antes que a atualização seja executada de fato, evitando exclusões acidentais ou interrupções não planejadas.
>
> 📖 **Documentação:** [Atualização de pilhas usando conjuntos de alterações](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)

---

### Quiz 8: Governança de Arquitetura Multi-Conta com AWS Control Tower
**Enunciado:** Uma grande empresa quer configurar um ambiente multi-conta seguro e em conformidade com as melhores práticas da AWS de forma automatizada, incluindo a criação rápida de novas contas prontas (*Account Factory*) e barreiras de proteção (*Guardrails*). Qual serviço de orquestração deve ser adotado?

- [ ] **A)** AWS Organizations isolado
- [x] **B)** AWS Control Tower
- [ ] **C)** AWS Systems Manager
- [ ] **D)** AWS Service Catalog

> **Por que esta é a resposta correta?**
> * O **AWS Control Tower** automatiza a configuração de um ambiente multi-conta bem arquitetado (*Landing Zone*), integrando AWS Organizations, IAM Identity Center e AWS Config com regras de governança e barreiras de proteção (*Guardrails*) pré-configuradas.
>
> 📖 **Documentação:** [O que é o AWS Control Tower?](https://docs.aws.amazon.com/pt_br/controltower/latest/userguide/what-is-control-tower.html)

---

### Quiz 9: Catálogo de Serviços Aprovados (AWS Service Catalog)
**Enunciado:** Uma organização quer permitir que desenvolvedores provisionem de forma autônoma apenas produtos de infraestrutura pré-aprovados e pré-configurados pela equipe de TI (ex: um banco de dados MySQL padrão), sem dar permissão total de criação no console AWS. Qual serviço fornece essa loja/catálogo interno?

- [ ] **A)** AWS Marketplace
- [x] **B)** AWS Service Catalog
- [ ] **C)** AWS Artifact
- [ ] **D)** AWS Proton

> **Por que esta é a resposta correta?**
> * O **AWS Service Catalog** permite que as empresas criem e gerenciem catálogos de serviços de TI aprovados para uso interno na AWS, garantindo que os usuários implantem apenas recursos que estejam em conformidade com os padrões da empresa.
>
> 📖 **Documentação:** [O que é o AWS Service Catalog?](https://docs.aws.amazon.com/pt_br/servicecatalog/latest/adminguide/introduction.html)

---

### Quiz 10: Desenvolvimento de IaC com AWS CDK (Cloud Development Kit)
**Enunciado:** Uma equipe de desenvolvimento prefere definir infraestrutura em nuvem usando linguagens de programação familiares (como TypeScript, Python, Java ou C#) em vez de escrever arquivos estruturados em YAML/JSON manualmente. Qual ferramenta traduz esse código em modelos do CloudFormation?

- [x] **A)** AWS CDK (Cloud Development Kit)
- [ ] **B)** AWS SAM (Serverless Application Model)
- [ ] **C)** AWS CLI
- [ ] **D)** AWS SDK

> **Por que esta é a resposta correta?**
> * O **AWS CDK** é uma estrutura de desenvolvimento de software de código aberto que permite definir recursos de infraestrutura em nuvem usando linguagens de programação populares. Durante a implantação, o CDK sintetiza o código em modelos nativos do AWS CloudFormation.
>
> 📖 **Documentação:** [O que é o AWS Cloud Development Kit (CDK)?](https://docs.aws.amazon.com/pt_br/cdk/v2/guide/home.html)
