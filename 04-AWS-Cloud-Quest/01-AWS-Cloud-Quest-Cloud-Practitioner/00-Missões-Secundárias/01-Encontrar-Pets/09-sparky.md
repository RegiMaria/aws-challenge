# 🐘 Sparky - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/6bd73294-60c2-48be-880e-c993d9150394" width="250" alt="Sparky Pet" />
</p>

- **Espécie:** Elefante (*Elephant*)
- **Domínio AWS:** Tecnologia e Serviços em Nuvem (*Cloud Technology & Services*)
- **Tópico Principal:** Redes e Infraestrutura Virtual (Amazon VPC, Sub-redes, Gateways e Tabelas de Roteamento)
- **Total de Quizzes:** 12
- **Status:** Adotado ✅

---

### Quiz 1: Conceito Fundamental de Amazon VPC
**Enunciado:** Uma empresa deseja isolar logicamente seus recursos de computação e rede em uma seção dedicada da nuvem AWS, definindo seu próprio intervalo de endereços IP (CIDR), sub-redes e tabelas de roteamento. Qual serviço habilita essa rede privada virtual?

- [ ] **A)** AWS Direct Connect
- [x] **B)** Amazon Virtual Private Cloud (Amazon VPC)
- [ ] **C)** Amazon Route 53
- [ ] **D)** AWS Transit Gateway

> **Por que esta é a resposta correta?**
> * O **Amazon VPC** permite provisionar uma seção isolada logicamente da nuvem AWS, onde o usuário tem controle total sobre o ambiente de rede virtual, incluindo seleção do intervalo de IP (CIDR), criação de sub-redes e configuração de roteamento e gateways.
>
> 📖 **Documentação:** [O que é a Amazon VPC?](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/what-is-amazon-vpc.html)

---

### Quiz 2: Diferença entre Sub-redes Públicas e Privadas
**Enunciado:** Qual é a característica técnica que define uma sub-rede como **pública** dentro de uma Amazon VPC?

- [ ] **A)** Estar localizada em uma Zona de Disponibilidade primária da AWS.
- [x] **B)** Ter uma tabela de roteamento associada que possui uma rota direta para um Internet Gateway (IGW).
- [ ] **C)** Conter apenas instâncias EC2 com o agente do CloudWatch instalado.
- [ ] **D)** Possuir um grupo de segurança que permite todas as portas de entrada sem restrição.

> **Por que esta é a resposta correta?**
> * Uma sub-rede torna-se **pública** quando sua tabela de roteamento (*Route Table*) inclui uma rota direcionando o tráfego destinado à internet (`0.0.0.0/0`) para um **Internet Gateway (IGW)** associado à VPC.
>
> 📖 **Documentação:** [Sub-redes da VPC](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/configure-subnets.html)

---

### Quiz 3: Conectividade de Saída para Sub-redes Privadas (NAT Gateway)
**Enunciado:** As instâncias EC2 contendo bancos de dados ficam em uma sub-rede privada para evitar conexões diretas vindas da internet. No entanto, essas instâncias precisam baixar atualizações e patches de segurança externos sem expor suas interfaces a conexões de entrada. Qual componente deve ser utilizado?

- [ ] **A)** Internet Gateway (IGW)
- [x] **B)** NAT Gateway
- [ ] **C)** Virtual Private Gateway (VGW)
- [ ] **D)** AWS Direct Connect

> **Por que esta é a resposta correta?**
> * O **NAT Gateway** (Network Address Translation) permite que instâncias em uma sub-rede privada se conectem à internet ou a outros serviços AWS para tráfego de saída (*outbound*), impedindo simultaneamente que a internet inicie conexões de entrada (*inbound*) não solicitadas com essas instâncias.
>
> 📖 **Documentação:** [Gateways NAT](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/vpc-nat-gateway.html)

---

### Quiz 4: Firewall de Recursos vs. Firewall de Sub-rede (Security Groups vs. NACLs)
**Enunciado:** Um engenheiro precisa configurar regras de segurança para a rede. Qual das opções descreve corretamente uma diferença fundamental entre os **Grupos de Segurança (Security Groups)** e as **Listas de Controle de Acesso à Rede (Network ACLs - NACLs)**?

- [x] **A)** Security Groups são *stateful* (respostas autorizadas automaticamente) e atuam no nível de instância/ENI; NACLs são *stateless* e atuam no nível de sub-rede.
- [ ] **B)** NACLs são *stateful* e atuam no nível de instância; Security Groups são *stateless* e atuam no nível de VPC.
- [ ] **C)** Security Groups suportam regras de bloqueio explícito (*Deny*); NACLs aceitam apenas regras de permissão (*Allow*).
- [ ] **D)** NACLs são aplicadas apenas para tráfego IPv6, enquanto Security Groups gerenciam IPv4.

