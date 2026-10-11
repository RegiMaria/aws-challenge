# 🐺 Twilight - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/95c29aba-9a34-44d7-96d0-dd3019f105f1" width="250" alt="Twilight Pet" />
</p>

- **Espécie:** Lobo (*Wolf*)
- **Domínio AWS:** Segurança, Identidade e Conformidade (*Security, Identity, & Compliance*)
- **Tópico Principal:** Criptografia de Dados e Modelo de Responsabilidade Compartilhada (AWS KMS, CloudHSM e Proteção de Dados)
- **Total de Quizzes:** 12
- **Status:** Adotado ✅

---

### Quiz 1: Gerenciamento Centralizado de Chaves de Criptografia (AWS KMS)
**Enunciado:** Uma empresa precisa criptografar dados em repouso em múltiplos serviços AWS (como volumes EBS, buckets S3 e bancos RDS) utilizando chaves de criptografia gerenciadas de forma centralizada e com auditoria completa de uso via CloudTrail. Qual serviço deve ser utilizado?

- [ ] **A)** AWS Secrets Manager
- [x] **B)** AWS Key Management Service (AWS KMS)
- [ ] **C)** AWS Certificate Manager (ACM)
- [ ] **D)** AWS Artifact

> **Por que esta é a resposta correta?**
> * O **AWS KMS** permite criar e gerenciar chaves de criptografia (*KMS keys*) e controlar seu uso em uma ampla variedade de serviços da AWS e em suas aplicações, utilizando módulos de segurança de hardware (HSMs) validados FIPS.
> * *Incorretas:* O ACM gerencia certificados SSL/TLS; o Secrets Manager armazena segredos e credenciais de banco de dados.
>
> 📖 **Documentação:** [O que é o AWS Key Management Service?](https://docs.aws.amazon.com/pt_br/kms/latest/developerguide/overview.html)

---

### Quiz 2: Módulo de Segurança de Hardware Dedicado (AWS CloudHSM)
**Enunciado:** Uma instituição financeira com normas estritas de conformidade regulatória exige posse exclusiva e controle direto sobre o hardware do Módulo de Segurança de Hardware (HSM), necessitando de instâncias HSM dedicadas de tenant único dentro de sua VPC. Qual serviço atende a essa exigência?

- [ ] **A)** AWS KMS
- [x] **B)** AWS CloudHSM
- [ ] **C)** AWS Secrets Manager
- [ ] **D)** AWS Security Hub

> **Por que esta é a resposta correta?**
> * O **AWS CloudHSM** fornece módulos de segurança de hardware dedicados na nuvem AWS, onde o cliente mantém controle exclusivo sobre as chaves e o gerenciamento do HSM, cumprindo padrões de conformidade corporativos rígidos.
>
> 📖 **Documentação:** [O que é o AWS CloudHSM?](https://docs.aws.amazon.com/pt_br/cloudhsm/latest/userguide/introduction.html)

---

### Quiz 3: Modelo de Responsabilidade Compartilhada — Responsabilidade DA AWS
**Enunciado:** De acordo com o Modelo de Responsabilidade Compartilhada da AWS, qual das seguintes tarefas é de responsabilidade estrita da **AWS** ("Segurança DA Nuvem")?

- [ ] **A)** Configuração das regras do firewall de entrada no Security Group do EC2.
- [ ] **B)** Criptografia de dados no nível de aplicação do cliente.
- [x] **C)** Segurança física e manutenção do hardware nos centros de dados (*Data Centers*).
- [ ] **D)** Gestão e criação de usuários e senhas dentro do IAM.

> **Por que esta é a resposta correta?**
> * A **AWS é responsável pela segurança DA nuvem**: proteção física das instalações, manutenção de servidores, armazenamento, rede física e o software de virtualização (hipervisor) que roda os serviços.
>
> 📖 **Documentação:** [Modelo de Responsabilidade Compartilhada da AWS](https://aws.amazon.com/pt/compliance/shared-responsibility-model/)

---

### Quiz 4: Modelo de Responsabilidade Compartilhada — Responsabilidade DO Cliente
**Enunciado:** De acordo com o Modelo de Responsabilidade Compartilhada da AWS, qual atividade é de responsabilidade exclusiva do **Cliente** ("Segurança NA Nuvem") ao operar uma instância Amazon EC2?

- [ ] **A)** Troca de discos rígidos com defeito nos servidores físicos do data center.
- [x] **B)** Aplicação de patches de segurança e atualizações do Sistema Operacional instalado na instância.
- [ ] **C)** Correção de vulnerabilidades no hipervisor Xen/KVM do EC2.
- [ ] **D)** Descarte seguro de hardware antigo da infraestrutura AWS.

