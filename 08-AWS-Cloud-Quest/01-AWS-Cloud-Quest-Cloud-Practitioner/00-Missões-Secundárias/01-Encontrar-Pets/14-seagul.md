# 🦅 Seagul - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/a3683af7-694a-4b57-b474-6734dc7f493a" width="250" alt="Seagul Pet" />
</p>

- **Espécie:** Gaivota (*Seagull*)
- **Domínio AWS:** Tecnologia e Serviços em Nuvem (*Cloud Technology & Services*)
- **Tópico Principal:** Redes, Entrega de Conteúdo e DNS (Amazon CloudFront e Amazon Route 53)
- **Total de Quizzes:** 12
- **Status:** Adotado ✅

---

### Quiz 1: Rede de Entrega de Conteúdo (Amazon CloudFront)
**Enunciado:** Uma empresa de streaming de mídia precisa entregar conteúdos estáticos (vídeos, imagens e arquivos JavaScript) para usuários em escala global com baixíssima latência, armazenando cópias desses arquivos em Locais de Borda (*Edge Locations*) próximos aos clientes. Qual serviço deve ser utilizado?

- [ ] **A)** Amazon Route 53
- [x] **B)** Amazon CloudFront
- [ ] **C)** AWS Direct Connect
- [ ] **D)** Amazon S3 Transfer Acceleration

> **Por que esta é a resposta correta?**
> * O **Amazon CloudFront** é uma rede de entrega de conteúdo (CDN) global que acelera a entrega de sites, vídeos, APIs e conteúdos estáticos/dinâmicos, fazendo o cache dos dados em centenas de Locais de Borda distribuídos pelo mundo.
> * *Incorretas:* Route 53 é o serviço de DNS; Direct Connect é conexão física dedicada on-premises.
>
> 📖 **Documentação:** [O que é o Amazon CloudFront?](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)

---

### Quiz 2: Serviço de DNS Escalável (Amazon Route 53)
**Enunciado:** Um arquiteto precisa registrar um novo nome de domínio (`minhaempresa.com`) e configurar o roteamento de tráfego dos usuários para servidores web e balanceadores de carga da AWS de forma altamente disponível e confiável. Qual serviço gerencia o DNS?

- [ ] **A)** Amazon VPC Route Tables
- [ ] **B)** Amazon CloudFront
- [x] **C)** Amazon Route 53
- [ ] **D)** AWS App Mesh

> **Por que esta é a resposta correta?**
> * O **Amazon Route 53** é um serviço Web do Domain Name System (DNS) altamente disponível e escalável. Ele traduz nomes de domínio legíveis por humanos (como `exemplo.com`) em endereços IP numéricos (como `192.0.2.1`) que os computadores usam para se conectar.
>
> 📖 **Documentação:** [O que é o Amazon Route 53?](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/Welcome.html)

---

### Quiz 3: Políticas de Roteamento de DNS - Baseado em Latência
**Enunciado:** Uma aplicação está hospedada em duas Regiões AWS diferentes (N.orte da Virgínia e São Paulo). Como configurar o Amazon Route 53 para direcionar automaticamente cada usuário final para a Região AWS que ofereça o menor tempo de resposta (*latência*) no momento do acesso?

- [ ] **A)** Roteamento Simples (*Simple Routing*)
- [ ] **B)** Roteamento de Failover (*Failover Routing*)
- [x] **C)** Roteamento baseado em Latência (*Latency-based Routing*)
- [ ] **D)** Roteamento Geográfico (*Geolocation Routing*)

> **Por que esta é a resposta correta?**
> * O **Roteamento baseado em Latência** permite que o Route 53 avalie o desempenho da rede em relação às diferentes regiões da AWS e encaminhe as solicitações do usuário para a região que proporcione a menor latência de resposta.
>
> 📖 **Documentação:** [Roteamento baseado em latência do Route 53](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy-latency.html)

---

### Quiz 4: Políticas de Roteamento de DNS - Failover para Alta Disponibilidade
**Enunciado:** Uma empresa possui um site principal em uma infraestrutura primária na AWS e um site de contingência (backup) estático hospedado em um bucket S3. O Route 53 deve monitorar a saúde do site principal e desviar o tráfego automaticamente para o backup se o servidor principal falhar. Qual política de roteamento usar?

- [ ] **A)** Roteamento Multivalor (*Multivalue Answer Routing*)
- [x] **B)** Roteamento de Failover (*Failover Routing*)
- [ ] **C)** Roteamento Geográfico (*Geolocation Routing*)
- [ ] **D)** Roteamento Round Robin Ponderado