> **Por que esta é a resposta correta?**
> * Os **Security Groups** são *stateful* (se o tráfego de entrada é permitido, a resposta de saída é liberada automaticamente) e funcionam como firewall virtual no nível da interface de rede (ENI/instância). As **NACLs** são *stateless* (exigem regras explícitas para entrada e saída) e atuam como camada defensiva no nível do limite da sub-rede, além de permitirem regras de negação (*Deny*).
>
> 📖 **Documentação:** [Comparar security groups e Network ACLs](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/vpc-network-acls.html#comparison-oc-sg-nacls)

---

### Quiz 5: Conexão Privada entre Duas VPCs (VPC Peering)
**Enunciado:** Uma empresa possui duas Amazon VPCs separadas em uma mesma conta AWS (uma para o setor de Finanças e outra para o setor de TI) e precisa conectar as redes para que instâncias se comuniquem usando endereços IP privados com baixa latência e alta largura de banda. Qual recurso viabiliza esse emparelhamento?

- [ ] **A)** AWS Direct Connect
- [x] **B)** Conexão de emparelhamento da VPC (*VPC Peering*)
- [ ] **C)** Internet Gateway
- [ ] **D)** AWS Site-to-Site VPN

> **Por que esta é a resposta correta?**
> * O **VPC Peering** é uma conexão de rede entre duas VPCs que permite rotear o tráfego de dados usando endereços IPv4 ou IPv6 privados. As instâncias se comunicam como se estivessem na mesma rede privada, sem transitar pela internet pública.
>
> 📖 **Documentação:** [O que é o emparelhamento de VPCs?](https://docs.aws.amazon.com/pt_br/vpc/latest/peering/what-is-vpc-peering.html)

---

### Quiz 6: Acesso Privado aos Serviços AWS (VPC Endpoints)
**Enunciado:** Uma aplicação em uma sub-rede privada precisa salvar arquivos no Amazon S3 de forma contínua. Por motivos de compliance e segurança, o tráfego não deve passar pela internet nem por um NAT Gateway. Qual recurso permite acessar o S3 diretamente através da rede privada da AWS?

- [x] **A)** VPC Endpoint (Gateway ou Interface Endpoint)
- [ ] **B)** AWS Client VPN
- [ ] **C)** Internet Gateway
- [ ] **D)** Amazon Route 53 Resolver

> **Por que esta é a resposta correta?**
> * Os **VPC Endpoints** (via AWS PrivateLink) permitem conectar de forma privada sua VPC a serviços AWS compatíveis sem exigir um Internet Gateway, NAT Gateway, conexão VPN ou AWS Direct Connect. O tráfego entre a VPC e o serviço permanece inteiramente dentro da rede backbone privada da AWS.
>
> 📖 **Documentação:** [Acessar serviços AWS usando VPC Endpoints](https://docs.aws.amazon.com/pt_br/vpc/latest/privatelink/vpc-endpoints.html)

---

### Quiz 7: Conectividade Privada Dedicada On-Premises (AWS Direct Connect)
**Enunciado:** Uma corporação precisa conectar seu centro de dados físico (*on-premises*) à sua Amazon VPC. O projeto exige uma conexão de rede física dedicada com velocidade de 1 Gbps a 100 Gbps, latência extremamente baixa e consistente, ignorando completamente a internet pública. Qual serviço atende a essa especificação?

- [ ] **A)** AWS Site-to-Site VPN
- [ ] **B)** VPC Peering
- [x] **C)** AWS Direct Connect
- [ ] **D)** Amazon CloudFront

> **Por que esta é a resposta correta?**
> * O **AWS Direct Connect** estabelece uma conexão de rede física dedicada entre a rede da empresa e uma das localizações do AWS Direct Connect. Ele contorna os provedores de serviços de internet para oferecer largura de banda garantida e custos de transferência reduzidos.
>
> 📖 **Documentação:** [O que é o AWS Direct Connect?](https://docs.aws.amazon.com/pt_br/directconnect/latest/UserGuide/Welcome.html)

---

### Quiz 8: Conexão Criptografada On-Premises via Internet (AWS Site-to-Site VPN)
**Enunciado:** Uma filial de escritório precisa de uma conexão rápida, segura e criptografada baseada em túneis IPsec para conectar seu roteador local à Amazon VPC através da internet pública. Qual é a opção mais econômica e rápida de configurar?

- [x] **A)** AWS Site-to-Site VPN
- [ ] **B)** AWS Direct Connect
- [ ] **C)** VPC Endpoint
- [ ] **D)** Amazon Route 53

