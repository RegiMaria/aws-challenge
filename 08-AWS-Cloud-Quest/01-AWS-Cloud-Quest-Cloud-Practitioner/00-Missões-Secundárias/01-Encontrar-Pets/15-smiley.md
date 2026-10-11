# 🐍 Smiley - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/53cad196-c5d4-43a1-8c02-7e80a1846f86" width="250" alt="Smiley Pet" />
</p>

- **Espécie:** Cobra / Serpente (*Snake*)
- **Domínio AWS:** Segurança, Identidade e Conformidade (*Security, Identity, & Compliance*)
- **Tópico Principal:** Segurança de Rede e Defesa de Perímetro (Security Groups, NACLs, GuardDuty e WAF)
- **Total de Quizzes:** 12
- **Status:** Adotado ✅

---

### Quiz 1: Firewall de Instância — Security Groups
**Enunciado:** Um administrador de sistemas precisa liberar apenas a porta 443 (HTTPS) para acesso externo a uma instância EC2 específica, bloqueando todas as outras portas por padrão. Qual componente de segurança atua diretamente como um firewall virtual no nível da interface de rede (ENI) da instância?

- [ ] **A)** Network ACL (NACL)
- [x] **B)** Security Group
- [ ] **C)** AWS WAF
- [ ] **D)** AWS Shield Advanced

> **Por que esta é a resposta correta?**
> * O **Security Group** age como um firewall virtual baseado em instâncias (ou interfaces de rede) que controla o tráfego de entrada (*inbound*) e saída (*outbound*). Por padrão, todas as conexões de entrada são negadas e as de saída são permitidas.
> * *Incorretas:* As NACLs atuam no nível da sub-rede e não da instância individual.
>
> 📖 **Documentação:** [Controlar o tráfego com grupos de segurança](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/vpc-security-groups.html)

---

### Quiz 2: Comportamento Stateful dos Security Groups
**Enunciado:** Uma instância EC2 possui um Security Group configurado para permitir tráfego de entrada na porta 80 (HTTP). Um usuário envia uma requisição para a aplicação. O que acontece com o tráfego de resposta gerado pela instância ao retornar para o cliente?

- [ ] **A)** O tráfego de resposta é bloqueado automaticamente, exigindo a abertura da mesma porta nas regras de saída (*Outbound*).
- [x] **B)** O tráfego de resposta é permitido automaticamente, pois os Security Groups são **stateful**.
- [ ] **C)** A resposta é redirecionada para um NAT Gateway obrigatoriamente.
- [ ] **D)** O tráfego de resposta precisa passar por uma verificação manual do AWS WAF.

> **Por que esta é a resposta correta?**
> * Os **Security Groups são *stateful***, o que significa que se você permitir uma solicitação de entrada, a resposta correspondente será permitida automaticamente, independentemente das regras de saída configuradas.
>
> 📖 **Documentação:** [Regras de Grupos de Segurança Stateful](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/vpc-security-groups.html#security-group-rules)

---

### Quiz 3: Camada Adicional de Defesa — Network ACLs (NACLs)
**Enunciado:** Uma equipe de segurança da rede precisa bloquear temporariamente todo e qualquer tráfego originado de um endereço IP malicioso específico (`203.0.113.50`) para que ele não consiga atingir nenhuma sub-rede da VPC. Qual recurso suporta regras explícitas de negação (*Deny*) para endereços IP?

- [ ] **A)** Security Groups
- [x] **B)** Network ACLs (Listas de Controle de Acesso à Rede)
- [ ] **C)** AWS Systems Manager
- [ ] **D)** Amazon Route 53 Resolver

> **Por que esta é a resposta correta?**
> * As **Network ACLs** funcionam como um firewall sem estado (*stateless*) no nível da sub-rede. Ao contrário dos Security Groups, as NACLs permitem criar regras de negação explícita (`Deny`), sendo úteis para bloquear IPs indesejados na borda da sub-rede.
>
> 📖 **Documentação:** [Controlar o tráfego para sub-redes com listas de controle de acesso à rede](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/vpc-network-acls.html)

---

### Quiz 4: Natureza Stateless das NACLs
**Enunciado:** Uma Network ACL (NACL) está configurada para permitir o tráfego de entrada na porta 443. Um usuário acessa o site, mas a página não carrega e os logs mostram pacotes bloqueados na saída. Qual é o motivo mais provável para esse comportamento?

- [ ] **A)** O Security Group está corrompido.
- [x] **B)** As NACLs são *stateless*, exigindo uma regra explícita nas regras de saída (*Outbound*) para permitir que a resposta retorne ao cliente.
- [ ] **C)** O tipo de instância EC2 é incompatível com o protocolo HTTPS.
- [ ] **D)** O bucket S3 associado expirou.