> **Por que esta é a resposta correta?**
> * O **Roteamento de Failover** é usado quando se deseja configurar uma arquitetura ativa-passiva (resiliência a desastres). O Route 53 verifica a integridade do recurso primário e direciona o tráfego para ele, alternando automaticamente para o recurso secundário em caso de falha.
>
> 📖 **Documentação:** [Roteamento de failover do Route 53](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy-failover.html)

---

### Quiz 5: Restrição de Acesso a Origens no CloudFront (Origin Access Control - OAC)
**Enunciado:** Uma empresa hospeda imagens privadas em um bucket Amazon S3 e deseja que os usuários acessem esses arquivos **apenas** através de uma distribuição do Amazon CloudFront, bloqueando qualquer tentativa de acesso direto ao bucket S3 via URL pública. Qual recurso deve ser configurado?

- [ ] **A)** Grupos de Segurança da VPC vinculados ao S3.
- [x] **B)** Controle de Acesso de Origem (*Origin Access Control - OAC*) ou OAI no CloudFront.
- [ ] **C)** Políticas de Controle de Serviços (SCPs) do AWS Organizations.
- [ ] **D)** Criptografia KMS no nível do objeto.

> **Por que esta é a resposta correta?**
> * O **Origin Access Control (OAC)** (evolução do antigo OAI) permite proteger o bucket S3 de modo que apenas identidades do CloudFront tenham permissão para ler os objetos, impedindo acessos diretos externos à origem.
>
> 📖 **Documentação:** [Restringir o acesso a uma origem do Amazon S3 usando o OAC](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)

---

### Quiz 6: Invalidação de Cache no CloudFront (Cache Invalidation)
**Enunciado:** Um desenvolvedor atualizou um arquivo de estilo `style.css` em uma origem S3, mas os usuários continuam visualizando a versão antiga no site devido ao armazenamento em cache do CloudFront. Como forçar o CloudFront a descartar a versão antiga antes do tempo de expiração (*TTL*) padrão?

- [ ] **A)** Reiniciar a instância EC2 de origem.
- [x] **B)** Criar uma solicitação de invalidação de cache (*Cache Invalidation*) no CloudFront para o caminho do arquivo.
- [ ] **C)** Alterar o tipo de volume EBS para `io2`.
- [ ] **D)** Atualizar os servidores DNS no Route 53.

> **Por que esta é a resposta correta?**
> * Se você precisar atualizar um arquivo antes que ele expire no cache dos Locais de Borda, você pode enviar uma solicitação de **invalidação** (*Invalidation*), fazendo com que o CloudFront busque a versão mais recente diretamente na origem na próxima solicitação.
>
> 📖 **Documentação:** [Invalidar ficheiros no CloudFront](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html)

---

### Quiz 7: Processamento de Borda com CloudFront Functions e Lambda@Edge
**Enunciado:** Uma empresa precisa executar códigos leves de JavaScript para modificar cabeçalhos de HTTP, redirecionar usuários ou inspecionar tokens de autenticação diretamente nos Locais de Borda da AWS, o mais próximo possível do usuário final, reduzindo a carga nos servidores centrais. Qual tecnologia utilizar?

- [x] **A)** CloudFront Functions ou Lambda@Edge
- [ ] **B)** AWS Step Functions em Região padrão
- [ ] **C)** Amazon SQS com filas FIFO
- [ ] **D)** AWS Direct Connect Router

> **Por que esta é a resposta correta?**
> * O **CloudFront Functions** e o **Lambda@Edge** permitem escrever códigos que rodam na rede de borda (*Edge*) da AWS em resposta a eventos do CloudFront, viabilizando personalização de conteúdo, testes A/B e validação de segurança antes mesmo de chegar à origem.
>
> 📖 **Documentação:** [Personalização na borda com CloudFront Functions](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html)

---

### Quiz 8: Roteamento Baseado em Região Geográfica (Geolocation Routing)
**Enunciado:** Uma empresa de notícias possui conteúdos restritos por direitos autorais e precisa garantir que visitantes da Europa sejam direcionados exclusivamente para servidores de cache na Europa, enquanto visitantes da Ásia vão para servidores asiáticos. Qual política do Route 53 aplica essa regra?

- [ ] **A)** Roteamento de Latência (*Latency Routing*)
- [x] **B)** Roteamento Geográfico (*Geolocation Routing*)
- [ ] **C)** Roteamento Multivalor (*Multivalue*)
- [ ] **D)** Roteamento Simples

