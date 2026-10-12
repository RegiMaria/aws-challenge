# 🧊🐻‍❄️  Sarge - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/3c855b52-2367-425e-a3fc-2525a4e74ed3" width="250" alt="Sarge Pet" />
</p>

- **Espécie:** Urso Polar (*Polar Bear*)
- **Domínio AWS:** Segurança, Identidade e Conformidade / Faturamento, Preços e Gestão (*Security, Identity, & Compliance / Billing & Management*)
- **Tópico Principal:** Auditoria de Configuração, Conformidade Regulatória e Gerenciamento de Mudanças (AWS Config, AWS Artifact e AWS Systems Manager)
- **Total de Quizzes:** 12
- **Status:** Adotado ✅

---

### Quiz 1: Avaliação Contínua de Conformidade e Inventário (AWS Config)
**Enunciado:** Um analista de conformidade precisa monitorar e registrar continuamente as alterações de configuração dos recursos AWS de sua conta, avaliando automaticamente se as configurações atendem às regras organizacionais (como garantir que todos os volumes EBS estejam criptografados). Qual serviço realiza esse rastreamento?

- [ ] **A)** AWS CloudTrail
- [x] **B)** AWS Config
- [ ] **C)** AWS Control Tower
- [ ] **D)** Amazon GuardDuty

> **Por que esta é a resposta correta?**
> * O **AWS Config** avalia, audita e monitora continuamente as configurações dos seus recursos AWS. Ele permite criar regras (*Config Rules*) para verificar a conformidade dos recursos em relação aos padrões de segurança e governança desejados.
> * *Incorretas:* O CloudTrail registra chamadas de API (quem fez o quê); o AWS Config rastreia o estado e o histórico de configuração do recurso ao longo do tempo.
>
> 📖 **Documentação:** [O que é o AWS Config?](https://docs.aws.amazon.com/pt_br/config/latest/developerguide/what-is-aws-config.html)

---

### Quiz 2: Relatórios de Conformidade Regulatória e Auditoria (AWS Artifact)
**Enunciado:** Durante uma auditoria externa de conformidade (ex: PCI-DSS, ISO 27001 ou SOC 2), a empresa precisa apresentar os relatórios de conformidade oficiais da infraestrutura global da AWS e aceitar acordos de auditoria. Onde o cliente pode baixar esses documentos diretamente no console?

- [x] **A)** AWS Artifact
- [ ] **B)** AWS Trusted Advisor
- [ ] **C)** AWS Security Hub
- [ ] **D)** AWS Systems Manager

> **Por que esta é a resposta correta?**
> * O **AWS Artifact** é o portal de recursos de conformidade no qual é possível ter acesso sob demanda a relatórios de segurança e conformidade da AWS (como relatórios SOC, certificações ISO e diretrizes PCI-DSS) e gerenciar acordos online (*Agreements*).
>
> 📖 **Documentação:** [O que é o AWS Artifact?](https://docs.aws.amazon.com/pt_br/artifact/latest/ug/what-is-aws-artifact.html)

---

### Quiz 3: Gerenciamento em Escala de Servidores (AWS Systems Manager)
**Enunciado:** Uma equipe de TI precisa gerenciar um parque de centenas de instâncias EC2 e servidores físicos locais (*on-premises*). A equipe deseja automatizar a aplicação de patches do sistema operacional, executar comandos remotamente e visualizar o inventário de software instalado sem precisar abrir conexões SSH diretas. Qual serviço unifica essa gestão?

- [ ] **A)** AWS OpsWorks
- [x] **B)** AWS Systems Manager (SSM)
- [ ] **C)** AWS CloudFormation
- [ ] **D)** AWS Service Catalog

> **Por que esta é a resposta correta?**
> * O **AWS Systems Manager** é o centro de operações da AWS para gerenciar recursos na AWS e em ambientes híbridos. Ele automatiza tarefas operacionais diárias como gerenciamento de patches (*Patch Manager*), execução remota de scripts (*Run Command*) e acesso a shell sem abrir portas SSH (*Session Manager*).
>
> 📖 **Documentação:** [O que é o AWS Systems Manager?](https://docs.aws.amazon.com/pt_br/systems-manager/latest/userguide/what-is-systems-manager.html)

---

### Quiz 4: Acesso Shell Seguro sem Portas Abertas (Session Manager)
**Enunciado:** Um administrador de sistemas precisa conceder acesso de terminal/shell a engenheiros para manutenção de instâncias EC2 privadas. Por razões de segurança, não é permitido abrir a porta 22 (SSH) nos Security Groups nem usar servidores Bastion. Qual funcionalidade do Systems Manager deve ser utilizada?

- [ ] **A)** Systems Manager Run Command
- [x] **B)** AWS Systems Manager Session Manager
- [ ] **C)** EC2 Serial Console
- [ ] **D)** AWS AppConfig