> **Por que esta é a resposta correta?**
> * A **AWS Site-to-Site VPN** cria conexões criptografadas (túneis IPsec) entre o equipamento de rede do cliente no ambiente físico (*Customer Gateway*) e o *Virtual Private Gateway* ou *Transit Gateway* na AWS, utilizando a internet existente para implementar o túnel rapidamente.
>
> 📖 **Documentação:** [O que é a AWS Site-to-Site VPN?](https://docs.aws.amazon.com/pt_br/vpn/latest/s2svpn/VPC_VPN.html)

---

### Quiz 9: Roteamento Centralizado em Escala (AWS Transit Gateway)
**Enunciado:** Uma empresa expandiu sua arquitetura para mais de 100 VPCs e múltiplos ambientes físicos *on-premises*. Gerenciar conexões de emparelhamento VPC Peering e túneis VPN individuais tornou-se insustentável pela complexidade de malha (*mesh*). Qual hub central de roteamento unifica e simplifica essa topologia de rede?

- [ ] **A)** Internet Gateway
- [ ] **B)** AWS PrivateLink
- [x] **C)** AWS Transit Gateway
- [ ] **D)** Amazon VPC Flow Logs

> **Por que esta é a resposta correta?**
> * O **AWS Transit Gateway** funciona como um roteador de rede centralizado. Ele conecta Amazon VPCs, contas AWS e redes locais a um único hub, eliminando conexões de emparelhamento complexas de ponto a ponto e simplificando o controle de tráfego global.
>
> 📖 **Documentação:** [O que é o AWS Transit Gateway?](https://docs.aws.amazon.com/pt_br/vpc/latest/tgw/what-is-transit-gateway.html)

---

### Quiz 10: Captura e Análise de Tráfego de Rede (VPC Flow Logs)
**Enunciado:** Para investigar um problema de conectividade ou auditagens de segurança, um analista precisa capturar informações sobre o tráfego IP de entrada e saída nas interfaces de rede (ENIs) dentro de sua VPC, incluindo aceitações (*ACCEPT*) e rejeições (*REJECT*) de conexões. Qual recurso deve ser habilitado?

- [ ] **A)** AWS CloudTrail
- [x] **B)** VPC Flow Logs
- [ ] **C)** Amazon CloudWatch Synthetics
- [ ] **D)** AWS Route 53 Resolver

> **Por que esta é a resposta correta?**
> * O **VPC Flow Logs** permite capturar e publicar informações sobre o tráfego IP que entra e sai das interfaces de rede da VPC no CloudWatch Logs ou no Amazon S3. É fundamental para diagnosticar regras de Grupos de Segurança/NACLs rígidas demais.
>
> 📖 **Documentação:** [VPC Flow Logs](https://docs.aws.amazon.com/pt_br/vpc/latest/userguide/flow-logs.html)

---

### Quiz 11: Espelhamento de Tráfego para Inspeção (VPC Traffic Mirroring)
**Enunciado:** Uma equipe de segurança da informação precisa copiar o tráfego bruto de rede de sub-redes de produção e enviá-lo para um appliance de detecção de intrusão (IDS/IPS) e inspeção profunda de pacotes (*Deep Packet Inspection*) sem instalar agentes nos servidores. Qual recurso nativo da VPC realiza essa cópia?

- [x] **A)** VPC Traffic Mirroring
- [ ] **B)** VPC Flow Logs
- [ ] **C)** Amazon EventBridge
- [ ] **D)** AWS WAF

> **Por que esta é a resposta correta?**
> * O **VPC Traffic Mirroring** permite extrair e copiar o tráfego de rede de uma interface de rede elástica (ENI) e enviá-lo para appliances virtuais de segurança ou análise, permitindo inspeção de pacotes sem impactar a performance das instâncias de origem.
>
> 📖 **Documentação:** [O que é o VPC Traffic Mirroring?](https://docs.aws.amazon.com/pt_br/vpc/latest/mirroring/what-is-traffic-mirroring.html)

---

### Quiz 12: Resolução de Endereços IP (VPC DNS e Route 53 Private Hosted Zones)
**Enunciado:** Uma empresa deseja que os nomes de domínio internos de seus servidores (ex: `db.interno.empresa.local`) sejam resolvidos apenas por instâncias situadas dentro de suas Amazon VPCs privadas, sem expor esses registros DNS para a internet pública. Qual funcionalidade do Amazon Route 53 atende a essa exigência?

- [ ] **A)** Zonas Hospedadas Públicas (*Public Hosted Zones*)
- [x] **B)** Zonas Hospedadas Privadas (*Private Hosted Zones*)
- [ ] **C)** Roteamento de Latência (*Latency Routing*)
- [ ] **D)** Registro de Domínios do Route 53

> **Por que esta é a resposta correta?**
> * As **Private Hosted Zones** do Amazon Route 53 armazenam informações sobre domínios DNS cujos registros você deseja resolver estritamente dentro de uma ou mais Amazon VPCs associadas, mantendo o mapeamento de nomes totalmente oculto da internet externa.
>
> 📖 **Documentação:** [Trabalhar com zonas hospedadas privadas do Route 53](https://docs.aws.amazon.com/pt_br/Route53/latest/DeveloperGuide/hosted-zones-private.html)