> **Por que esta é a resposta correta?**
> * O **Cliente é responsável pela segurança NA nuvem**: gerenciamento dos dados do cliente, configuração do sistema operacional e patches na instância EC2, regras de firewall (Security Groups) e gestão de acessos e identidades (IAM).
>
> 📖 **Documentação:** [Responsabilidades do cliente no modelo compartilhado](https://docs.aws.amazon.com/pt_br/whitepapers/latest/aws-risk-and-compliance/shared-responsibility-model.html)

---

### Quiz 5: Criptografia em Trânsito (*In-Transit Encryption*)
**Enunciado:** Para proteger dados confidenciais enquanto eles trafegam entre o navegador do usuário e os servidores web da empresa na AWS contra interceptações (*Man-in-the-Middle*), qual mecanismo de segurança deve ser implementado?

- [ ] **A)** Criptografia de disco do EBS com chave KMS por padrão.
- [x] **B)** Protocolo HTTPS utilizando certificados SSL/TLS gerenciados via AWS Certificate Manager (ACM).
- [ ] **C)** Habilitação de réplicas de leitura Multi-AZ no RDS.
- [ ] **D)** Ativação do S3 Versioning no bucket.

> **Por que esta é a resposta correta?**
> * A **criptografia em trânsito** protege dados que transitam pela rede pública ou privada através de protocolos seguros como HTTPS/TLS. O AWS Certificate Manager (ACM) facilita o provisionamento e renovação de certificados SSL/TLS para uso no ALB ou CloudFront.
>
> 📖 **Documentação:** [Proteger dados em trânsito na AWS](https://docs.aws.amazon.com/pt_br/whitepapers/latest/data-protection-aws-cloud/protecting-data-in-transit.html)

---

### Quiz 6: Criptografia em Repouso (*At-Rest Encryption*)
**Enunciado:** Uma empresa exige que todos os dados gravados em discos de armazenamento e bancos de dados sejam criptografados antes de serem gravados no meio físico. Como é chamada essa proteção contra leitura não autorizada do suporte físico?

- [x] **A)** Criptografia em repouso (*Encryption at Rest*)
- [ ] **B)** Criptografia em trânsito (*Encryption in Transit*)
- [ ] **C)** Firewall de aplicação web (*WAF*)
- [ ] **D)** Inspeção profunda de pacotes (*DPI*)

> **Por que esta é a resposta correta?**
> * A **criptografia em repouso** protege dados armazenados em dispositivos físicos (como volumes EBS, buckets S3 ou bancos RDS), garantindo que, mesmo se o suporte físico for removido, os dados permaneçam ilegíveis sem as chaves de descriptografia.
>
> 📖 **Documentação:** [Proteger dados em repouso na AWS](https://docs.aws.amazon.com/pt_br/whitepapers/latest/data-protection-aws-cloud/protecting-data-at-rest.html)

---

### Quiz 7: Chaves Gerenciadas pela AWS vs. Chaves Gerenciadas pelo Cliente (KMS)
**Enunciado:** Qual é a principal vantagem de utilizar uma Chave Gerenciada pelo Cliente (*Customer Managed Key - CMK*) no AWS KMS em vez de uma Chave Gerenciada pela AWS (*AWS Managed Key*)?

- [ ] **A)** As chaves gerenciadas pelo cliente são totalmente gratuitas em qualquer quantidade.
- [x] **B)** O cliente tem controle total sobre as políticas de chave, permitindo definir quem pode usar ou administrar a chave e configurar rotação personalizada.
- [ ] **C)** O cliente precisa manter os servidores físicos do KMS em seu data center local.
- [ ] **D)** As chaves gerenciadas pelo cliente não ficam registradas no AWS CloudTrail.

> **Por que esta é a resposta correta?**
> * As **Customer Managed Keys** oferecem flexibilidade e controle total: você pode alterar políticas de acesso, habilitar/desabilitar chaves, criar aliases, configurar rotação e definir concessões (*grants*) de uso por serviços ou contas específicas.
>
> 📖 **Documentação:** [Chaves gerenciadas pela AWS e chaves gerenciadas pelo cliente](https://docs.aws.amazon.com/pt_br/kms/latest/developerguide/concepts.html#key-mgmt)

---

### Quiz 8: Gestão de Certificados Digitais SSL/TLS (AWS Certificate Manager - ACM)
**Enunciado:** Uma equipe web precisa emitir e renovar certificados digitais SSL/TLS públicos para seus domínios associados a um Application Load Balancer sem precisar gerenciar processos manuais de renovação de expiração. Qual serviço automatiza esse ciclo de vida?

- [ ] **A)** AWS KMS
- [x] **B)** AWS Certificate Manager (ACM)
- [ ] **C)** AWS Directory Service
- [ ] **D)** Amazon Route 53 Domain Registrar

