# AWS SimuLearn - Highly Available Web Applications
### Guia completo para iniciantes (Overview, Contexto, Prática e DIY)

---

## 📘 Overview (Visão Geral)

### Cenário de negócio simulado

Uma **agência de viagens** quer garantir que sua **aplicação web** continue disponível para os clientes, mesmo que **alguns componentes do sistema falhem**.

### O que é o AWS SimuLearn?

Neste assignment do AWS SimuLearn, você passa por duas etapas principais:

1. **Etapa de design (conversa)** - você revisa um cenário real e ajuda um cliente fictício a desenhar uma solução na AWS, conversando com um "cliente" gerado por IA.
2. **Etapa prática (lab)** - depois que o design está pronto, você constrói a solução proposta seguindo um passo a passo estruturado, dentro de um ambiente real do AWS Management Console.

Assim você ganha experiência prática com serviços da AWS, desenvolvendo competências reais usadas por profissionais de nuvem no dia a dia.

### Como funciona

- O SimuLearn usa **IA generativa** para desenvolver suas *soft skills*, como comunicação e resolução de problemas, em conversas realistas com clientes gerados por IA.
- Um **agente de quiz (IA)** avalia suas respostas durante a conversa.
- Um **agente assistente, o Dr. Newton**, está disponível para ajudar quando você tiver dúvidas.
- Depois da conversa, você constrói e valida a solução em um **ambiente real do Console AWS**, praticando com aplicação prática para o mercado de trabalho.

---

## 🧩 Contexto: Highly Available Web Applications

### Descrição do problema

A agência de viagens quer garantir que sua aplicação web continue disponível mesmo se **partes do sistema falharem** (um servidor cair,
uma Zona de Disponibilidade inteira ficar fora do ar, um pico de tráfego sobrecarregar os servidores, etc.).
O conceito principal trabalhado é **Alta disponibilidade**, ou seja, o sistema deve continuar funcionando memso que uma parte falhar.
Outro conceito importante é o **Auto-Healing**, a capacidade de um sistema detectar sozinho quando algo quebrou e se consertar automaticamente,
sem uma pessoa precisar intervir. Detalhado mais abaixo.

### Anotações técnicas da solução

1. Criar um **Auto Scaling group** distribuído em **múltiplas AZs** (Availability Zones).
2. Criar um **load balancer** (balanceador de carga).
3. Conectar (attach) o Auto Scaling group ao load balancer.
4. Criar um **health check** do load balancer para as instâncias EC2.

### A conversa com o cliente (Owen Stack)

> **Owen Stack:** Oi! Obrigado por responder nosso pedido de ajuda. Nossa agência de viagens acabou de ter um problema desastroso.
>
> **Você:** Poxa, sinto muito. O que aconteceu?
>
> **Owen:** O evento promocional que fizemos semana passada aumentou nosso tráfego web e sobrecarregou nossa aplicação. Todas as requisições estavam dando timeout.
>
> **Owen:** E piorou: nossas instâncias Amazon EC2 também estavam hospedadas em uma Availability Zone que foi afetada por tempestades fortes. Ficamos fora do ar por horas.
>
> **Você:** Sinto muito que isso tenha acontecido. Vamos falar sobre como evitar isso no futuro.
>
> **Você:** Primeiro, você pode usar a AWS para criar um **Auto Scaling group** distribuído em **múltiplas Availability Zones**, para que seus recursos não fiquem isolados em um único lugar.
>
> **Você:** Segundo, você pode criar um **load balancer** para que o tráfego de rede seja distribuído igualmente entre seus servidores.
>
> **Owen:** Entendi. Então, se eu espalhar as instâncias em várias AZs, isso vai me deixar completamente seguro contra falhas?
>
> **Você:** Bem, nenhum sistema é 100% à prova de falhas, mas as Availability Zones ficam em locais geograficamente distintos, e é improvável que um evento único afete várias AZs ao mesmo tempo.
>
> **Owen:** Entendi. Mas como podemos verificar se nossa aplicação está fora do ar em uma Availability Zone diferente?
>
> **Você:** Depois de espalhar suas instâncias EC2 em várias AZs, você pode conectar um load balancer ao seu Auto Scaling group para ter alta disponibilidade e elasticidade.
>
> **Você:** Você também pode monitorar a saúde das instâncias EC2 configurando um **health check** no seu load balancer.
>
> **Owen:** Isso parece ótimo! Você pode construir esse sistema pra gente? Nosso site precisa aguentar a demanda maior.

---

### 💡 Explicando os conceitos principais (antes de começar)

Esses são os conceitos centrais desse assignment. Vale a pena ler com calma antes de seguir para os passos.