> **Por que esta é a resposta correta?**
> * Como as **NACLs são *stateless***, o tráfego de retorno não é liberado automaticamente. Se você permitir o tráfego de entrada, também precisará configurar uma regra de saída correspondente que permita o fluxo de retorno das portas efêmeras.
>
> 📖 **Documentação:** [Diferença entre Grupos de Segurança e NACLs (*Stateless vs. Stateful*)](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/vpc-network-acls.html#nacl-rules)

---

### Quiz 5: Detecção Inteligente de Ameaças (Amazon GuardDuty)
**Enunciado:** Uma empresa deseja monitorar continuamente suas contas AWS, instâncias EC2 e logs do S3 em busca de comportamentos maliciosos ou não autorizados, como criptografia de dados por ransomware, ataques de força bruta SSH ou instâncias comunicando-se com servidores de comando e controle conhecidos. Qual serviço gerenciado de detecção de ameaças deve ser ativado?

- [ ] **A)** Amazon Inspector
- [x] **B)** Amazon GuardDuty
- [ ] **C)** AWS Trusted Advisor
- [ ] **D)** AWS Shield Standard

> **Por que esta é a resposta correta?**
> * O **Amazon GuardDuty** é um serviço inteligente de detecção de ameaças que utiliza aprendizado de máquina (*machine learning*), anomalias e inteligência de ameaças integrada para monitorar fluxos de dados do CloudTrail, VPC Flow Logs e DNS Logs, alertando sobre atividades suspeitas.
>
> 📖 **Documentação:** [O que é o Amazon GuardDuty?](https://docs.aws.amazon.com/pt_br/guardduty/latest/ug/guardduty_whatis.html)

---

### Quiz 6: Avaliação Automatizada de Vulnerabilidades (Amazon Inspector)
**Enunciado:** Um auditor de segurança precisa escanear automaticamente centenas de instâncias Amazon EC2 e imagens de contêineres armazenadas no Amazon ECR em busca de vulnerabilidades de software conhecidas, exposições de rede e falhas no sistema operacional. Qual serviço realiza essa varredura automatizada?

- [ ] **A)** Amazon GuardDuty
- [x] **B)** Amazon Inspector
- [ ] **C)** AWS Config
- [ ] **D)** AWS WAF

> **Por que esta é a resposta correta?**
> * O **Amazon Inspector** é um serviço automatizado de gerenciamento de vulnerabilidades que analisa continuamente as instâncias EC2 e imagens do ECR em busca de exposição de software e vulnerabilidades conhecidas (CVEs).
>
> 📖 **Documentação:** [O que é o Amazon Inspector?](https://docs.aws.amazon.com/pt_br/inspector/latest/user/what-is-inspector.html)

---

### Quiz 7: Proteção de Aplicações Web contra Injeção e Bots (AWS WAF)
**Enunciado:** Uma aplicação web voltada ao público sofre tentativas frequentes de ataques de injeção de SQL (*SQLi*) e ataques de negação de serviço na camada de aplicação (Camada 7). Como filtrar e bloquear essas requisições maliciosas antes que atinjam os servidores web de backend?

- [ ] **A)** Configurar uma Network ACL com negação de IP.
- [x] **B)** Implementar o **AWS WAF (Web Application Firewall)** associado ao ALB ou CloudFront.
- [ ] **C)** Usar o Amazon Macie para mascarar dados.
- [ ] **D)** Ativar o AWS IAM Access Analyzer.

> **Por que esta é a resposta correta?**
> * O **AWS WAF** protege aplicações web contra explorações comuns da web e bots que podem afetar a disponibilidade, comprometer a segurança ou consumir recursos excessivos, permitindo criar regras personalizadas para bloquear SQLi, XSS, etc.
>
> 📖 **Documentação:** [Como funciona o AWS WAF](https://docs.aws.amazon.com/pt_br/waf/latest/developerguide/waf-chapter.html)

---

### Quiz 8: Descoberta e Proteção de Dados Sensíveis (Amazon Macie)
**Enunciado:** Uma empresa precisa cumprir regulamentações de privacidade de dados (como LGPD e GDPR) e quer auditar automaticamente seus buckets Amazon S3 para descobrir se há arquivos contendo informações de identificação pessoal (PII), como números de cartões de crédito ou CPFs armazenados sem criptografia. Qual serviço utilizar?

- [ ] **A)** Amazon GuardDuty
- [x] **B)** Amazon Macie
- [ ] **C)** AWS Artifact
- [ ] **D)** AWS Secrets Manager