> **Por que esta é a resposta correta?**
> * O **AWS Certificate Manager (ACM)** permite provisionar, gerenciar e implantar facilmente certificados SSL/TLS públicos e privados para uso com serviços integrados da AWS (como ALB, CloudFront e API Gateway), cuidando da renovação automática.
>
> 📖 **Documentação:** [O que é o AWS Certificate Manager?](https://docs.aws.amazon.com/pt_br/acm/latest/userguide/acm-overview.html)

---

### Quiz 9: Responsabilidade Compartilhada em Serviços Serverless / PaaS
**Enunciado:** Ao utilizar o serviço **AWS Lambda** (PaaS/Serverless) em comparação com o **Amazon EC2** (IaaS), como a divisão do Modelo de Responsabilidade Compartilhada se altera em favor do cliente?

- [ ] **A)** O cliente passa a gerenciar a segurança física do data center da AWS.
- [x] **B)** A AWS assume a responsabilidade de gerenciar o sistema operacional, patches de runtime e infraestrutura, cabendo ao cliente apenas o código e a gestão de acessos.
- [ ] **C)** A AWS assume total responsabilidade pelo código e bugs escritos pelo cliente.
- [ ] **D)** O cliente não precisa mais configurar permissões do IAM no Lambda.

> **Por que esta é a resposta correta?**
> * Em serviços abstratos/Serverless como o **AWS Lambda**, a AWS gerencia a camada de infraestrutura, o sistema operacional e a plataforma de execução (*runtime*). O cliente foca apenas no seu próprio código, dados e configurações de autorização.
>
> 📖 **Documentação:** [Modelo de responsabilidade para serviços gerenciados da AWS](https://aws.amazon.com/pt/compliance/shared-responsibility-model/)

---

### Quiz 10: Envelopamento Criptográfico (*Envelope Encryption*)
**Enunciado:** Para otimizar a performance de criptografia de arquivos grandes, o AWS KMS utiliza uma técnica onde uma Chave de Dados (*Data Key*) criptografa o arquivo real e a Chave de Dados é criptografada por uma Chave KMS Principal (*KMS Key*). Como se chama esse conceito?

- [x] **A)** Criptografia de Envelopamento (*Envelope Encryption*)
- [ ] **B)** Criptografia de Bloco Simétrico
- [ ] **C)** Hashing Assimétrico de Curva Elíptica
- [ ] **D)** Dupla Criptografia por Tunneling

> **Por que esta é a resposta correta?**
> * A **Criptografia de Envelopamento** utiliza uma chave de dados para criptografar os dados da aplicação e, em seguida, criptografa a própria chave de dados com uma chave mestra (KMS Key). Isso evita enviar grandes volumes de dados pela rede até o KMS para serem criptografados.
>
> 📖 **Documentação:** [Criptografia de envelopamento no AWS KMS](https://docs.aws.amazon.com/pt_br/kms/latest/developerguide/concepts.html#enveloping)

---

### Quiz 11: Criptografia Padrão de Buckets no Amazon S3 (SSE-S3 vs. SSE-KMS)
**Enunciado:** Uma empresa deseja que todos os arquivos enviados para o Amazon S3 sejam criptografados automaticamente no lado do servidor (*Server-Side Encryption*) utilizando chaves gerenciadas de forma transparente pelo próprio Amazon S3 sem custo adicional por chave. Qual opção atende a essa solicitação?

- [ ] **A)** SSE-C (Server-Side Encryption with Customer-Provided Keys)
- [x] **B)** SSE-S3 (Server-Side Encryption with Amazon S3 Managed Keys)
- [ ] **C)** Criptografia no lado do cliente (*Client-Side Encryption*)
- [ ] **D)** Criptografia via AWS CloudHSM

> **Por que esta é a resposta correta?**
> * Com o **SSE-S3**, cada objeto é criptografado com uma chave exclusiva utilizando o padrão AES-256. A AWS gerencia tanto a criptografia quanto as chaves de forma transparente e automática sem taxas adicionais.
>
> 📖 **Documentação:** [Proteger dados usando criptografia no lado do servidor com chaves gerenciadas pelo Amazon S3 (SSE-S3)](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/userguide/UsingServerSideEncryption.html)

---

### Quiz 12: Rotação Automática de Chaves no AWS KMS
**Enunciado:** Para atender às políticas internas de segurança, uma empresa precisa garantir que as chaves de criptografia gerenciadas pelo cliente no AWS KMS tenham suas matérias de chave renovadas/rotacionadas a cada 1 ano de forma automatizada. Como configurar esse requisito?

- [ ] **A)** Agendando uma função Lambda manual que exclui a chave antiga e cria uma nova.
- [x] **B)** Ativando a opção de **Rotação Automática de Chaves** (*Automatic Key Rotation*) no painel do KMS para a chave desejada.
- [ ] **C)** Recriando a conta AWS anualmente.
- [ ] **D)** Abrindo um chamado no AWS Support solicitando a troca do hardware física do HSM.

> **Por que esta é a resposta correta?**
> * O AWS KMS permite ativar a **rotação automática de chaves** para Chaves Gerenciadas pelo Cliente. Quando ativada, o KMS gera uma nova matéria de chave a cada ano automaticamente, mantendo a matéria antiga disponível de forma transparente para descriptografar dados passados.
>
> 📖 **Documentação:** [Rotação de chaves do AWS KMS](https://docs.aws.amazon.com/pt_br/kms/latest/developerguide/rotate-keys.html)