| Conceito | O que significa (explicação simples) |
|---|---|
| **Availability Zone (AZ) / Zona de Disponibilidade** | Um data center (ou grupo de data centers) fisicamente separado dentro de uma Região da AWS, com energia, refrigeração e rede independentes. Cada Região tem várias AZs. Se uma AZ sofre um problema físico (como uma tempestade, incêndio, falha de energia), as outras AZs continuam funcionando normalmente, pois estão em locais geograficamente distintos. |
| **Alta disponibilidade (High Availability)** | Estratégia de projetar um sistema para que ele continue funcionando mesmo se uma parte falhar - por exemplo, distribuindo servidores em várias AZs e usando um load balancer para redirecionar o tráfego automaticamente para as instâncias saudáveis. |
| **Redundância** | Ter mais de um recurso fazendo a mesma função, para que, se um falhar, o outro assuma. Exemplos: várias instâncias EC2 rodando a mesma aplicação; um banco de dados RDS com uma réplica de standby. Redundância é a base técnica que torna a alta disponibilidade possível. |
| **Failover** | O processo automático de "trocar" para um recurso redundante quando o recurso principal falha. Por exemplo, se a instância principal de um banco de dados RDS falha, o RDS pode fazer failover automático para uma instância standby, minimizando o tempo de indisponibilidade. |
| **Health Check (verificação de saúde)** | Uma checagem periódica e automática que testa se uma instância (ou aplicação) está respondendo corretamente. Se uma instância falha no health check, ela é considerada "não saudável" (unhealthy), e o load balancer para de enviar tráfego para ela / o Auto Scaling group pode substituí-la. |
| **CloudWatch (Amazon CloudWatch)** | Serviço de monitoramento da AWS. Ele coleta métricas (como uso de CPU) e pode disparar **alarmes** quando essas métricas ultrapassam um limite definido. Esses alarmes podem, por exemplo, avisar o Auto Scaling group para adicionar mais instâncias. |

**Auto-Healing (auto-recuperação)**

Esse é um conceito importante que aparece na prática deste lab, mesmo sem estar no título do assignment
(ele é o tema central de um outro assignment do SimuLearn, o "[Auto-Healing and Scaling Applications](https://skillbuilder.aws/learn/ND78HW21FD/aws-simulearn-autohealing-and-scaling-applications/1C6SZU2GKX?parentId=1UQVR262ZB)").

O que é: a capacidade de um sistema detectar sozinho quando algo quebrou e se consertar automaticamente, sem uma pessoa precisar intervir.

Como funciona com o EC2 Auto Scaling group:

Você define uma capacidade mínima de instâncias que o grupo deve manter (por exemplo, 2 instâncias).
O Auto Scaling group fica monitorando a saúde dessas instâncias o tempo todo (via EC2 status checks ou health checks do load balancer).
Se uma instância falha, trava ou é encerrada por qualquer motivo, o grupo percebe que está abaixo da capacidade mínima configurada.
Automaticamente, ele lança uma nova instância para substituir a que quebrou, usando o launch template/configuração original - garantindo
que a nova instância seja idêntica à antiga.
Tudo isso acontece sem você precisar fazer nada-- é a "cura automática" do sistema.

Onde você veremos isso na prática: nos Passos 42-43 deste guia, você encerrou manualmente a instância do Auto Scaling group, 
e ele percebeu que ficou abaixo da capacidade mínima e criou uma nova instância sozinho, na mesma sub-rede - isso é o auto-healing
acontecendo de verdade, ao vivo.

Auto-healing x Alta disponibilidade: eles trabalham juntos, mas não são a mesma coisa. Auto-healing garante que instâncias 
quebradas sejam substituídas. Alta disponibilidade garante que, mesmo assim, o serviço continue no ar durante essa substituição
(por isso a importância de várias AZs e do load balancer redirecionando o tráfego só para as instâncias saudáveis, enquanto a nova sobe).


#### Load Balancer, Elastic Load Balancing (ELB) e Application Load Balancer (ALB)

| Conceito | Explicação |
|---|---|
| **Load Balancer (balanceador de carga)** | Um serviço que fica na frente dos seus servidores e distribui as requisições dos clientes entre várias instâncias, para que nenhum servidor fique sobrecarregado. Ele é o **único ponto de contato** para os clientes: eles enviam requisições ao load balancer, e ele decide para qual instância (target) encaminhar cada uma. |
| **Elastic Load Balancing (ELB)** | É o **serviço da AWS** que oferece balanceamento de carga. "ELB" é o nome genérico da família de serviços; dentro dele existem **tipos diferentes** de load balancer, cada um pensado para um tipo de tráfego. |
| **Application Load Balancer (ALB)** | Um dos tipos de load balancer dentro do ELB. Ele opera na **Camada 7 (camada de aplicação)** do modelo OSI, ou seja, entende o conteúdo das requisições **HTTP/HTTPS** (URLs, headers, cookies etc.) e pode tomar decisões de roteamento baseadas nesse conteúdo. É o tipo recomendado quando você precisa de **gerenciamento flexível de aplicações web**. |
| **Network Load Balancer (NLB)** *(para contexto, não é usado neste lab)* | Outro tipo de ELB, opera na **Camada 4 (camada de transporte)**, lidando com tráfego TCP/UDP de forma extremamente rápida, sem olhar o conteúdo da requisição. Usado quando performance/latência é mais crítica que roteamento inteligente. |
| **Gateway Load Balancer (GWLB)** *(para contexto)* | Também um tipo de ELB, usado para distribuir tráfego entre appliances de rede virtuais (firewalls, sistemas de detecção de intrusão), operando na Camada 3 (rede). |

**Resumindo a hierarquia:** *Elastic Load Balancing (ELB)* é o serviço/família da AWS → dentro dele você escolhe o **tipo** de load balancer → neste lab usamos o **Application Load Balancer (ALB)**, que atua na camada 7 (HTTP/HTTPS), ideal para aplicações web.

#### DNS, Route 53 e CloudFront

