<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" width="220" alt="AWS Logo"/>

# 🔐 IAM Deep Dive

</div>

Este repositório documenta uma trilha própria (fora das trilhas oficiais de Knowledge Badge) montada pra estudar **AWS Identity and Access Management (IAM) a fundo** - do básico até tópicos avançados como troubleshooting, Access Analyzer, arquitetura interna e isolamento de tenants com ABAC.

A trilha nasceu porque o curso de IAM dentro do **AWS Security Champion** cobre o serviço de forma bem superficial (só 10 minutos), e IAM é a base de segurança de praticamente tudo na AWS - por isso merece uma trilha dedicada.

> 📅 **Cada tópico da trilha = 1 dia de estudo.**

A cada dia eu completo um treinamento, tiro um print/certificado de conclusão e salvo na pasta `05-AWS-IAM-Deep-Dive/` com o padrão de nome `day-XX-nome-do-topico.png`. Todo o progresso fica registrado na tabela lá embaixo.

---

## 📚 Resumo dos tópicos

**1. AWS Identity and Access Management - Identity Center Getting Started** - Introdução ao IAM Identity Center (antigo AWS SSO): como centralizar o acesso de usuários a múltiplas contas AWS e aplicações, com login único (SSO).

**2. Introduction to AWS Identity and Access Management (IAM)** - Fundamentos do IAM: autenticação e autorização, políticas, roles, e um demo prático de criação de grupo/usuário IAM.

**3. AWS Identity and Access Management - Architecture and Terminology** - Como o IAM funciona por dentro: sua arquitetura e a terminologia essencial (principals, actions, resources, conditions, policies) pra entender qualquer outro curso de IAM com mais profundidade.

**4. AWS Identity and Access Management - Access Analyzer Getting Started** - Como usar o IAM Access Analyzer pra identificar recursos compartilhados com entidades externas e revisar permissões excessivas antes que virem um risco de segurança.

**5. Authentication and Authorization with AWS Identity and Access Management** - Diferença entre autenticação (quem você é) e autorização (o que você pode fazer) no contexto do IAM, e como o serviço aplica esses dois conceitos na prática.

**6. AWS Identity and Access Management (IAM) - Troubleshooting** - Como diagnosticar e resolver problemas comuns de permissão/acesso no IAM, e como reunir informações úteis pra abrir um case de suporte na AWS.

**7. Deep Dive with Security: AWS Identity and Access Management (IAM)** - O curso mais denso da trilha (8h): mergulho avançado em políticas complexas, boundaries, roles cross-account, boas práticas de segurança e cenários reais de uso do IAM em escala.

- Lógica de avaliação de políticas → módulos "IAM Policy Basics", "Policy Evaluation Overview", "IAM Condition Keys", "Advanced Policy Elements"
- Federação de identidade → módulo inteiro "IDENTITY FEDERATION DEEP DIVE" (Federating Users, SAML-Based Federation, Web-Based Federation, IAM Identity Center)

**8. Implementing SaaS Tenant Isolation with ABAC and AWS IAM** - Aplicação avançada de IAM: como usar ABAC (controle de acesso por atributos/tags) em vez de RBAC pra isolar dados de diferentes tenants em uma aplicação SaaS multi-tenant.

---

## 📊 Progresso

| Dia | Data | Tópico | Nível | Duração | Certificado (repo) | Link AWS |
|----|------|--------|-------|---------|---------------------|----------|
| 01 | 16/08 | AWS Identity and Access Management - Identity Center Getting Started | Fundamental | 1h | [Ver-certificado](https://github.com/RegiMaria/aws-challenge/blob/main/05-AWS-IAM-Deep-Dive/day-01-IAM-Getting-Started.png) | [Abrir curso](https://skillbuilder.aws/learn/FNSWD5KQ6D/aws-identity-and-access-management--identity-center-getting-started/UJPMQR327J) |
| 02 | 16/08 | 💡 LAB-Introduction to AWS Identity and Access Management (IAM) | Fundamental | 45min | [Ver-certificado](https://github.com/RegiMaria/aws-challenge/blob/main/05-AWS-IAM-Deep-Dive/day-02-LAB-IAM.png) | [Abrir curso](https://skillbuilder.aws/learn/XFPX3M7HAQ/introduction-to-aws-identity-and-access-management-iam/DQJ3N5QRRU) |
| 03 | 17/08 | AWS Identity and Access Management - Architecture and Terminology | Fundamental | 15min | [Ver-certificado](https://github.com/RegiMaria/aws-challenge/blob/main/05-AWS-IAM-Deep-Dive/day-03-IAM-Architecture-Terminology.png) | [Abrir curso](https://skillbuilder.aws/learn/JUU8UTHZAC/aws-identity-and-access-management--architecture-and-terminology/GZNZAF17KZ) |
| 04 | - | AWS Identity and Access Management - Access Analyzer Getting Started | Fundamental | 1h | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/TRVKDBK35D/aws-identity-and-access-management--access-analyzer-getting-started/T4XH5UHBQ4) |
| 05 | - | Authentication and Authorization with AWS Identity and Access Management | Fundamental | 15min | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/B14DZR1P26/authentication-and-authorization-with-aws-identity-and-access-management/AAFD2QRC3C) |
| 06 | - | AWS Identity and Access Management (IAM) - Troubleshooting | Intermediário | 1h | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/ESGHTNXQFZ/aws-identity-and-access-management-iam--troubleshooting/3MQXNQUUQ4) |
| 07 | - | Deep Dive with Security: AWS Identity and Access Management (IAM) | Avançado | 8h | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/525X5QYY33/deep-dive-with-security-aws-identity-and-access-management-iam/EJR97RFJZ3) |
| 08 | - | Implementing SaaS Tenant Isolation with ABAC and AWS IAM | Avançado | 45min | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/B2E9EMZZU2/implementing-saas-tenant-isolation-with-abac-and-aws-iam/5PENCY3QD7) |

> Ainda não tenho o link do repositório para os certificados - vou preenchendo essa coluna conforme completo cada dia.

---

## 📖 Leituras complementares

Materiais de referência (sem certificado, mas ótimos complementos sobre federação de identidade):

- [AWS IAM – Identity Providers and Federation (User Guide)](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/id_roles_providers.html)
- [Configurando federação na AWS a partir do Azure DevOps usando OpenID Connect](https://aws.amazon.com/pt/blogs/aws-brasil/configurando-federacao-na-aws-a-partir-do-azure-devops-usando-openid-connect/)

---

## 🏆 Meta

Dominar IAM na prática - da arquitetura básica até troubleshooting e padrões avançados como ABAC - como base sólida para todas as outras trilhas de segurança do repositório.

---

<div align="center">

Se gostou, não esqueça de deixar uma ⭐

<img width="200" alt="Image" src="https://github.com/user-attachments/assets/aca57b06-3ea1-49e4-96fb-b2a00b8f8918" />

</div>
