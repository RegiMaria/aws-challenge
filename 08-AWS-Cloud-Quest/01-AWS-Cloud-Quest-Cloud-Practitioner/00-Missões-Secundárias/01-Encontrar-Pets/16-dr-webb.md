# 🕷️ Dr. Webb - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/4d99c0af-9c91-4c7b-9b2f-7b0c03bed3b1" width="250" alt="Dr Webb Pet" />
</p>

- **Espécie:** Aranha (*Spider*)
- **Domínio AWS:** Segurança, Identidade e Conformidade (*Security, Identity, & Compliance*)
- **Tópico Principal:** Gestão de Identidades e Acessos (AWS Identity and Access Management - IAM)
- **Total de Quizzes:** 12
- **Status:** Adotado ✅

---

### Quiz 1: Controle de Acesso e Identidades (AWS IAM)
**Enunciado:** Um administrador da AWS precisa criar novos usuários para a equipe de desenvolvimento, organizar esses usuários por departamentos e definir permissões específicas sobre quais recursos cada desenvolvedor pode acessar. Qual serviço atende a essa finalidade?

- [ ] **A)** AWS Organizations
- [x] **B)** AWS Identity and Access Management (IAM)
- [ ] **C)** AWS Control Tower
- [ ] **D)** AWS Directory Service

> **Por que esta é a resposta correta?**
> * O **AWS IAM** é o serviço centralizado que permite gerenciar o acesso a serviços e recursos da AWS com segurança. Com o IAM, é possível criar e gerenciar usuários, grupos e funções (*roles*), além de utilizar permissões para conceder ou negar acesso aos recursos.
> * *Incorretas:* O AWS Organizations gerencia múltiplas contas, não usuários individuais dentro de uma conta.
>
> 📖 **Documentação:** [O que é o AWS IAM?](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/introduction.html)

---

### Quiz 2: O Princípio do Menor Privilégio (*Least Privilege*)
**Enunciado:** Qual é a boa prática fundamental de segurança recomendada pela AWS ao atribuir permissões do IAM para usuários, funções ou aplicações?

- [ ] **A)** Conceder acesso total (`AdministratorAccess`) a todos os usuários para evitar chamadas de suporte.
- [x] **B)** Conceder apenas as permissões estritamente necessárias para executar as tarefas necessárias (*Princípio do Menor Privilégio*).
- [ ] **C)** Atribuir todas as permissões no nível da conta principal (*Root*).
- [ ] **D)** Desativar a autenticação de dois fatores (MFA) para acelerar o desenvolvimento.

> **Por que esta é a resposta correta?**
> * O **Princípio do Menor Privilégio** dita que os usuários e aplicações devem receber apenas as permissões necessárias para realizar suas funções específicas, reduzindo o impacto de acessos indevidos ou credenciais comprometidas.
>
> 📖 **Documentação:** [Melhores práticas de segurança no IAM — Menor Privilégio](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/best-practices.html#grant-least-privilege)

---

### Quiz 3: Permissões para Aplicações em Instâncias EC2 (IAM Roles)
**Enunciado:** Uma aplicação rodando em uma instância Amazon EC2 precisa ler e gravar arquivos em um bucket Amazon S3. Qual é a forma mais segura de conceder esse acesso sem armazenar credenciais fixas (*Access Keys*) no código-fonte ou na instância?

- [ ] **A)** Gravar a chave de acesso e a chave secreta do usuário Root no arquivo de configuração da aplicação.
- [x] **B)** Criar uma Função IAM (*IAM Role*) com as permissões do S3 e associá-la à instância EC2 (Perfil de Instância).
- [ ] **C)** Desativar a segurança do bucket S3 tornando-o totalmente público.
- [ ] **D)** Compartilhar a senha do console AWS com o desenvolvedor responsável.

> **Por que esta é a resposta correta?**
> * As **IAM Roles** fornecem credenciais temporárias de segurança que são renovadas automaticamente pela AWS, eliminando a necessidade de embutir chaves de acesso estáticas e vulneráveis no código das aplicações.
>
> 📖 **Documentação:** [Uso de funções do IAM para conceder permissões a aplicativos executados em instâncias EC2](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html)

---

### Quiz 4: Proteção do Usuário Root da Conta AWS
**Enunciado:** Ao criar uma nova conta AWS, o usuário *Root* possui acesso irrestrito a todos os recursos e dados de faturamento. Qual é a recomendação imediata de segurança para proteger essa identidade?