| Conceito | Explicação |
|---|---|
| **DNS (Domain Name System)** | O "sistema de nomes" da internet. Ele traduz nomes de domínio legíveis por humanos (como `exemplo.com`) para endereços IP, que é como os computadores realmente se encontram na rede. |
| **Amazon Route 53** | O serviço de **DNS gerenciado** da AWS. Ele é responsável por resolver o nome de domínio da sua aplicação (ex: `agenciadeviagens.com`) para o endereço IP correto - por exemplo, o IP de uma distribuição CloudFront ou de um load balancer. Neste cenário, o Route 53 é o primeiro passo do fluxo: quando alguém digita o endereço do site, o Route 53 traduz esse nome para um IP e direciona a requisição para o próximo componente (o CloudFront). |
| **Amazon CloudFront** | Uma **rede de entrega de conteúdo (CDN - Content Delivery Network)**. Depois que o Route 53 resolve o IP, a requisição do usuário se conecta a uma distribuição CloudFront, que reduz a latência fazendo **cache** de conteúdo (como imagens e vídeos) em servidores espalhados pelo mundo, mais perto do usuário, em vez de buscar tudo direto do servidor de origem toda vez. |
| **Amazon S3** | Serviço de armazenamento de objetos da AWS. Neste cenário, o S3 guarda os **arquivos estáticos** (imagens, vídeos) que o CloudFront faz cache e distribui aos usuários. |
| **Amazon RDS** | Serviço de banco de dados relacional gerenciado da AWS. Os servidores de aplicação se conectam a uma instância RDS, que oferece um ambiente de banco de dados gerenciado (a AWS cuida de backups, patches, etc.) e pode ter uma instância de **failover** para redundância caso a instância principal falhe. |

---

### Passo a passo da solução proposta (etapa de design)

**Objetivo:** Implantar a aplicação web em várias Availability Zones, com um Application Load Balancer e health checks, para garantir alta disponibilidade.

1. Essa solução ajuda a fornecer alta disponibilidade para a aplicação web através da infraestrutura de computação em nuvem da AWS.
2. O **Amazon Route 53** gerencia os serviços de **DNS**, traduzindo nomes de domínio (como `exemplo.com`) em endereços IP.
3. As requisições dos usuários se conectam a uma distribuição do **Amazon CloudFront** através do IP resolvido, reduzindo a latência por meio de cache de conteúdo e otimização de entrega.
4. O **Amazon S3** armazena os ativos estáticos (imagens e vídeos) para o CloudFront fazer cache e distribuição.
5. O **Elastic Load Balancing (ELB)** distribui o tráfego da aplicação entre várias Availability Zones, enquanto um **EC2 Auto Scaling group** gerencia a capacidade de instâncias conforme a demanda.
6. Os servidores de aplicação se conectam a uma instância do **Amazon RDS**, fornecendo um ambiente de banco de dados gerenciado que suporta múltiplos bancos de dados e ferramentas de acesso padrão.
7. O **auto scaling** ajusta o número de instâncias EC2 de acordo com políticas de escalonamento definidas.
8. Os grupos do **EC2 Auto Scaling** se integram ao **CloudWatch** para monitoramento de métricas e ao **ELB** para distribuição dinâmica de carga.
9. Quando o uso de CPU ultrapassa 80% por um período determinado, o sistema automaticamente lança servidores web adicionais e os adiciona ao load balancer. **Instâncias de failover do RDS** fornecem redundância do banco de dados durante falhas da instância principal.


### Arquitetura
<img width="1200" height="683" alt="Image" src="https://github.com/user-attachments/assets/eb4255f1-5ec6-4e28-aed2-6f97cebb2561" />



---

## 🧪 Prática (Practice Lab)

### Objetivo da prática

Neste laboratório prático, você vai:
- Configurar um Auto Scaling group para usar um **Application Load Balancer**.
- Configurar **health checks** do load balancer para o Auto Scaling group.
- Adicionar uma **segunda Availability Zone** ao Auto Scaling group.

### Metas do laboratório prático
- Configurar um Auto Scaling group para usar um Application Load Balancer.
- Configurar health checks do load balancer para o Auto Scaling group.
- Adicionar uma segunda Availability Zone ao Auto Scaling group.

> ⚠️ **Nota:** Neste ambiente de laboratório, apenas os serviços da AWS necessários para o exercício estão habilitados. Além disso, as funcionalidades desses serviços são limitadas ao que o lab exige.

---

### 📍 Passo a passo completo (55 passos)

**Passo 1 - Introdução**
1. Revise as tarefas do lab prático na seção Concept.
2. Clique em **Start Lab** ou **Open AWS Console** para começar.
3. Siga as instruções cuidadosamente, usando as setas para navegar entre os passos.

> 💡 **Conceito:** O **Amazon EC2** fornece servidores virtuais para hospedar aplicações web. Você pode lançar e gerenciar esses servidores para desenvolver e implantar aplicações mais rápido, sem investimento inicial em hardware.

**Passo 2 - Acessando o console EC2**
1. Na barra de navegação superior, na caixa de busca, digite: `ec2`
2. Nos resultados, em Services, clique em **EC2**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Os grupos do **Amazon EC2 Auto Scaling** ajustam automaticamente sua capacidade de instâncias EC2 com base em condições definidas. Isso ajuda a manter o desempenho da aplicação durante picos de demanda, reduzindo custos quando a demanda é baixa.

**Passo 3 - Revisando o Auto Scaling group existente**
1. No painel esquerdo, clique em **Auto Scaling Groups**.
2. Na seção Auto Scaling groups, marque a caixa para selecionar **TravelAgencyWebServers**.
3. Abaixo dessa seção, na aba **Details**, revise os detalhes de capacidade atual.
4. Vá para o próximo passo.

> 💡 **Conceito:** Ao especificar sub-redes de uma ou mais Availability Zones (AZs), você determina onde seu Auto Scaling group pode implantar instâncias EC2.

