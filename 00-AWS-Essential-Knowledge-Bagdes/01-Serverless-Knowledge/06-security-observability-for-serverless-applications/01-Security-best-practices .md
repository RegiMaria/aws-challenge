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