> **Por que esta é a resposta correta?**
> * O **Amazon Macie** é um serviço de segurança de dados totalmente gerenciado que utiliza aprendizado de máquina e correspondência de padrões para descobrir e proteger dados confidenciais armazenados no Amazon S3, como PII e credenciais financeiras.
>
> 📖 **Documentação:** [O que é o Amazon Macie?](https://docs.aws.amazon.com/pt_br/macie/latest/user/what-is-macie.html)

---

### Quiz 9: Armazenamento e Rotação de Credenciais e Segredos (AWS Secrets Manager)
**Enunciado:** Uma aplicação precisa se conectar a um banco de dados RDS e o código precisa recuperar a senha de acesso. Para evitar o armazenamento da senha em texto plano no código-fonte, qual serviço deve ser utilizado para armazenar, gerenciar e rotacionar essa senha automaticamente?

- [ ] **A)** AWS Parameter Store sem criptografia
- [x] **B)** AWS Secrets Manager
- [ ] **C)** AWS Artifact
- [ ] **D)** Amazon S3 com Object Lock

> **Por que esta é a resposta correta?**
> * O **AWS Secrets Manager** ajuda a proteger segredos necessários para acessar aplicações, serviços e recursos de TI, permitindo rotacionar, gerenciar e recuperar credenciais de banco de dados, chaves de API e senhas de forma segura com criptografia KMS.
>
> 📖 **Documentação:** [O que é o AWS Secrets Manager?](https://docs.aws.amazon.com/pt_br/secretsmanager/latest/userguide/intro.html)

---

### Quiz 10: Centralização de Alertas de Segurança (AWS Security Hub)
**Enunciado:** Uma equipe de segurança opera dezenas de serviços de proteção na AWS (GuardDuty, Inspector, Macie, IAM Access Analyzer). Eles precisam de um painel centralizado único que colete, priorize e consolide todas as descobertas e alertas de segurança dessas ferramentas em conformidade com padrões da indústria. Qual serviço utilizar?

- [ ] **A)** AWS CloudTrail
- [x] **B)** AWS Security Hub
- [ ] **C)** AWS Trusted Advisor
- [ ] **D)** Amazon CloudWatch Dashboards

> **Por que esta é a resposta correta?**
> * O **AWS Security Hub** fornece uma visão abrangente do seu estado de segurança na AWS e ajuda a verificar seu ambiente em relação a padrões e melhores práticas da indústria (como CIS Benchmarks), consolidando achados de múltiplas ferramentas de segurança em um único painel.
>
> 📖 **Documentação:** [O que é o AWS Security Hub?](https://docs.aws.amazon.com/pt_br/securityhub/latest/userguide/what-is-securityhub.html)

---

### Quiz 11: Análise de Políticas de Acesso Externo (IAM Access Analyzer)
**Enunciado:** Um administrador de segurança deseja verificar rapidamente se existem recursos na conta (como buckets S3, papéis do IAM ou chaves do KMS) que estão compartilhados acidentalmente com entidades externas ou contas públicas na internet. Qual recurso auxilia nessa análise de risco?

- [x] **A)** IAM Access Analyzer
- [ ] **B)** AWS Shield Standard
- [ ] **C)** VPC Flow Logs
- [ ] **D)** AWS WAF IP Sets

> **Por que esta é a resposta correta?**
> * O **IAM Access Analyzer** ajuda a identificar quaisquer recursos em sua organização ou conta que estejam compartilhados com entidades externas, ajudando a identificar potenciais exposições não intencionais de dados e políticas permissivas demais.
>
> 📖 **Documentação:** [O que é o IAM Access Analyzer?](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-what-is.html)

---

### Quiz 12: Mitigação de Ataques de DDoS em Camada de Aplicação (AWS Shield Advanced)
**Enunciado:** Uma grande instituição financeira sofre ataques contínuos e complexos de negação de serviço distribuído (DDoS) na camada de aplicação (Camada 7) e precisa contar com suporte de especialistas de plantão da AWS (Response Team - SRT) para mitigar o ataque em tempo real. Qual plano de proteção deve ser contratado?

- [ ] **A)** AWS Shield Standard
- [x] **B)** AWS Shield Advanced
- [ ] **C)** AWS WAF básico
- [ ] **D)** AWS Basic Support

> **Por que esta é a resposta correta?**
> * O **AWS Shield Advanced** é um serviço pago que oferece proteção ampliada contra ataques de DDoS para aplicações executadas no Amazon EC2, ELB, CloudFront e Route 53, além de incluir acesso 24x7 à equipe de resposta a ataques da AWS (*Shield Response Team - SRT*) e proteção financeira contra custos gerados por picos de tráfego de ataques.
>
> 📖 **Documentação:** [O que é o AWS Shield Advanced?](https://docs.aws.amazon.com/pt_br/waf/latest/developerguide/shield-advanced.html)