**Passo 4 - Revisando a configuração de rede**
1. Na seção Network, revise e observe que o Auto Scaling group está configurado com **apenas uma sub-rede em uma única Availability Zone**.
   - Distribuir instâncias em várias Availability Zones ajuda a manter a disponibilidade da aplicação, equilibrando instâncias de substituição entre essas zonas quando ocorrem falhas.
2. Vá para o próximo passo.

> 💡 **Conceito:** Um Auto Scaling group inicialmente lança o número de instâncias que você especifica como capacidade desejada (desired capacity). O grupo então mantém essa contagem de instâncias por meio de health checks periódicos.

**Passo 5 - Revisando as instâncias do grupo**
1. Role para cima até a barra de abas e clique na aba **Instance management**.
2. Revise e observe que o Auto Scaling group contém uma instância EC2.
3. Vá para o próximo passo.

> 💡 **Conceito:** **Target groups** (grupos de destino) roteiam requisições para targets registrados individualmente, como instâncias EC2, usando o protocolo e a porta que você especificar.

**Passo 6 - Criando um target group**
1. No painel esquerdo, clique em **Balanceamento de Carga (Load Balancing) > Target Groups** (Grupos de destino).
2. Clique em **Create target group**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Cada target group é usado para rotear requisições para um ou mais targets registrados.

**Passo 7 - Configurando o tipo de target**
1. Na seção Settings, escolha **Instances**.
2. Role para baixo até **Target group name**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Por padrão, um load balancer roteia requisições para seus targets usando o protocolo e a porta que você especificou ao criar o target group.

**Passo 8 - Nomeando o target group**
1. Em Target group name, digite: `TravelAgencyWebServer-TG`
2. Role para baixo até a seção VPC.
3. Vá para o próximo passo.

> 💡 **Conceito:** O load balancer começa a rotear tráfego para um target recém-registrado assim que o processo de registro é concluído e o target passa no primeiro health check inicial.

**Passo 9 - Selecionando a VPC**
1. Em VPC, escolha **(lab/TravelAgencyVpc)**.
2. Role até o final da página.
3. Clique em **Next**, depois clique em **Next** novamente.
4. Vá para o próximo passo.

> 💡 **Conceito:** Se a demanda da sua aplicação diminuir, ou você precisar dar manutenção nos seus targets, você pode **desregistrar (deregister)** targets dos seus target groups.

**Passo 10 - Criando o target group**
1. Role até o final da página.
2. Clique em **Create target group**.
3. Vá para o próximo passo.

**Passo 11 - Acessando Load Balancers**
1. Revise a criação bem-sucedida do target group.
2. No painel esquerdo, clique em **Load Balancers**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Um **load balancer** serve como o único ponto de contato para os clientes. Os clientes enviam requisições ao load balancer, e ele as envia para os targets, como instâncias EC2.

**Passo 12 - Criando o load balancer**
1. No painel esquerdo, clique em **Balanceamento de Carga (Load Balancing) > Load Balancers**.
2. Clique em **Create load balancer**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Você pode escolher o tipo de load balancer adequado com base nas necessidades da sua aplicação. Se você precisa de gerenciamento flexível de aplicações, recomenda-se usar um **Application Load Balancer** (atua na Camada 7, entendendo tráfego HTTP/HTTPS).

**Passo 13 - Escolhendo o tipo Application Load Balancer**
1. Em Load balancer types, em **Application Load Balancer**, clique em **Create**.
2. Vá para o próximo passo.

> 💡 **Conceito:** Os componentes de um Application Load Balancer são: o load balancer em si, os **listeners** (que determinam o roteamento) e os **target groups** (que enviam para os targets registrados).

**Passo 14 - Nomeando o load balancer e definindo o esquema**
1. Em Load balancer name, digite: `TravelAgencyServers-ALB`
2. Em Scheme, selecione **Internet-facing** (voltado para a internet).
3. Role para baixo até VPC.
4. Vá para o próximo passo.

> 💡 **Conceito:** Com um load balancer **internet-facing** (voltado para a internet), apenas VPCs com um **internet gateway** ficam disponíveis para seleção.

**Passo 15 - Configurando VPC e sub-redes**
1. Em VPC, escolha **lab/TravelAgencyVpc**.
2. Em Availability Zones and subnets, escolha **us-east-1a**.
3. Para Subnet for us-east-1a, escolha a **sub-rede pública**.
4. Repita os passos 2 e 3 para **us-east-1b** e **us-east-1c**.
   - Certifique-se de escolher a sub-rede pública para cada Availability Zone.
5. Vá para o próximo passo.

**Passo 16 - Configurando o security group do load balancer**
1. Em Security groups, remova **default** e escolha **TravelAgencyWebServer**.
2. Vá para o próximo passo.

> 💡 **Conceito:** Um **listener** verifica requisições de conexão dos clientes, usando o protocolo e a porta que você configurar. As regras que você define para um listener determinam como o load balancer roteia requisições para seus targets registrados.

**Passo 17 - Configurando o listener**
1. Em **Default action for Listeners and routing** (Listeners e roteamento - ação de roteamento), escolha **Forward to target groups** (Encaminhar aos grupos de destino).
2. Em Target group, escolha **TravelAgencyWebServer-TG**.
3. Role para baixo até **Summary** (Resumo).
4. Vá para o próximo passo.

**Passo 18 - Revisando a configuração**
1. Revise o resumo da configuração do load balancer.
2. Vá para o próximo passo.

**Passo 19 - Criando o load balancer**
1. Clique em **Create load balancer**.
2. Vá para o próximo passo.

**Passo 20 - Voltando ao Auto Scaling group**
1. No painel esquerdo, clique em **Auto Scaling Groups**.
2. Escolha **TravelAgencyWebServers**.
3. Vá para o próximo passo.