> **Por que esta é a resposta correta?**
> * O **Roteamento Geográfico (Geolocation)** permite escolher como o tráfego é roteado com base na localização geográfica (país ou continente) de onde originou a consulta DNS, ideal para conformidade regional de conteúdo ou idiomas.
>
> 📖 **Documentação:** [Roteamento geográfico do Route 53](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/routing-policy-geolocation.html)

---

### Quiz 9: Proteção Contra Ataques de Negação de Serviço (AWS Shield)
**Enunciado:** Um portal de notícias sofre um ataque coordenado de negação de serviço distribuído (DDoS) na camada de rede e transporte (camadas 3 e 4). Qual serviço gerenciado protege automaticamente o site e o Route 53 contra esse tipo de ataque sem custos adicionais na versão Standard?

- [x] **A)** AWS Shield Standard
- [ ] **B)** AWS WAF (Web Application Firewall)
- [ ] **C)** Amazon GuardDuty
- [ ] **D)** AWS Inspector

> **Por que esta é a resposta correta?**
> * O **AWS Shield Standard** é ativado automaticamente e de forma gratuita em todas as contas AWS, protegendo recursos contra os ataques de DDoS mais comuns e frequentes nas camadas 3 e 4 (infraestrutura). O Shield Advanced oferece proteção customizada para camadas de aplicação (camada 7).
>
> 📖 **Documentação:** [O que é o AWS Shield?](https://docs.aws.amazon.com/pt_br/waf/latest/developerguide/shield-chapter.html)

---

### Quiz 10: Firewall de Aplicação Web (AWS WAF)
**Enunciado:** Um engenheiro de segurança precisa bloquear tráfego malicioso direcionado a uma aplicação web no CloudFront ou ALB, filtrando ataques comuns como Injeção de SQL (*SQL Injection*) e Cross-Site Scripting (XSS) com base em regras personalizadas. Qual serviço deve ser associado?

- [ ] **A)** AWS Shield Advanced
- [x] **B)** AWS WAF (Web Application Firewall)
- [ ] **C)** Amazon VPC Network ACLs
- [ ] **D)** AWS Config Rules

> **Por que esta é a resposta correta?**
> * O **AWS WAF** permite monitorar solicitações HTTP/HTTPS encaminhadas para o CloudFront, API Gateway ou ALB, controlando o acesso ao conteúdo através de regras de segurança customizadas que bloqueiam ataques como SQL Injection e XSS.
>
> 📖 **Documentação:** [O que é o AWS WAF?](https://docs.aws.amazon.com/pt_br/waf/latest/developerguide/waf-chapter.html)

---

### Quiz 11: Aceleração de Transferência de Dados de Longa Distância (S3 Transfer Acceleration)
**Enunciado:** Uma agência de publicidade no Brasil precisa enviar arquivos de vídeo pesados (vários gigabytes) de forma rápida e segura para um bucket S3 localizado na Região de Tóquio, utilizando a rede global de borda da AWS para otimizar o trajeto pela internet. Qual recurso do S3 acelera esse upload?

- [x] **A)** Amazon S3 Transfer Acceleration
- [ ] **B)** AWS Storage Gateway com fita virtual
- [ ] **C)** Amazon CloudFront Invalidations
- [ ] **D)** AWS Direct Connect privado

> **Por que esta é a resposta correta?**
> * O **Amazon S3 Transfer Acceleration** utiliza os Locais de Borda da AWS distribuídos globalmente para direcionar os uploads dos clientes para o Ponto de Presença mais próximo, otimizando o tráfego na rota de rede da AWS até o bucket de destino.
>
> 📖 **Documentação:** [Aceleração de transferência do Amazon S3](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/userguide/transfer-acceleration.html)

---

### Quiz 12: Monitoramento de Integridade de DNS (Route 53 Health Checks)
**Enunciado:** Como o Amazon Route 53 determina se um endpoint externo (como um servidor web fora da AWS ou uma instância EC2) está operacional antes de enviar uma consulta DNS para ele?

- [ ] **A)** Através do painel AWS CloudTrail Logs.
- [x] **B)** Por meio de verificações de integridade globais (*Health Checks*) configuradas no Route 53.
- [ ] **C)** Consultando o status do EBS Snapshot.
- [ ] **D)** Analisando as regras do AWS WAF.

> **Por que esta é a resposta correta?**
> * O **Route 53 Health Checks** monitora a integridade dos seus recursos. Vários servidores distribuídos pelo mundo testam a acessibilidade do seu endpoint via HTTP, HTTPS ou TCP, alimentando as políticas de roteamento inteligente (como failover).
>
> 📖 **Documentação:** [Como o Route 53 confere a integridade dos recursos](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/dns-health-checks.html)