- [ ] **A)** Compartilhar as credenciais do Usuário Root com os líderes do time de tecnologia.
- [x] **B)** Ativar a Autenticação de Múltiplos Fatores (MFA), eliminar chaves de acesso do Root e criar usuários IAM específicos para o dia a dia.
- [ ] **C)** Utilizar o Usuário Root para o trabalho rotineiro de desenvolvimento.
- [ ] **D)** Excluir a política de senha da conta.

> **Por que esta é a resposta correta?**
> * O **Usuário Root** deve ser protegido com MFA físico ou virtual e utilizado estritamente para tarefas administrativas que exigem acesso Root. Para todas as outras atividades diárias, devem ser criados usuários ou papéis do IAM.
>
> 📖 **Documentação:** [Melhores práticas para proteger o usuário root da conta AWS](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)

---

### Quiz 5: Autenticação de Múltiplos Fatores (MFA)
**Enunciado:** Qual mecanismo adiciona uma camada extra de proteção sobre as credenciais padrão de nome de usuário e senha, exigindo um código numérico temporário de um dispositivo físico ou aplicativo autenticador durante o login?

- [ ] **A)** Criptografia de disco do EBS
- [x] **B)** Autenticação de Múltiplos Fatores (*Multi-Factor Authentication - MFA*)
- [ ] **C)** Função do IAM (*IAM Role*)
- [ ] **D)** AWS WAF Rules

> **Por que esta é a resposta correta?**
> * O **MFA** fornece segurança aprimorada exigindo que os usuários forneçam um código gerado por um dispositivo físico token ou aplicativo (como Google Authenticator) além de sua senha de acesso.
>
> 📖 **Documentação:** [Uso de MFA na AWS](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/id_credentials_mfa.html)

---

### Quiz 6: Estrutura de Documentos de Política do IAM (JSON Policies)
**Enunciado:** As políticas do IAM são documentos em formato JSON que definem permissões. Qual elemento dentro do bloco da política especifica explicitamente se a ação declarada será permitida ou negada?

- [ ] **A)** `Principal`
- [x] **B)** `Effect` (Allow / Deny)
- [ ] **C)** `Resource`
- [ ] **D)** `Action`

> **Por que esta é a resposta correta?**
> * O elemento **`Effect`** em um documento de política do IAM determina o resultado final da instrução, assumindo obrigatoriamente os valores `Allow` (Permitir) ou `Deny` (Negar).
>
> 📖 **Documentação:** [Elementos de política JSON do IAM — Effect](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/reference_policies_elements_effect.html)

---

### Quiz 7: Avaliação de Políticas — Deny Explícito
**Enunciado:** Um usuário possui duas políticas do IAM anexadas: a Política A permite ler objetos em um bucket S3 (`Allow s3:GetObject`), e a Política B nega explicitamente o acesso a esse mesmo bucket (`Deny s3:GetObject`). Qual será a decisão final de acesso quando o usuário tentar ler um arquivo?

- [ ] **A)** O acesso será permitido, pois a permissão da Política A prevalece.
- [x] **B)** O acesso será **negado**, pois um `Deny` explícito sempre se sobrepõe a qualquer `Allow`.
- [ ] **C)** A AWS solicitará uma aprovação manual do administrador.
- [ ] **D)** O arquivo será copiado automaticamente para outra região.

> **Por que esta é a resposta correta?**
> * Lógica de avaliação do IAM: por padrão, todas as solicitações são negadas implicitamente. Um `Allow` explícito concede a permissão, mas um **`Deny` explícito sempre substitui qualquer permissão `Allow`**, independente de onde esteja declarado.
>
> 📖 **Documentação:** [Lógica de avaliação de políticas do IAM](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)

---

### Quiz 8: Single Sign-On e Federação de Identidades (AWS IAM Identity Center)
**Enunciado:** Uma empresa possui centenas de funcionários cadastrados em um provedor de identidade corporativo externo (como Microsoft Entra ID / Active Directory ou Google Workspace) e deseja permitir que eles façam login no AWS Management Console usando suas credenciais corporativas existentes sem duplicar usuários. Qual serviço usar?

- [ ] **A)** AWS IAM Users tradicionais
- [x] **B)** AWS IAM Identity Center (sucessor do AWS Single Sign-On)
- [ ] **C)** Amazon GuardDuty
- [ ] **D)** AWS Secrets Manager