**Passo 21 - Editando integrações de load balancing**
1. Clique na aba **Integrations**.
2. Na seção Load balancing, clique em **Edit**.
3. Vá para o próximo passo.

**Passo 22 - Conectando o target group ao Auto Scaling group**
1. Em Load balancing, escolha **Application, Network or Gateway Load Balancer target groups** (Grupos de destino de Application, Network ou Gateway Load Balancer).
2. Em Target groups, escolha **TravelAgencyWebServer-TG**.
3. Clique em **Update**.
4. Vá para o próximo passo.

> 💡 **Conceito:** Crie security groups para controlar o fluxo de tráfego, definindo: o tráfego permitido para o load balancer, e o tráfego permitido do load balancer para os servidores web.

**Passo 23 - Criando um security group para o load balancer**
1. No painel esquerdo, clique em **Security Groups**.
2. Na seção Security Groups, clique em **Create security group**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Security groups pertencem a uma **VPC (Virtual Private Cloud)** e só podem ser atribuídos a recursos dentro dessa VPC.

**Passo 24 - Configurando o novo security group**
1. Em Basic details, em Security group name, digite: `TravelAgencyLoadBalancer`
2. Em Description, digite uma descrição, como *Allow access to the travel agency load balancer from the internet*.
3. Em VPC, escolha o nome da VPC que termina com `lab/TravelAgencyVpc`.
4. Na seção Inbound rules, clique em **Add rule**.
5. Vá para o próximo passo.

> 💡 **Conceito:** Para um load balancer voltado ao público: especifique `0.0.0.0/0` como origem para aceitar tráfego de qualquer endereço; defina um security group como destino de saída para restringir o tráfego apenas às instâncias associadas a esse grupo.

**Passo 25 - Configurando a regra de entrada (inbound)**
1. Em Type, escolha **HTTP**.
2. Para permitir todo o tráfego de entrada, em Source, na caixa de busca Custom, escolha **0.0.0.0/0**.
3. Vá para o próximo passo.

**Passo 26 - Configurando a regra de saída (outbound) e criando o security group**
1. Na seção Outbound rules, em Type, escolha **HTTP**.
2. Em Destination, ao lado de Custom, escolha o security group **TravelAgencyWebServer**.
3. Para remover o destino `0.0.0.0/0`, clique no X.
4. Clique em **Create security group**.
5. Vá para o próximo passo.

> 💡 **Conceito:** Para aumentar a segurança, você pode editar o security group da instância EC2 para permitir apenas tráfego de entrada vindo do Application Load Balancer.

**Passo 27 - Editando o security group das instâncias**
1. No painel esquerdo, clique em **Security Groups**.
2. Na seção Security Groups, marque a caixa para selecionar o security group **TravelAgencyWebServer**.
3. No menu suspenso Actions, escolha **Edit inbound rules**.
4. Vá para o próximo passo.

> 💡 **Conceito:** Substitua a origem `0.0.0.0/0` por um security group para: controlar o tráfego para as instâncias sem especificar faixas de endereço, e permitir tráfego apenas das instâncias associadas a esse security group.

**Passo 28 - Removendo a regra antiga**
1. Na seção Inbound rules, para remover a regra existente, clique em **Delete**.
   - Aqui você está removendo a regra que **acabou de ser criada** no security group TravelAgencyWebServer (a que permitia HTTP de `0.0.0.0/0`), porque no próximo passo ela será substituída por uma regra mais restritiva, liberando o tráfego **apenas** vindo do security group do load balancer (TravelAgencyLoadBalancer).
   - Para modificar o tipo de uma regra, você precisa excluir a regra existente.
2. Para adicionar uma nova regra, clique em **Add rule**.
3. Vá para o próximo passo.

**Passo 29 - Criando a nova regra restrita ao load balancer**
1. Em Type, escolha **HTTP**.
2. Em Source, ao lado de Custom, escolha o security group **TravelAgencyLoadBalancer**.
3. Clique em **Save rules**.
4. Vá para o próximo passo.

> 💡 **Conceito:** Security groups ficam ativos quando atribuídos a uma instância. Cada instância pode ter múltiplos security groups.

**Passo 30 - Acessando o load balancer criado**
1. No painel esquerdo, clique em **Load Balancers**.
2. Na seção Load balancers, clique em **TravelAgencyServers-ALB** (esse é o nome correto do load balancer que você criou no Passo 14 - não confunda com o nome do target group).
3. Vá para o próximo passo.

**Passo 31 - Editando a segurança do load balancer**
1. Clique na aba **Security**.
2. Clique em **Edit**.
3. Vá para o próximo passo.

**Passo 32 - Removendo o security group antigo**
1. Em Security groups, para remover o security group TravelAgencyWebServer, clique no X.
2. Vá para o próximo passo.

**Passo 33 - Aplicando o security group correto**
1. Escolha o security group **TravelAgencyLoadBalancer**.
2. Clique em **Save changes**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Você pode testar sua aplicação copiando o **DNS name** do load balancer e colando em um navegador. Isso oferece acesso imediato para visualizar sua aplicação.

**Passo 34 - Copiando o DNS name do load balancer**
1. No alerta de sucesso, revise a mensagem.
2. No Application Load Balancer, em DNS name, clique no ícone de cópia para copiar o nome fornecido.
3. Vá para o próximo passo.

**Passo 35 - Testando a aplicação pelo navegador**
1. Em uma nova aba (ou janela) do navegador, cole o DNS name copiado na barra de endereço, adicione `http://` no início e pressione Enter.
   - Se aparecer um erro, verifique se a URL começa com `http://` (não `https://`). A página está hospedada apenas em HTTP.
   - Se você não ver sua aplicação, verifique as regras no security group associado ao load balancer.
