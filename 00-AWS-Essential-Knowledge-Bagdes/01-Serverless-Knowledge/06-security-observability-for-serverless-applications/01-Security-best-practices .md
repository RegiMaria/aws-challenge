# 01 - Segurança e Observabilidade para Aplicações Serverless (Security and Observability for Serverless Applications)

Ao final a gente deve:

Objetivos do curso

- Resumir suas responsabilidades relacionadas à segurança de suas aplicações.

- Descrever as maneiras pelas quais a AWS cria isolamento entre funções e entre micro-máquinas virtuais (microVMs).

- Listar as melhores práticas de proteção de infraestrutura e fronteiras de rede para componentes com os quais sua arquitetura serverless possa interagir.

- Listar as melhores práticas para validar entradas e proteger dados em repouso (at rest) e em trânsito (in transit) em uma arquitetura serverless baseada em API.

- Resumir os tipos de alarmes do CloudWatch que você deve implementar para gerenciar suas aplicações serverless.

- Listar as principais considerações para registro de logs com AWS Lambda e Amazon API Gateway, tanto em termos do que capturar quanto do que evitar.

- Comparar as maneiras pelas quais você pode usar CloudTrail, AWS Config e X-Ray para monitorar e responder a anomalias e alterações em sua arquitetura serverless.

## Protegendo Arquiteturas Serverless (Securing Serverless Architectures)

As melhores práticas de segurança não mudam
Quando você projeta arquiteturas serverless, as mesmas melhores práticas aplicadas
para proteger qualquer outra arquitetura na nuvem continuam válidas. Nesta lição, você aprenderá como
aplicar e implementar essas práticas em arquiteturas serverless, incluindo o AWS Lambda.

Recursos principais para salvar e revisar:

- Best Practices for Security, Identity, & Compliance (Melhores Práticas de Segurança, Identidade e Conformidade)

- OWASP Secure Coding Practices Quick Reference Guide (Guia de Referência Rápida de Práticas de Codificação Segura da OWASP)

- Antes de mergulhar em como proteger suas arquiteturas serverless, é importante entender o que a AWS gerencia e pelo que você é responsável.

Modelo de Responsabilidade Compartilhada (Shared Responsibility Model)

O modelo de responsabilidade compartilhada define os aspectos gerenciados 
pela AWS e os aspectos gerenciados por você (cliente). A divisão muda entre a
perspectiva de serviços de infraestrutura e a perspectiva de serviços gerenciados (abstraídos):

Perspectiva de serviços de infraestrutura

- Responsabilidades do cliente (Segurança NA Nuvem / Security IN the Cloud): Dados do cliente;
Gerenciamento de Plataforma e Aplicação; Configuração de Sistema Operacional, Rede e Firewall;
Criptografia de Dados no Lado do Cliente; Criptografia de Dados no Lado do Servidor; Proteção de Tráfego de Rede;
e Controle de Acesso do Cliente.

- Responsabilidades da AWS (Segurança DA Nuvem / Security OF the Cloud): Serviços Fundamentais
da AWS (Computação, Armazenamento, Banco de Dados, Rede); Infraestrutura Global da AWS (Zonas de
Disponibilidade, Regiões, Edge Locations); Endpoints da AWS; e AWS IAM.

Perspectiva de serviços abstraídos (Serverless / Gerenciados):

- As responsabilidades da AWS sobem no gráfico, passando a englobar
também a configuração de sistema operacional, rede e firewall, bem como o gerenciamento de
plataforma e aplicação.

Esse é um dos grandes benefícios das arquiteturas serverless: ao utilizar serviços gerenciados 
da AWS, você transfere uma fatia maior da responsabilidade operacional para a AWS. Você continua 
enfrentando os mesmos desafios de segurança, mas a AWS gerencia grande parte deles em seu nome. 
Por exemplo, no AWS Lambda, você não é responsável pelo sistema operacional ou configuração de rede 
onde suas funções são executadas, nem pelo gerenciamento dos runtimes (a menos que você desenvolva 
custom runtimes).

Três Melhores Práticas de Segurança

As três práticas fundamentais de segurança pelas quais você é responsável são:

1.Seguir o princípio do menor privilégio (Follow the principle of least privilege).

2.Proteger dados em repouso e em trânsito (Protect data at rest and in transit).

3.Auditar seu sistema em busca de alterações, acessos inesperados, padrões incomuns ou erros 
(Audit your system for changes, unexpected access, unusual patterns, or errors).

*(Em outras palavras: deixe entrar apenas quem você convidou, tranque as coisas privadas
e garanta que ninguém esteja fazendo nada inesperado durante a visita.)*

1. Seguir o Princípio do Menor Privilégio

O Amazon API Gateway é a porta de entrada para sua aplicação, 
sendo o ponto ideal para impedir acessos não autorizados às suas APIs.

Limitando o acesso às APIs no API Gateway

Você possui três opções principais para autorizar o acesso às suas APIs através do API Gateway:

- AWS Identity and Access Management (IAM): Melhor opção para clientes que já estão dentro do seu ambiente
AWS ou que podem obter credenciais temporárias do IAM para acessar o ambiente. Adicione permissões restritas (least privilege) à respectiva IAM Role para invocar a API com segurança.

- Amazon Cognito: Fornece um serviço gerenciado com recursos nativos de cadastro e login (sign-in/sign-up) ou atua como um Provedor de Identidade (IdP) em cenários de identidade federada (ideal para aplicações web e mobile quando você ainda não possui um IdP).

- Lambda Authorizers: Invocam uma função Lambda para autenticar e validar um usuário contra um Provedor de Identidade (IdP) existente (como OAuth ou SAML) ou executar lógica adicional de autenticação. Também são úteis para centralizar o acesso a APIs entre múltiplas contas.

> Nota de escala: Quanto maior o ecossistema de APIs usando um Lambda Authorizer,
> maior será o número de invocações concorrentes daquela função. Utilize o cache de
> autorização (authorizer caching) (de 5 minutos a 1 hora) para reduzir o número de invocações repetidas.

Camadas adicionais de proteção na borda e rede

- Fronteiras de rede (VPC): Se sua função Lambda se conectar a uma VPC
ou componentes externos, proteja as fronteiras usando Security Groups e Network ACLs (NACLs).

- AWS WAF: Para APIs públicas, implemente o AWS WAF na frente do API Gateway para bloquear ataques comuns da web.

- Endpoints Otimizados para Borda (Edge-Optimized Endpoints): Fornecem automaticamente uma distribuição gerenciada
do Amazon CloudFront na frente da API, ajudando a mitigar ataques de negação de serviço (DDoS). Em regional endpoints,
você pode adicionar sua própria distribuição CloudFront.

- Validação de Requisições (Request Validation): Use o API Gateway para validar a estrutura do payload
recebido antes mesmo de invocar o backend, mitigando vulnerabilidades comuns da OWASP.

- Políticas de Recursos (Resource Policies): Restrinja ou permita o acesso à API por conta AWS, faixa de IP ou VPC Endpoint.

- Planos de Uso e Chaves de API (Usage Plans & API Keys): Regule o volume de requisições feitas por clientes específicos
aplicando throttling. (Importante: nunca dependa exclusivamente de API Keys como mecanismo de segurança/autenticação).