> **Por que esta é a resposta correta?**
> * O **Session Manager** fornece gerenciamento de instâncias seguro e auditável baseado em navegador e CLI sem a necessidade de abrir portas de entrada (como a 22/SSH), gerenciar chaves SSH ou manter instâncias de salto (*Bastion Hosts*).
>
> 📖 **Documentação:** [AWS Systems Manager Session Manager](https://docs.aws.amazon.com/pt_br/systems-manager/latest/userguide/session-manager.html)

---

### Quiz 5: Recomendações de Otimização e Melhores Práticas (AWS Trusted Advisor)
**Enunciado:** Um arquiteto de soluções quer auditar uma conta AWS para receber recomendações automatizadas alinhadas com as melhores práticas em cinco categorias: Otimização de Custos, Desempenho, Segurança, Tolerância a Falhas e Limites de Serviço. Qual ferramenta fornece esse diagnóstico?

- [ ] **A)** AWS Config
- [x] **B)** AWS Trusted Advisor
- [ ] **C)** Amazon Inspector
- [ ] **D)** AWS Health Dashboard

> **Por que esta é a resposta correta?**
> * O **AWS Trusted Advisor** analisa o ambiente AWS em tempo real e fornece recomendações para ajudar a seguir as melhores práticas em otimização de custos, desempenho, segurança, tolerância a falhas e limites de cota da conta.
>
> 📖 **Documentação:** [O que é o AWS Trusted Advisor?](https://docs.aws.amazon.com/pt_br/awssupport/latest/user/trusted-advisor.html)

---

### Quiz 6: Gerenciamento Centralizado de Parâmetros e Segredos (SSM Parameter Store)
**Enunciado:** Uma aplicação precisa armazenar variáveis de configuração centralizadas (como URIs de bancos de dados, chaves de API e strings de conexão) que possam ser compartilhadas entre microsserviços com controle de acesso via IAM e criptografia via KMS. Qual funcionalidade do Systems Manager oferece esse repositório?

- [x] **A)** AWS Systems Manager Parameter Store
- [ ] **B)** AWS Secrets Manager
- [ ] **C)** AWS Config Store
- [ ] **D)** Amazon S3 Select

> **Por que esta é a resposta correta?**
> * O **Parameter Store** (recurso do Systems Manager) fornece armazenamento seguro e hierárquico para gerenciamento de dados de configuração (como senhas, strings de banco de dados e códigos de licença) em texto sem formatação ou criptografado.
>
> 📖 **Documentação:** [AWS Systems Manager Parameter Store](https://docs.aws.amazon.com/pt_br/systems-manager/latest/userguide/systems-manager-parameter-store.html)

---

### Quiz 7: Remediação Automatizada de Não Conformidades (Config Remediation)
**Enunciado:** Uma regra do AWS Config detectou que um bucket S3 foi criado acidentalmente com acesso público liberado. Em vez de apenas gerar um alerta, o administrador deseja que a permissão pública do bucket seja removida automaticamente no momento da detecção. Como implementar essa ação automática?

- [ ] **A)** Criando uma nova IAM Role com permissões totais no S3.
- [x] **B)** Configurando uma Ação de Remediação (*Remediation Action*) no AWS Config vinculada a um automação do Systems Manager (*SSM Automation Document*).
- [ ] **C)** Reiniciando a instância EC2 que gerencia o S3.
- [ ] **D)** Executando um backup do bucket via AWS Backup.

> **Por que esta é a resposta correta?**
> * O AWS Config permite associar **Ações de Remediação** às suas regras de conformidade. Quando uma não conformidade é identificada, o AWS Config pode disparar automaticamente um documento de automação do Systems Manager para corrigir o recurso (ex: fechar a permissão pública do bucket S3).
>
> 📖 **Documentação:** [Remediação de recursos não em conformidade com o AWS Config](https://docs.aws.amazon.com/pt_br/config/latest/developerguide/remediation.html)

---

### Quiz 8: Monitoramento da Saúde da Infraestrutura AWS (AWS Health Dashboard)
**Enunciado:** Uma empresa percebeu que uma de suas instâncias EC2 parou de responder repentinamente. Onde a equipe pode verificar se há um evento de manutenção programada ou uma degradação de serviço na infraestrutura da AWS afetando especificamente os seus recursos?

- [ ] **A)** AWS Service Health Dashboard (Geral)
- [x] **B)** AWS Health Dashboard (Personal Health Dashboard)
- [ ] **C)** AWS Trusted Advisor
- [ ] **D)** AWS Config timeline