2. Revise a página HTML carregada.
   - A aplicação web da agência de viagens agora está atrás de um Application Load Balancer.
3. Vá para o próximo passo.

> 💡 **Conceito:** O health check padrão do load balancer valida apenas o caminho raiz (`/`) do servidor HTTP. Para uma validação mais robusta: implemente health checks de aplicação que verifiquem a configuração do servidor e o acesso externo; monitore o load balancer para confirmar que os health checks estão ativos.

**Passo 36 - Testando o endpoint de health check**
1. Na mesma aba do navegador, no final do endereço que você editou, digite: `/health` e pressione Enter.
2. Revise a mensagem curta que carrega, informando que a instância está saudável (healthy).
   - Mantenha essa aba aberta para health checks posteriores.
3. Vá para o próximo passo.

> 💡 **Conceito:** Você pode modificar as configurações de health check do load balancer para alinhá-las aos seus requisitos de desempenho.

**Passo 37 - Acessando as configurações de health check do target group**
1. Volte para o console Amazon EC2 na outra aba do navegador.
2. No painel esquerdo, clique em **Target Groups**.
3. Na seção Target groups, marque a caixa para selecionar **TravelAgencyServers-ALB** (esse é o nome correto - não confunda com "TravelAgencyWebServers-1").
4. Abaixo dessa seção, clique na aba **Health checks** (Verificações de integridade).
5. Na seção Health check settings, clique em **Edit**.
6. Vá para o próximo passo.

> 💡 **Conceito:** Um target registrado precisa passar em um health check inicial para ser considerado saudável. O load balancer fecha a conexão do health check depois de completar cada verificação.

**Passo 38 - Configurando o caminho do health check**
1. Em Health check path, digite: `/health`
2. Clique para expandir **Advanced health check settings**.
3. Vá para o próximo passo.

**Passo 39 - Ajustando os parâmetros do health check**
1. Em **Unhealthy threshold** (Limite não íntegro), digite: `2`
2. Em **Timeout** (Tempo limite), digite: `2`
3. Em Interval, digite: `5`
4. Clique em **Save changes**.
   - As configurações padrão marcam instâncias como não saudáveis (unhealthy) depois de 150 segundos (5 checagens em intervalos de 30 segundos). As novas configurações marcam instâncias como unhealthy depois de apenas 10 segundos (2 checagens em intervalos de 5 segundos).
5. Vá para o próximo passo.

> 💡 **Conceito:** Depois de criar um load balancer voltado para a internet, você pode rodar suas aplicações em uma sub-rede privada. Você pode adicionar ou remover sub-redes associadas ao Auto Scaling group conforme necessário.

**Passo 40 - Editando a rede do Auto Scaling group**
1. No painel esquerdo, clique em **Auto Scaling Groups**.
2. Na seção Auto Scaling groups, marque a caixa para selecionar **TravelAgencyWebServers**.
   - Se o grupo já estiver selecionado e a janela abaixo estiver vazia, desmarque e marque novamente.
3. Abaixo dessa seção, na aba Details, na seção Network, clique em **Edit**.
4. Vá para o próximo passo.

> 💡 **Conceito:** Adicionar ou remover sub-redes define onde os recursos do Auto Scaling group podem residir.

**Passo 41 - Confirmando a sub-rede atual**
1. Em Availability Zones and subnets, mantenha ou escolha a sub-rede **lab/TravelAgencyVpc/PublicSubnet1**.
   - Se qualquer outra sub-rede estiver selecionada, clique no X para removê-la.
2. Clique em **Update**.
3. Vá para o próximo passo.

**Passo 42 - Encerrando a instância atual (para testar a auto-recuperação)**
1. No painel esquerdo, clique em **Instances**.
2. Na seção Instances, marque a caixa para selecionar a instância TravelAgencyWebServers.
3. Em Instance ID, revise o ID.
4. Abaixo dessa seção, clique na aba **Networking**.
5. Em Subnet ID, revise e confirme que a instância está na sub-rede lab/TravelAgencyVpc/PublicSubnet1.
6. Na seção Instances, no menu suspenso **Instance state** (Estado da instância), escolha **Terminate/delete instance** (Encerrar/excluir instância).

> 💡 **Conceito:** Quando o encerramento de uma instância reduz o Auto Scaling group abaixo do seu tamanho mínimo, uma **nova instância é lançada automaticamente** - isso é o auto-healing em ação.

**Passo 43 - Verificando a nova instância criada automaticamente**
1. Depois de alguns minutos, na seção Instances, clique no ícone de atualizar.
   - A lista de instâncias agora deve incluir a instância recém-criada.
2. Marque a caixa para selecionar a nova instância.
3. Em Instance ID, revise o novo ID.
4. Abaixo dessa seção, clique na aba Networking.
5. Em Subnet ID, revise e confirme que a nova instância está na sub-rede lab/TravelAgencyVpc/PublicSubnet1.
6. Vá para o próximo passo.

**Passo 44 - Revisando o histórico de atividades**
1. No painel esquerdo, clique em **Auto Scaling Groups**.
2. Na seção Auto Scaling groups, marque a caixa para selecionar TravelAgencyWebServers.
   - Se o grupo já estiver selecionado e a janela abaixo estiver vazia, desmarque e marque novamente.
3. Abaixo dessa seção, clique na aba **Activity**.
4. Vá para o próximo passo.