> **Por que esta é a resposta correta?**
> * O **AWS IAM Identity Center** conecta seu provedor de identidade corporativo existente à AWS por meio de federação (SAML 2.0 / OpenID Connect), permitindo que os usuários façam login centralizado e acessem múltiplas contas e aplicações AWS.
>
> 📖 **Documentação:** [O que é o AWS IAM Identity Center?](https://docs.aws.amazon.com/pt_br/singlesignon/latest/userguide/what-is.html)

---

### Quiz 9: Políticas Baseadas em Recursos vs. Baseadas em Identidade
**Enunciado:** Uma empresa deseja autorizar o acesso de uma conta AWS externa a um bucket Amazon S3 sem precisar criar usuários do IAM locais. Onde a política de permissões deve ser anexada diretamente?

- [ ] **A)** No usuário local da conta chamadora.
- [x] **B)** No próprio recurso (Bucket Policy do Amazon S3) como uma Política Baseada em Recurso (*Resource-based Policy*).
- [ ] **C)** Na tabela de roteamento da VPC.
- [ ] **D)** No arquivo do AWS CloudTrail.

> **Por que esta é a resposta correta?**
> * As **Políticas Baseadas em Recursos** são anexadas diretamente ao recurso (como Buckets S3, Chaves KMS ou Filas SQS) e especificam quem (Qual `Principal`) tem permissão para realizar quais ações naquele recurso específico.
>
> 📖 **Documentação:** [Políticas baseadas em identidade e baseadas em recursos](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access_policies_identity-vs-resource.html)

---

### Quiz 10: Rotação de Chaves de Acesso (*Access Keys*)
**Enunciado:** Um desenvolvedor utiliza credenciais de linha de comando (*AWS CLI*) compostas por uma `Access Key ID` e uma `Secret Access Key`. Qual é a melhor prática de segurança para gerenciar essas chaves ao longo do tempo?

- [ ] **A)** Manter as mesmas chaves de acesso indefinidamente para não quebrar scripts.
- [x] **B)** Rotacionar as chaves de acesso periodicamente e revogar as chaves antigas não utilizadas.
- [ ] **C)** Enviar as chaves por e-mail para todos os membros da equipe de suporte.
- [ ] **D)** Publicar as chaves no repositório público do GitHub.

> **Por que esta é a resposta correta?**
> * A **rotação periódica de chaves de acesso** reduz o risco de que chaves antigas ou vazadas acidentalmente sejam exploradas por partes não autorizadas por tempo indeterminado.
>
> 📖 **Documentação:** [Gerenciamento e rotação de chaves de acesso do IAM](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/id_credentials_access-keys.html#rotating_access_keys)

---

### Quiz 11: Acesso Temporário com AWS STS (Security Token Service)
**Enunciado:** Qual serviço web da AWS é responsável por emitir credenciais de segurança temporárias de curto prazo (que expiram automaticamente entre minutos e poucas horas) para usuários federados ou IAM Roles?

- [x] **A)** AWS Security Token Service (AWS STS)
- [ ] **B)** AWS KMS (Key Management Service)
- [ ] **C)** AWS Secrets Manager
- [ ] **D)** Amazon Cognito

> **Por que esta é a resposta correta?**
> * O **AWS STS** é o serviço que gera credenciais temporárias restritas e configuráveis com tempo de expiração para autenticar chamadas de API, garantindo maior segurança do que credenciais estáticas de longo prazo.
>
> 📖 **Documentação:** [O que é o AWS Security Token Service (STS)?](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/id_credentials_temp.html)

---

### Quiz 12: Gerenciamento de Usuários de Aplicações Web e Mobile (Amazon Cognito)
**Enunciado:** Um desenvolvedor está criando um aplicativo móvel e precisa gerenciar a inscrição, login, autenticação e perfil de milhões de usuários finais (clientes do app), permitindo inclusive login social via Google e Apple. Qual serviço é especializado nesse gerenciamento de identidades de clientes?

- [ ] **A)** AWS IAM
- [x] **B)** Amazon Cognito
- [ ] **C)** AWS Organizations
- [ ] **D)** AWS IAM Identity Center

> **Por que esta é a resposta correta?**
> * O **Amazon Cognito** lida com a autenticação, autorização e gerenciamento de usuários para seus aplicativos Web e móveis. Ele escala para milhões de usuários e suporta login social e provedores de identidade SAML/OIDC.
>
> 📖 **Documentação:** [O que é o Amazon Cognito?](https://docs.aws.amazon.com/pt_br/cognito/latest/developerguide/what-is-amazon-cognito.html)
