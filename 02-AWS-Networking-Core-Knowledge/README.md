<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" width="220" alt="AWS Logo"/>

# 🌐 02-Networking Core - Knowledge Badge Readiness Path

</div>

Este repositório documenta minha jornada para conquistar o **[AWS Networking Core Knowledge Badge](https://skillbuilder.aws/learning-plan/KG2Z8NEHYV/networking-core--knowledge-badge-readiness-path-includes-labs/ZDTDSRZ9NE?_sa=search&_si=7288b951-3614-43d2-b159-a0e0fbaba2be&_sq=ad2226e6-d555-42fa-ad60-ed70b82c2070)**, disponível no AWS Skill Builder.

O learning path é composto por **27 treinamentos** (cursos digitais, labs e a avaliação final), que juntos somam cerca de **30h57min** de conteúdo, cobrindo desde os fundamentos de VPC, subnets e rotas até load balancers, CloudFront, Direct Connect, Transit Gateway e PrivateLink.

> 📅 **Cada tópico da trilha = 1 dia de estudo.**

A cada dia eu completo um treinamento, tiro um print/certificado de conclusão (quando disponível) e salvo na pasta `AWS-Networking-Core/` com o padrão de nome `day-XX-nome-do-topico.png`. Todo o progresso fica registrado na tabela lá embaixo.

Ao final da trilha, faço o **Networking Core Knowledge Badge Assessment** (nota mínima de 80%) para receber o badge digital emitido pela Credly.

---

## 📚 Resumo dos tópicos

**1. Networking Core Pre-Assessment** - Avaliação diagnóstica para identificar seu nível de conhecimento em networking na AWS antes de começar a trilha, e direcionar quais cursos focar primeiro.

**2. AWS Networking Basics** - Introdução aos serviços de rede da AWS, visão geral dos serviços disponíveis e casos de uso comuns.

**3. Subnets, Gateways, and Route Tables Explained** - Usando uma arquitetura de três camadas como exemplo, explica a diferença entre subnets públicas e privadas, e como gateways e route tables funcionam no roteamento de rede.

**4. Configuring and Deploying VPCs with Multiple Subnets** - Como configurar e implantar uma VPC com múltiplas subnets em duas Availability Zones, incluindo boas práticas e cenários comuns de arquitetura de rede.

**5. Introduction to Amazon Virtual Private Cloud (VPC)** - Lab prático: usar o wizard da VPC para criar uma VPC, anexar um Internet Gateway, adicionar uma subnet e configurar o roteamento.

**6. Building Your First Amazon Virtual Private Cloud (VPC)** - Lab prático: criar uma VPC básica sem usar o wizard, com controle total sobre IP range, subnets, route tables e gateways (IPv4 e IPv6).

**7. AWS Network Connectivity Options** - Opções de conectividade de rede além do nível fundamental: como escolher a forma mais segura, escalável e altamente disponível de montar sua rede, e padrões de design de rede complexos.

**8. Introduction to Amazon VPC Lattice** - Como o VPC Lattice ajuda a alcançar conectividade segura e escalável para comunicação service-to-service na AWS.

**9. Introduction to AWS Cloud WAN** - Dashboard central para conectar filiais, data centers e VPCs, permitindo construir, gerenciar e monitorar uma rede global.

**10. Introduction to Amazon CloudFront** - Lab prático: criar uma distribuição CloudFront para servir, via CDN, um arquivo de imagem público armazenado em um bucket S3.

**11. Caching Static Files with Amazon CloudFront** - Lab prático: integrar um site estático hospedado em S3 com o CloudFront para entregar conteúdo a partir das edge locations da AWS.

**12. Introduction to Amazon Direct Connect** - Visão geral do AWS Direct Connect, que permite estabelecer uma conexão de rede dedicada entre seu ambiente on-premises e a AWS, incluindo aspectos técnicos e casos de uso comuns.

**13. AWS Networking Practical Approaches** - Abordagens práticas para desafios comuns de rede na AWS: configurações, roteamento, largura de banda, latência e custo, com cenários e boas práticas de excelência operacional.

**14. Application Load Balancer Getting Started** - Benefícios, casos de uso típicos e conceitos técnicos do Application Load Balancer, que distribui tráfego na camada 7 (OSI) entre múltiplos targets.

**15. Gateway Load Balancer Getting Started** - Como implantar e gerenciar appliances virtuais (firewalls, IDS, deep packet inspection) usando o Gateway Load Balancer como gateway de rede transparente.

**16. Network Load Balancer (NLB) Getting Started** - Como o Network Load Balancer opera na camada 4 (transporte), lidando com milhões de requisições por segundo com latência ultra baixa.

**17. Introduction to Elastic Load Balancing** - Lab prático: criar e testar um Network Elastic Load Balancer distribuindo tráfego entre dois servidores web em Availability Zones diferentes.

**18. Working with Elastic Load Balancing** - Lab prático: lançar instâncias EC2 com servidores web, configurar um ELB, balancear requisições entre elas e observar métricas no CloudWatch.

**19. Configure and Deploy AWS PrivateLink** - Como configurar e implantar o PrivateLink, criar VPC endpoint services com network load balancers e DNS privado, gerenciar permissões e aceitar conexões.

**20. Configure and Deploy AWS Client VPN** - Como configurar o Client VPN como administrador (console e CLI) e como conectar como usuário final a partir do próprio dispositivo.

**21. Introduction to Amazon API Gateway** - Visão geral do API Gateway: como criar, publicar, manter, monitorar e proteger APIs em qualquer escala, incluindo demonstração de criação e deploy de recursos.

**22. Introduction to Amazon API Gateway (Lab)** - Lab prático: criar um microsserviço de FAQ que retorna um par pergunta/resposta em JSON via API Gateway integrado a uma função Lambda.

**23. Working with Amazon CloudFront for Dynamic Content Acceleration** - Lab prático: distribuir um site dinâmico (phpBB) via CloudFront, acelerando conteúdo estático e dinâmico a partir das edge locations.

**24. Resolve VPC Routing Conflicts** - Lab prático avançado: diagnosticar e resolver conflitos de roteamento entre duas VPCs peered (ALB e servidores web) que impedem o acesso a um site.

**25. Comparing Amazon VPC Peering and AWS Transit Gateway** - Lab prático: configurar conectividade entre 3 VPCs em 2 regiões usando primeiro VPC Peering e depois Transit Gateway, comparando as duas abordagens.

**26. Configuring and Deploying Amazon VPC for a 3-tier Web App** - Lab prático: configurar uma VPC e implantar uma aplicação web de três camadas.

**27. Networking Core Knowledge Badge Assessment** - Avaliação final da trilha. Nota mínima de 80% para conquistar o badge (emitido pela Credly em 5-7 dias úteis).

---

## 📊 Progresso

| Dia | Data | Tópico | Descrição | Certificado (repo) | Link AWS |
|----|------|--------|-----------|---------------------|----------|
| 01 | - | Networking Core Pre-Assessment | Diagnóstico inicial de conhecimento em networking. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/BA27UY1TDP/networking-core-preassessment/E4F56BA75S?parentId=ZDTDSRZ9NE) |
| 02 | - | AWS Networking Basics | Introdução aos serviços de rede da AWS. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/S1VYRYHD8V/aws-networking-basics/SKP7248UVF?parentId=ZDTDSRZ9NE) |
| 03 | - | Subnets, Gateways, and Route Tables Explained | Subnets públicas/privadas, gateways e route tables. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/D3C12EX9SU/subnets-gateways-and-route-tables-explained/FACFF89ATD?parentId=ZDTDSRZ9NE) |
| 04 | - | Configuring and Deploying VPCs with Multiple Subnets | VPC com múltiplas subnets em duas AZs. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/4HWA8PME5S/configuring-and-deploying-vpcs-with-multiple-subnets/BTRCDVX2RU?parentId=ZDTDSRZ9NE) |
| 05 | - | Introduction to Amazon VPC | Lab: criar VPC, Internet Gateway, subnet e rota. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/PH6Z6EVH8Z/introduction-to-amazon-virtual-private-cloud-vpc/PA8H7FUE15?parentId=ZDTDSRZ9NE) |
| 06 | - | Building Your First Amazon VPC | Lab: criar VPC do zero, sem o wizard. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/R4AK6FUM4D/building-your-first-amazon-virtual-private-cloud-vpc/C5RF3QG2V1?parentId=ZDTDSRZ9NE) |
| 07 | - | AWS Network Connectivity Options | Opções avançadas de conectividade de rede. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/WQJNBEZYDW/aws-network-connectivity-options/PZDF9Z7DN5?parentId=ZDTDSRZ9NE) |
| 08 | - | Introduction to Amazon VPC Lattice | Conectividade service-to-service com VPC Lattice. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/AJT74B5N5V/introduction-to-amazon-vpc-lattice/FWAQWCPW25?parentId=ZDTDSRZ9NE) |
| 09 | - | Introduction to AWS Cloud WAN | Rede global unificada com AWS Cloud WAN. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/K9EZWX2M76/introduction-to-aws-cloud-wan/D2YBDR71RB?parentId=ZDTDSRZ9NE) |
| 10 | - | Introduction to Amazon CloudFront | Lab: criar uma distribuição CloudFront com S3. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/NVJ224EA5N/introduction-to-amazon-cloudfront/7JNHFE69BD?parentId=ZDTDSRZ9NE) |
| 11 | - | Caching Static Files with Amazon CloudFront | Lab: site estático S3 integrado ao CloudFront. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/VQ1URQKWXT/caching-static-files-with-amazon-cloudfront/G9Z699UYUY?parentId=ZDTDSRZ9NE) |
| 12 | - | Introduction to Amazon Direct Connect | Conexão dedicada on-premises → AWS. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/DFZQX2499F/introduction-to-amazon-direct-connect/FNPC4S8C9W?parentId=ZDTDSRZ9NE) |
| 13 | - | AWS Networking Practical Approaches | Abordagens práticas para desafios de rede. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/B6A3C8NAXG/aws-networking-practical-approaches/3D9CNSEW84?parentId=ZDTDSRZ9NE) |
| 14 | - | Application Load Balancer Getting Started | Introdução ao Application Load Balancer (camada 7). | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/72MYSKW681/application-load-balancer-getting-started/1TVTD6FGSD?parentId=ZDTDSRZ9NE) |
| 15 | - | Gateway Load Balancer Getting Started | Introdução ao Gateway Load Balancer. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/Q6KPJV1182/gateway-load-balancer-getting-started/BUNKE32EH3?parentId=ZDTDSRZ9NE) |
| 16 | - | Network Load Balancer (NLB) Getting Started | Introdução ao Network Load Balancer (camada 4). | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/DWYSUVJ5RQ/network-load-balancer-nlb-getting-started/4VBEW4SP7C?parentId=ZDTDSRZ9NE) |
| 17 | - | Introduction to Elastic Load Balancing | Lab: criar e testar um Network ELB. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/EVX5H1CFEB/introduction-to-elastic-load-balancing/N33EKUP27H?parentId=ZDTDSRZ9NE) |
| 18 | - | Working with Elastic Load Balancing | Lab: balancear tráfego entre instâncias EC2 com ELB. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/U3SPYA234U/working-with-elastic-load-balancing/BRPX7TASWB?parentId=ZDTDSRZ9NE) |
| 19 | - | Configure and Deploy AWS PrivateLink | Configurar VPC endpoint services com PrivateLink. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/ZJH589YJHQ/configure-and-deploy-aws-privatelink/PCN7GQHC71?parentId=ZDTDSRZ9NE) |
| 20 | - | Configure and Deploy AWS Client VPN | Configurar e conectar ao AWS Client VPN. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/PNY68FFZTK/configure-and-deploy-aws-client-vpn/C5JQ2PJ72H?parentId=ZDTDSRZ9NE) |
| 21 | - | Introduction to Amazon API Gateway | Criar, publicar e proteger APIs com API Gateway. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/2CWTRRKXXU/introduction-to-amazon-api-gateway/K51ZBCKRSK?parentId=ZDTDSRZ9NE) |
| 22 | - | Introduction to Amazon API Gateway (Lab) | Lab: microsserviço de FAQ com API Gateway + Lambda. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/87NHV2TSKV/introduction-to-amazon-api-gateway/3S1482KMEZ?parentId=ZDTDSRZ9NE) |
| 23 | - | Working with Amazon CloudFront for Dynamic Content Acceleration | Lab: acelerar conteúdo dinâmico com CloudFront. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/83F9YRCA3P/working-with-amazon-cloudfront-for-dynamic-content-acceleration/4ZBWKDB8JK?parentId=ZDTDSRZ9NE) |
| 24 | - | Resolve VPC Routing Conflicts | Lab avançado: troubleshooting de roteamento entre VPCs. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/NM24X2NSEB/resolve-vpc-routing-conflicts/JX7G19NQYX?parentId=ZDTDSRZ9NE) |
| 25 | - | Comparing VPC Peering and AWS Transit Gateway | Lab: VPC Peering vs. Transit Gateway. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/TN32W85EET/comparing-amazon-virtual-private-cloud-vpc-peering-and-aws-transit-gateway/PW3BB44R1V?parentId=ZDTDSRZ9NE) |
| 26 | - | Configuring and Deploying Amazon VPC for a 3-tier Web App | Lab: VPC para uma aplicação web de 3 camadas. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/S1R2EF3PAH/configuring-and-deploying-amazon-vpc-for-a-3tier-web-app/V1R25V1GBK?parentId=ZDTDSRZ9NE) |
| 27 | - | Networking Core Knowledge Badge Assessment | Avaliação final para conquistar o badge. | _pendente_ | [Abrir curso](https://skillbuilder.aws/learn/YWS1RMVJ99/networking-core-knowledge-badge-assessment/X2AZTSKCTQ?parentId=ZDTDSRZ9NE) |

> ℹ️ Ainda não tenho o link do repositório para os certificados - vou preenchendo essa coluna conforme completo cada dia.

---

## 🏆 Meta

Completar os 27 treinamentos e passar no assessment com nota ≥ 80% para conquistar o **Networking Core Knowledge Badge**.

---

<div align="center">

Se gostou, não esqueça de deixar uma ⭐

<img width="200" alt="Image" src="https://github.com/user-attachments/assets/aca57b06-3ea1-49e4-96fb-b2a00b8f8918" />

</div>