> 💡 **Conceito:** A seção Activity history exibe os eventos de escalonamento, com descrições, horários de início e fim, códigos de status e causas.

**Passo 45 - Confirmando a substituição da instância**
1. Na seção Activity history, revise e confirme que a instância antiga foi encerrada.
2. Revise e confirme que o Auto Scaling group respondeu criando uma nova instância.
3. Vá para o próximo passo.

**Passo 46 - Testando o health check com a nova instância**
1. Volte para a aba do navegador com a página de health check.
2. Clique no ícone de atualizar do navegador.
3. Revise a mensagem.
   - Se aparecer um erro, espere um pouco mais e atualize novamente.
   - Na mensagem, observe que o valor de instance ID mudou para a nova instância.
4. Vá para o próximo passo.

> 💡 **Conceito:** Você pode distribuir seu Auto Scaling group em múltiplas Availability Zones de uma Região para obter **redundância geográfica**. Adicione um load balancer para distribuir o tráfego de entrada entre essas zonas.

**Passo 47 - Editando a rede novamente para adicionar uma segunda AZ**
1. Na outra aba do navegador, volte para a página de Auto Scaling groups no console Amazon EC2.
2. Revise e confirme que o grupo TravelAgencyWebServers ainda está selecionado.
3. Abaixo dessa seção, na aba Details, na seção Network, clique em **Edit**.

> 💡 **Conceito:** O EC2 Auto Scaling lança instâncias em zonas diferentes com base na sua estratégia de distribuição: **"Balanced best effort"** lança nas zonas saudáveis; **"Balanced only"** continua lançando na zona original, mesmo que não esteja saudável.

**Passo 48 - Adicionando a segunda Availability Zone**
1. Em Availability Zones and subnets, escolha a sub-rede **lab/TravelAgencyVpc/PublicSubnet2**.
   - Tanto lab/TravelAgencyVpc/PublicSubnet1 quanto lab/TravelAgencyVpc/PublicSubnet2 devem estar selecionadas agora.
2. Clique em **Update**.
3. Vá para o próximo passo.

> 💡 **Conceito:** A capacidade de auto scaling ajusta automaticamente os recursos computacionais dentro dos limites definidos.

**Passo 49 - Editando a capacidade do grupo**
1. No alerta de sucesso, revise a mensagem.
2. Na aba Details, na seção **TravelAgencyWebServers Capacity overview** (Visão geral da capacidade), clique em **Edit**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Você pode mudar a capacidade desejada manualmente para testar o comportamento do Auto Scaling group. Aumentar a capacidade desejada lança novas instâncias até atingir o valor especificado, sem ultrapassar a capacidade máxima.

**Passo 50 - Aumentando a capacidade desejada**
1. Na janela pop-up, em Desired capacity, digite: `2`
2. Revise que Max desired capacity foi atualizado para 2.
3. Clique em **Update**.
4. Vá para o próximo passo.

**Passo 51 - Verificando a nova atividade**
1. No alerta de sucesso, revise a mensagem.
2. Clique na aba **Activity**.
3. Vá para o próximo passo.

> 💡 **Conceito:** O load balancer usa **connection draining** para completar requisições em andamento (in-flight) antes de desregistrar instâncias ou remover instâncias não saudáveis.

**Passo 52 - Confirmando o lançamento da nova instância**
1. Revise e confirme que uma nova instância está sendo lançada.
   - Anote o ID da nova instância.
2. Vá para o próximo passo.

> 💡 **Conceito:** Você pode verificar o Subnet ID para confirmar que o auto scaling lançou a instância na sub-rede correta.

**Passo 53 - Confirmando a sub-rede da nova instância**
1. No painel esquerdo, clique em **Instances**.
2. Na seção Instances, marque a caixa para selecionar a instância com o novo ID do passo anterior.
3. Abaixo dessa seção, clique na aba Networking.
4. Em Subnet ID, revise e confirme que a nova instância está na sub-rede lab/TravelAgencyVpc/PublicSubnet2.
5. Vá para o próximo passo.

**Passo 54 - Confirmando com o health check**
1. Volte para a aba do navegador com a página de health check.
2. Clique no ícone de atualizar do navegador.
3. Revise a mensagem.
   - Talvez seja necessário clicar no ícone de atualizar algumas vezes.
   - Na mensagem, observe que o instance ID mudou para a instância mais nova.
4. Vá para o próximo passo.

**Passo 55 - Conclusão da prática**
🎉 Parabéns! Você concluiu a seção Practice. Agora vá para a seção **DIY** para completar a solução.

---

## 🛠️ DIY (Faça Você Mesmo)

### Metas do DIY

1. Configurar o Auto Scaling group existente para incluir uma nova instância EC2 em uma **terceira Availability Zone**.

### Método de validação da solução

O servidor de teste vai verificar se você implantou um **Application Load Balancer** com **no mínimo três instâncias EC2** distribuídas em **três Availability Zones**.

> 💡 **Dica oficial do lab:** depois de adicionar a terceira Availability Zone, você também precisa **aumentar a capacidade desejada (desired capacity)** - só adicionar a AZ não cria a instância sozinha, é preciso pedir para o Auto Scaling group efetivamente lançar mais uma instância.

### 💡 Explicando o que você precisa fazer

Lembra da Prática: você já tinha um Auto Scaling group (`TravelAgencyWebServers`) rodando em **duas** Availability Zones (`PublicSubnet1` e `PublicSubnet2`), com capacidade desejada = 2. Agora o objetivo é repetir exatamente o padrão que você já praticou nos Passos 40-51, só que adicionando **mais uma AZ/sub-rede** e **mais uma instância**:

- Hoje: 2 AZs, 2 instâncias.
- Meta do DIY: 3 AZs, no mínimo 3 instâncias.

Isso é feito em duas partes: (1) editar a rede do Auto Scaling group para incluir a terceira sub-rede pública, e (2) editar a capacidade do grupo para que uma terceira instância seja realmente lançada.

### Passo a passo sugerido para o DIY

1. No console AWS, acesse o serviço **EC2**.
2. No painel esquerdo, clique em **Auto Scaling Groups**.
3. Marque a caixa para selecionar **TravelAgencyWebServers**.
4. Na aba **Details**, na seção **Network**, clique em **Edit**.
5. Em **Availability Zones and subnets**, mantenha as sub-redes já selecionadas (`PublicSubnet1` e `PublicSubnet2`) e **adicione a terceira sub-rede pública** - a que corresponde à **us-east-1c** (a mesma Availability Zone que você já havia configurado no load balancer lá no Passo 15 da Prática, mas que ainda não tinha sido adicionada ao Auto Scaling group).
6. Clique em **Update**.

> 💡 **Conceito:** Só adicionar a sub-rede/AZ à configuração de rede do Auto Scaling group **não cria uma instância nova automaticamente**. Isso apenas informa ao grupo "você **pode** lançar instâncias aqui também". Para que uma instância realmente apareça nessa nova AZ, é preciso aumentar a **capacidade desejada (desired capacity)** - foi exatamente isso que a dica do lab avisou.

7. Volte para a aba **Details**.
8. Na seção **Capacity overview** (Visão geral da capacidade), clique em **Edit**.
9. Em **Desired capacity**, digite: `3`
   - Confira se **Max desired capacity** é igual ou maior que 3 (se necessário, aumente o máximo também, senão o Auto Scaling group não vai conseguir lançar a terceira instância).
10. Clique em **Update**.
11. Aguarde alguns instantes e clique na aba **Activity** para confirmar que uma nova instância está sendo lançada.
12. No painel esquerdo, clique em **Instances**, encontre a nova instância, clique na aba **Networking** e confirme que o **Subnet ID** corresponde à terceira sub-rede pública (a nova AZ que você acabou de adicionar).
13. Para confirmar que tudo está funcionando de ponta a ponta, volte para a aba do navegador com a página `/health` (a mesma usada na Prática), atualize algumas vezes e observe que o **instance ID** alterna entre as três instâncias saudáveis, distribuídas nas três AZs, por trás do Application Load Balancer.

### Validação

<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/ef040867-4e7f-4792-8b02-cafb4209ddc4" />

### ⚠️ Pontos de atenção (erros comuns)

- **Esquecer de aumentar a desired capacity**: se você só adicionar a terceira AZ/sub-rede e não aumentar a capacidade desejada para 3, o Auto Scaling group continua com apenas 2 instâncias, e a validação vai falhar.
- **Max capacity menor que a desired capacity**: a capacidade desejada nunca pode ultrapassar a capacidade máxima do grupo. Se o máximo estiver configurado como 2, aumente-o antes de tentar colocar desired capacity = 3.
- **Escolher a sub-rede errada**: certifique-se de escolher a sub-rede **pública** da terceira Availability Zone (a mesma AZ que já está configurada no load balancer desde a Prática), para manter a consistência com o Application Load Balancer.
- **Load balancer não é o alvo da mudança**: neste DIY você mexe na rede e na capacidade do **Auto Scaling group**, não no load balancer em si - o Application Load Balancer (`TravelAgencyServers-ALB`) já foi configurado com as três AZs lá na Prática (Passo 15), então ele já está pronto para receber tráfego da nova instância assim que ela for registrada no target group.

---

## ✅ Resumo dos conceitos aprendidos

| Termo | Explicação resumida |
|---|---|
| **Availability Zone (AZ)** | Data center fisicamente isolado dentro de uma Região da AWS; base da alta disponibilidade. |
| **Alta disponibilidade** | Sistema projetado para continuar funcionando mesmo com falhas parciais. |
| **Redundância** | Ter mais de um recurso cumprindo a mesma função, como backup. |
| **Failover** | Troca automática para um recurso redundante quando o principal falha. |
| **Health Check** | Verificação periódica que testa se uma instância está respondendo corretamente. |
| **Load Balancer** | Distribui requisições dos clientes entre várias instâncias. |
| **Elastic Load Balancing (ELB)** | Serviço da AWS que engloba os diferentes tipos de load balancer (ALB, NLB, GWLB). |
| **Application Load Balancer (ALB)** | Tipo de ELB que opera na Camada 7 (HTTP/HTTPS), ideal para aplicações web. |
| **Target Group** | Define para quais instâncias (targets) o load balancer envia o tráfego, e com qual protocolo/porta. |
| **Amazon Route 53** | Serviço de DNS gerenciado da AWS; traduz nomes de domínio em endereços IP. |
| **Amazon CloudFront** | CDN da AWS; faz cache de conteúdo perto do usuário para reduzir latência. |
| **Amazon S3** | Armazenamento de objetos; guarda arquivos estáticos como imagens/vídeos. |
| **Amazon RDS** | Banco de dados relacional gerenciado; pode ter failover automático. |
| **Amazon CloudWatch** | Monitora métricas (como uso de CPU) e dispara alarmes/ações automáticas. |
| **Auto Scaling Group (ASG)** | Gerencia automaticamente o número de instâncias EC2, mantendo capacidade e substituindo instâncias com falha. |
| **Security Group** | Conjunto de regras de firewall aplicadas a recursos dentro de uma VPC. |

---