> **Por que esta é a resposta correta?**
> * O **AWS Health Dashboard** fornece visualizações personalizadas sobre o status operacional dos serviços AWS que sustentam os *seus* recursos específicos, alertando sobre interrupções, eventos de manutenção programada e alterações no ambiente.
>
> 📖 **Documentação:** [O que é o AWS Health Dashboard?](https://docs.aws.amazon.com/pt_br/health/latest/ug/what-is-aws-health.html)

---

### Quiz 9: Automação de Instalação de Patches (SSM Patch Manager)
**Enunciado:** O administrador de sistemas precisa definir uma janela de manutenção semanal automatizada para aplicar correções de segurança críticas em todos os servidores Linux da empresa sem intervenção humana manual. Qual recurso do Systems Manager automatiza essa rotina?

- [ ] **A)** Systems Manager State Manager
- [x] **B)** Systems Manager Patch Manager
- [ ] **C)** AWS License Manager
- [ ] **D)** AWS Control Tower

> **Por que esta é a resposta correta?**
> * O **Patch Manager** automatiza o processo de aplicação de patches em instâncias gerenciadas para sistemas operacionais e aplicações, permitindo definir linhas de base de patches (*Patch Baselines*) e agendar janelas de manutenção (*Maintenance Windows*).
>
> 📖 **Documentação:** [AWS Systems Manager Patch Manager](https://docs.aws.amazon.com/pt_br/systems-manager/latest/userguide/systems-manager-patch.html)

---

### Quiz 10: Avaliação de Conformidade de Licenças de Software (AWS License Manager)
**Enunciado:** Uma organização utiliza licenças de software comercial de terceiros (como Microsoft Windows Server ou Oracle Database) que possuem limites rígidos de contagem por número de vCPUs ou soquetes. Qual serviço ajuda a rastrear e evitar que os implantadores violem os termos do contrato de licença na AWS?

- [ ] **A)** AWS Artifact
- [ ] **B)** AWS Organizations
- [x] **C)** AWS License Manager
- [ ] **D)** AWS Service Catalog

> **Por que esta é a resposta correta?**
> * O **AWS License Manager** permite gerenciar centralmente suas licenças de software de fornecedores corporativos (como Microsoft, SAP, Oracle) na AWS e em ambientes locais, estabelecendo regras para bloquear a inicialização de instâncias caso o limite de licenças seja excedido.
>
> 📖 **Documentação:** [O que é o AWS License Manager?](https://docs.aws.amazon.com/pt_br/license-manager/latest/userguide/license-manager-overview.html)

---

### Quiz 11: Rastreamento de Alterações Históricas de Configuração (Config Timeline)
**Enunciado:** Após uma falha de aplicação, um engenheiro precisa saber exatamente qual alteração foi feita nas regras do grupo de segurança de uma instância EC2 na terça-feira passada às 14:00. Qual funcionalidade do AWS Config detalha esse histórico do recurso?

- [ ] **A)** AWS CloudTrail Event History
- [x] **B)** Linha do Tempo de Configuração do Recurso (*Resource Config Timeline*)
- [ ] **C)** VPC Flow Logs
- [ ] **D)** AWS Systems Manager Inventory

> **Por que esta é a resposta correta?**
> * O AWS Config mantém uma **Linha do Tempo de Configuração (*Timeline*)** para cada recurso rastreado, mostrando visualmente a evolução das suas propriedades, relacionamentos com outros recursos e eventos de alteração ao longo do tempo.
>
> 📖 **Documentação:** [Visualizar a linha do tempo de configuração de um recurso](https://docs.aws.amazon.com/pt_br/config/latest/developerguide/view-manage-resource-timeline.html)

---

### Quiz 12: Manutenção do Estado Desejado dos Servidores (SSM State Manager)
**Enunciado:** A equipe de governança precisa garantir que todas as instâncias EC2 da empresa mantenham sempre um agente antivírus específico instalado e rodando em segundo plano. Se o agente for parado ou removido, ele deve ser reinstalado/reiniciado automaticamente. Qual recurso do Systems Manager garante esse estado contínuo?

- [x] **A)** AWS Systems Manager State Manager
- [ ] **B)** AWS Config Rules
- [ ] **C)** AWS OpsWorks Stacks
- [ ] **D)** Amazon EventBridge
> **Por que esta é a resposta correta?**
> * O **State Manager** é um serviço de gerenciamento de configuração que automatiza o processo de manter suas instâncias no estado desejado (*Desired State*), garantindo que softwares, agentes e configurações de sistema operacional permaneçam em conformidade contínua.
>
> 📖 **Documentação:** [AWS Systems Manager State Manager](https://docs.aws.amazon.com/pt_br/systems-manager/latest/userguide/systems-manager-state.html)
