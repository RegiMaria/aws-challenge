# AWS SimuLearn - Connecting VPCs
### Guia completo para iniciantes (Overview, Contexto, Prática e DIY)

---

## 📘 Overview (Visão Geral)

### Cenário de negócio simulado

O **time de Marketing** quer manter **ambientes de rede separados** para cada departamento (Marketing, Finanças e Desenvolvimento), mas ainda assim precisa que esses ambientes consigam **se comunicar entre si** quando necessário.

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

### Objetivos do assignment

- Analisar como o **VPC peering** funciona dentro da Amazon VPC.
- Planejar os passos de implementação de conexões de peering.
- Configurar uma conexão de peering entre duas VPCs.
- Implementar configurações de VPC peering específicas por sub-rede.

### Serviços AWS usados

- **Amazon Elastic Compute Cloud (EC2)**
- **Amazon VPC**

---

## 🧩 Contexto: Connecting VPCs

### Descrição do problema

O time de Marketing quer manter **ambientes de rede separados** para cada departamento, mas ainda assim garantir que consigam **se comunicar entre si** quando necessário.

O conceito central deste lab é o **VPC Peering (emparelhamento de VPCs)**: uma forma de conectar duas VPCs isoladas para que os recursos dentro delas possam se enxergar e trocar tráfego usando **endereços IP privados**, como se estivessem na mesma rede - **sem** precisar de internet gateway, VPN, hardware físico, ou expor nada publicamente.

### Anotações técnicas da solução

1. Criar uma conexão de peering para **cada par de VPCs** que precisa se comunicar.
2. **Aceitar** a solicitação de peering de cada VPC.
3. **Atualizar as route tables** (tabelas de rotas) para que o tráfego entre as VPCs pareadas seja direcionado corretamente.

---

### 💡 Explicando os conceitos principais (antes de começar)

Esses são os conceitos centrais desse assignment. Vale a pena ler com calma antes de seguir para os passos.

| Conceito | O que significa (explicação simples) |
|---|---|
| **VPC (Virtual Private Cloud)** | Uma rede virtual isolada e privada dentro da AWS, que você define e controla completamente (faixa de IPs, sub-redes, tabelas de rotas, gateways, etc.). Por padrão, uma VPC não enxerga outra VPC - elas são isoladas entre si. |
| **VPC Peering (emparelhamento de VPCs)** | Uma conexão de rede entre duas VPCs que permite que o tráfego flua entre elas usando **endereços IP privados**, como se fizessem parte da mesma rede. O tráfego passa pela rede interna da própria AWS, nunca pela internet pública. |
| **CIDR (faixa de IP da VPC)** | O intervalo de endereços IP que pertence a uma VPC (ex: `10.10.0.0/16`). Para duas VPCs conseguirem fazer peering, **suas faixas de CIDR não podem se sobrepor** (precisam ser únicas). |
| **Requester (Solicitante)** | A VPC que **inicia** o pedido de peering. É quem "convida" a outra VPC para se conectar. |
| **Accepter (Aceitante)** | A VPC que **recebe** o pedido de peering e precisa **aceitá-lo** explicitamente para a conexão ficar ativa. Pode até ser uma VPC de outra conta AWS. |
| **Route Table (tabela de rotas)** | Um conjunto de regras associado a uma sub-rede que diz "para chegar a tal destino, envie o tráfego por tal caminho (target)". Sem uma rota apontando para a conexão de peering, o tráfego não sabe como chegar até a outra VPC, mesmo com o peering já ativo. |
| **Security Group** | Um firewall virtual, aplicado a nível de instância, que controla qual tráfego de entrada/saída é permitido. É **stateful**: se o tráfego de entrada é permitido, a resposta de saída é automaticamente liberada, mesmo sem uma regra explícita de saída. |
| **Transitividade (ausência dela no peering)** | O VPC Peering **não é transitivo**. Ou seja: se A está pareada com B, e B está pareada com C, isso **não** significa que A consegue falar com C automaticamente através de B. Cada par de VPCs que precisa se comunicar exige sua **própria** conexão de peering direta. |

---

### 🔗 Como o peering realmente funciona neste cenário (quem conecta com quem, quem autoriza, quem aceita, e por quê)

Esse é o ponto que costuma gerar mais dúvida, então vamos destrinchar com calma.

**As três VPCs do cenário:**

- **Marketing VPC** - CIDR `10.10.0.0/16`
- **Finance VPC** - CIDR `172.31.0.0/16` (hospeda o servidor `FinanceServer`, que é o recurso que todo mundo precisa acessar)
- **Developer VPC** - hospeda instâncias do time de desenvolvimento

**Por que a Finance VPC é o "centro" do cenário?**
Porque ela hospeda o recurso que os outros dois times precisam acessar (o servidor financeiro). Marketing e Developer não precisam conversar entre si - só precisam, cada um, acessar a Finance.

**Passo a passo de "quem faz o quê" em uma conexão de peering:**

1. **Quem inicia (Requester):** uma das duas VPCs cria a solicitação de peering, informando o ID da própria VPC (Requester) e o ID da VPC com a qual quer se conectar (Accepter). Neste lab, por exemplo, a **Marketing VPC** é a Requester e a **Finance VPC** é a Accepter na primeira conexão.
2. **Quem autoriza/aceita (Accepter):** a conexão nasce com o status **"Pending Acceptance"** (pendente de aceitação). Ela só fica **"Active"** depois que alguém com permissão sobre a VPC Accepter (neste caso, a **Finance VPC**) entra e clica em **"Accept request"**. Se as VPCs estiverem na mesma conta AWS (como neste lab), você mesmo aceita dos dois lados. Se fossem contas diferentes, o dono da conta da VPC Accepter precisaria aceitar.
3. **A conexão em si não basta:** mesmo depois de "Active", o peering sozinho **não redireciona tráfego nenhum**. Ele só cria o "túnel" possível entre as duas redes.
4. **As route tables (tabelas de rotas) precisam ser atualizadas dos dois lados:**
   - Na VPC **Marketing**, é preciso adicionar uma rota dizendo: "para chegar à faixa `172.31.0.0/16` (Finance), use a conexão de peering".
   - Na VPC **Finance**, é preciso adicionar a rota **inversa**, dizendo: "para chegar à faixa `10.10.0.0/16` (Marketing), use essa mesma conexão de peering".
   - Sem as **duas** rotas (uma em cada VPC), o tráfego só consegue ir em um sentido, ou não vai em sentido nenhum.
5. **Os Security Groups ainda filtram o tráfego:** mesmo com o peering ativo e as rotas configuradas, o **security group da instância de destino** (por exemplo, o do `FinanceServer`) precisa ter uma regra de entrada permitindo explicitamente o tráfego vindo da faixa de IP da outra VPC (ex: permitir ICMP vindo de `10.10.0.0/16`). Sem essa regra, o pacote chega até a rede certa, mas é bloqueado na porta da instância.

**Resumindo o fluxo completo para o par Marketing ↔ Finance:**

```
Marketing (Requester) ──cria peering──▶ Finance (Accepter)
Finance ──aceita a solicitação (Accept request)──▶ status muda para Active
Marketing: adiciona rota 172.31.0.0/16 → Peering Connection
Finance: adiciona rota 10.10.0.0/16 → Peering Connection
Finance: libera no Security Group o tráfego vindo de 10.10.0.0/16
✅ Agora o MarketingServer consegue "pingar" o FinanceServer
```

**E o segundo par, Developer ↔ Finance, segue exatamente a mesma lógica:**

```
Developer (Requester) ──cria peering──▶ Finance (Accepter)
Finance ──aceita a solicitação──▶ status muda para Active
Developer: adiciona rota <CIDR da Finance> → Peering Connection
Finance: adiciona rota <CIDR da Developer> → Peering Connection
Finance: libera no Security Group o tráfego vindo do CIDR da Developer VPC
✅ Agora a instância Developer consegue acessar o FinanceServer
```

**Por que Marketing e Developer NÃO se comunicam entre si:**
Porque não existe (e não precisa existir) uma conexão de peering direta entre elas, e mesmo que ambas estejam pareadas com a Finance, o **VPC Peering não é transitivo** - ou seja, a Finance não "repassa" o tráfego de uma para a outra. Cada comunicação exige seu próprio par de peering + rotas + regras de security group.

---

### Arquitetura conceitual

```
        ┌───────────────┐                          ┌───────────────┐
        │  Marketing VPC│◀──── Peering nº 1 ─-──▶ │   Finance VPC │
        │ 10.10.0.0/16  │                          │ 172.31.0.0/16 │
        └───────────────┘                          │(FinanceServer)│
                                                   └───────────────┘
                                                            ▲
                                                            │
                                                      Peering nº 2
                                                            │
                                                            ▼
                                                    ┌────────────────┐
                                                    │  Developer VPC │
                                                    │ (CIDR próprio) │
                                                    └────────────────┘

  ❌ Marketing e Developer NÃO têm peering direto e NÃO se comunicam
     (peering não é transitivo)
```

---

## 🧪 Prática (Practice Lab)

### Objetivo da prática

Neste laboratório prático, você vai:
- Configurar uma **conexão de VPC peering** (entre Marketing e Finance).
- Garantir que o **tráfego seja roteado corretamente** entre as VPCs pareadas.

### Metas do laboratório prático
- Configurar uma conexão de VPC peering.
- Garantir que o tráfego seja roteado corretamente entre as VPCs pareadas.

> ⚠️ **Nota:** Neste ambiente de laboratório, apenas os serviços da AWS necessários para o exercício estão habilitados. Além disso, as funcionalidades desses serviços são limitadas ao que o lab exige.

---

### 📍 Passo a passo completo (36 passos)

**Passo 1 - Introdução**
1. Revise as tarefas do lab prático na seção Concept.
2. Clique em **Start Lab** ou **Open AWS Console** para começar.
3. Siga as instruções cuidadosamente, usando as setas para navegar entre os passos.

> 💡 **Conceito:** A **Amazon Virtual Private Cloud (Amazon VPC)** é um serviço que ajuda você a lançar recursos da AWS em uma rede virtual logicamente isolada, que você mesmo define. Você tem controle total sobre o seu ambiente de rede virtual.

**Passo 2 - Acessando o console VPC**
1. Na barra de navegação superior, na caixa de busca, digite: `vpc`
2. Nos resultados, em Services, clique em **VPC**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Conexões de VPC peering habilitam roteamento de IP privado entre VPCs que, de outra forma, seriam isoladas entre si.

**Passo 3 - Revisando as VPCs existentes**
1. No painel esquerdo, clique em **Your VPCs**.
2. Na seção Your VPCs, revise as VPCs disponíveis: **Marketing**, **Finance** e **Developer**.
3. Vá para o próximo passo.

**Passo 4 - Acessando o console EC2**
1. Na barra de navegação superior, na caixa de busca, digite: `ec2`
2. Nos resultados, em Services, clique em **EC2**.
3. Vá para o próximo passo.

**Passo 5 - Acessando as instâncias**
1. No Dashboard, na seção Resources, clique em **Instances (running)**.
2. Vá para o próximo passo.

> 💡 **Conceito:** Uma **sub-rede (subnet)** é uma faixa de endereços IP dentro de uma VPC e precisa residir em uma única Availability Zone.

**Passo 6 - Revisando a instância FinanceServer**
1. Na seção Instances, marque a caixa para selecionar a instância **FinanceServer**.
2. Abaixo dessa seção, clique na aba **Networking**.
3. Revise e observe que **não há** Public IPv4 address nem Public IPv4 DNS preenchidos.
   - Uma instância criada em uma sub-rede privada não tem endereços públicos.
4. Em Private IPv4 addresses, clique no ícone de cópia para copiar o endereço IP fornecido, e cole em um editor de texto no seu dispositivo.
   - Você vai usar esse endereço nos próximos passos.
5. Em Subnet ID, revise o ID fornecido.
6. Vá para o próximo passo.

> 💡 **Conceito:** Você pode se conectar a uma instância Amazon EC2 de quatro formas: EC2 Instance Connect, Session Manager, cliente SSH, ou EC2 serial console.

**Passo 7 - Selecionando a instância MarketingServer**
1. Na seção Instances, desmarque a caixa da instância FinanceServer.
2. Marque a instância **MarketingServer**.
3. Abaixo dessa seção, na aba Networking, em VPC ID, revise a qual VPC a instância MarketingServer pertence.
4. No topo da seção Instances, clique em **Connect**.
5. Vá para o próximo passo.

> 💡 **Conceito:** O **Session Manager**, uma capacidade do AWS Systems Manager, gerencia instâncias EC2, dispositivos edge e servidores on-premises através de um shell baseado em navegador ou da AWS CLI.

**Passo 8 - Conectando via Session Manager**
1. Clique na aba **Session Manager**.
2. Clique em **Connect**.
   - O terminal do Session Manager para a instância MarketingServer abre em uma nova aba (ou janela) do navegador.
3. Vá para o próximo passo.

> 💡 **Conceito:** VPCs usam endereços IP privados e **não conseguem** se comunicar com outras VPCs por padrão.

**Passo 9 - Testando a conexão (ainda sem peering)**
1. Na janela do terminal do Session Manager, substituindo o endereço IP de exemplo pelo IP do FinanceServer que você copiou anteriormente, execute (digite o comando e pressione Enter):
   ```
   ping 172.31.x.xx
   ```
   - Esse comando testa a conexão com a instância FinanceServer.
2. Revise e observe que o comando **trava**, e não há conexão.
   - Não há mensagem de erro. Como a instância FinanceServer usa apenas um endereço IP privado, as VPCs não conseguem estabelecer caminhos de roteamento para destinos fora da própria faixa de IP privado local.
3. Para encerrar o processo em execução, pressione **Ctrl+C** no teclado.
4. Vá para o próximo passo.

> 💡 **Conceito:** Instâncias EC2 residem dentro de uma sub-rede em uma VPC.

**Passo 10 - Encontrando a sub-rede do MarketingServer**
1. Na outra aba do navegador, volte para a página Instances no console Amazon EC2.
2. Revise e confirme que a instância MarketingServer ainda está selecionada.
3. Na aba Networking, em Subnet ID, clique no ID de **MarketingPublicSubnet1**.
4. Vá para o próximo passo.

> 💡 **Conceito:** Uma **route table (tabela de rotas)** usa regras de roteamento para controlar o tráfego de uma sub-rede.

**Passo 11 - Localizando a route table da Marketing**
1. Na seção Subnets, escolha a sub-rede disponível.
2. Na aba Details, em Route table, clique no ID de rota fornecido.
   - O dashboard da VPC abre em uma nova aba (ou janela) do navegador.
3. Vá para o próximo passo.

> 💡 **Conceito:** Route tables com internet gateways incluem regras para tráfego IP local e público. Recomenda-se especificar blocos CIDR das faixas de IP privado da RFC 1918.

**Passo 12 - Revisando a route table atual**
1. Na seção Route tables, escolha a route table Marketing disponível.
2. Na aba Routes, revise as regras de roteamento.
   - Devem aparecer duas rotas: uma para tráfego local e uma para tráfego de internet através do internet gateway.
3. Vá para o próximo passo.

> 💡 **Conceito:** O VPC peering permite que instâncias se comuniquem como se compartilhassem a mesma rede.

**Passo 13 - Iniciando a criação da conexão de peering**
1. No painel esquerdo, clique em **Peering connections**.
2. Na seção Peering connections, clique em **Create peering connection**.
3. Vá para o próximo passo.

> 💡 **Conceito:** A VPC **Requester** (solicitante) inicia pedidos de permissão de acesso para outras VPCs, incluindo VPCs em contas diferentes da AWS.

**Passo 14 - Configurando a VPC Requester**
1. Na seção Peering connection settings, em Name, digite: `Marketing <> Finance`
2. Em **VPC ID (Requester)**, na lista suspensa, escolha a VPC **Marketing**.
3. Em VPC CIDRs, revise e confirme que a VPC Marketing tem `10.10.0.0/16` como sua faixa de CIDR.
4. Vá para o próximo passo.

> 💡 **Conceito:** VPCs pareadas precisam ter blocos CIDR únicos. Por exemplo, VPCs usando `192.168.0.0/28` não conseguem fazer peering entre si.

**Passo 15 - Configurando a VPC Accepter**
1. Em **VPC ID (Accepter)**, escolha a VPC **Finance**.
2. Em VPC CIDRs, revise e confirme que a VPC Finance tem `172.31.0.0/16` como sua faixa de CIDR.
3. Clique em **Create peering connection**.
4. Vá para o próximo passo.

> 💡 **Conceito:** Para configurar VPC peering:
> **Dentro da mesma conta:** forneça os IDs das duas VPCs → crie a solicitação de peering → aceite a solicitação.
> **Entre contas diferentes:** os dois donos de conta precisam aceitar a solicitação.

**Passo 16 - Aceitando a solicitação de peering**
1. No alerta de sucesso, revise a mensagem.
2. Na seção Details, em Status, revise e confirme que o status é **Pending Acceptance**.
3. Clique em **Actions** para expandir a lista suspensa.
4. Escolha **Accept request**.
5. Vá para o próximo passo.

**Passo 17 - Confirmando a aceitação**
1. Na janela pop-up, clique em **Accept request**.
2. Vá para o próximo passo.

**Passo 18 - Confirmando que a conexão está ativa**
1. No alerta de sucesso, revise a mensagem.
2. Na seção Details, em Status, revise e confirme que o status agora é **Active**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Depois de estabelecer uma conexão de VPC peering, é preciso adicionar rotas nas route tables de **ambas** as VPCs para habilitar o tráfego entre elas.

**Passo 19 - Voltando à instância MarketingServer**
1. Na outra aba do navegador, volte para a página Instances no console Amazon EC2.
2. Escolha a instância MarketingServer.
3. Na aba Details, em Subnet ID, clique no ID fornecido.
4. Vá para o próximo passo.

**Passo 20 - Acessando a route table da Marketing (novamente)**
1. Na seção Subnets, escolha a sub-rede disponível.
2. Na aba Details, em Route table, clique no ID fornecido.
   - O dashboard da VPC abre em uma nova aba (ou janela) do navegador.
3. Vá para o próximo passo.

> 💡 **Conceito:** O tráfego de VPC peering só flui **depois** que as rotas são configuradas nas route tables de **ambas** as VPCs.

**Passo 21 - Editando a route table da Marketing**
1. Na seção Route tables, escolha a route table Marketing.
2. Na aba Routes, clique em **Edit routes**.
3. Vá para o próximo passo.

**Passo 22 - Adicionando a rota para a Finance**
1. Clique em **Add route**.
2. Para configurar a rota, em Destination, na nova caixa de busca, digite: `172.31.0.0/16`
   - Esse bloco CIDR pertence à VPC Finance.
3. Em Target, escolha **Peering Connection**.
4. Logo abaixo, escolha o target da conexão de peering que contém **Marketing <> Finance**.
5. Clique em **Save changes**.
6. Vá para o próximo passo.

**Passo 23 - Confirmando a nova rota da Marketing**
1. No alerta de sucesso, revise a mensagem.
2. Na aba Routes, revise a rota recém-criada.
   - A nova rota direciona o tráfego originado na VPC Marketing (`10.10.0.0/16`) destinado à VPC Finance (`172.31.0.0/16`) através da conexão de peering.
3. Vá para o próximo passo.

**Passo 24 - Voltando à instância FinanceServer**
1. Na outra aba do navegador, volte para a página Instances no console Amazon EC2.
2. Escolha a instância FinanceServer.
   - Você pode precisar desmarcar a instância MarketingServer.
3. Na aba Details, em Subnet ID, clique no ID fornecido.
4. Vá para o próximo passo.

**Passo 25 - Acessando a route table da Finance**
1. Na seção Subnets, escolha a sub-rede disponível.
2. Na aba Details, em Route table, clique no ID de rota fornecido.
   - O dashboard da VPC abre em uma nova aba (ou janela) do navegador.
3. Vá para o próximo passo.

> 💡 **Conceito:** O VPC peering exige rotas nas tabelas de rotas de **ambas** as VPCs para funcionar.

**Passo 26 - Editando a route table da Finance**
1. Na seção Route tables, escolha a route table Finance.
2. Na aba Routes, clique em **Edit routes**.
3. Vá para o próximo passo.

**Passo 27 - Adicionando a rota para a Marketing**
1. Clique em **Add route**.
2. Para configurar a rota, em Destination, na nova caixa de busca, digite: `10.10.0.0/16`
   - Esse bloco CIDR pertence à VPC Marketing.
3. Em Target, escolha **Peering Connection**.
4. Logo abaixo, escolha o target da conexão de peering que contém **Marketing <> Finance**.
5. Clique em **Save changes**.
6. Vá para o próximo passo.

**Passo 28 - Confirmando a nova rota da Finance**
1. No alerta de sucesso, revise a mensagem.
2. Na aba Routes, revise a rota recém-criada.
   - A nova rota direciona o tráfego originado na VPC Finance (`172.31.0.0/16`) destinado à VPC Marketing (`10.10.0.0/16`) através da conexão de peering.
3. Vá para o próximo passo.

**Passo 29 - Voltando à instância MarketingServer**
1. Na outra aba do navegador, volte para a página Instances no console Amazon EC2.
2. Escolha a instância MarketingServer.
   - Você pode precisar desmarcar a instância FinanceServer. Para maior clareza, também pode fechar as outras abas abertas.
3. Clique em **Connect**.
4. Vá para o próximo passo.

**Passo 30 - Reconectando via Session Manager**
1. Clique na aba **Session Manager**.
2. Clique em **Connect**.
   - O terminal do Session Manager para a instância MarketingServer abre em uma nova aba (ou janela) do navegador.
3. Vá para o próximo passo.

> 💡 **Conceito:** Network ACLs e security groups continuam filtrando o tráfego entre VPCs pareadas, mesmo com o peering e as rotas já configurados.

**Passo 31 - Testando novamente a conexão (ainda bloqueada)**
1. Na janela do terminal do Session Manager, substituindo o endereço IP de exemplo pelo IP que você copiou anteriormente, execute:
   ```
   ping 172.31.x.xx
   ```
   - Esse é o endereço IPv4 privado da instância FinanceServer.
2. Revise e observe que o comando **ping ainda não funciona**.
3. Para encerrar o processo em execução, pressione **Ctrl+C** no teclado.
4. Vá para o próximo passo.

> 💡 **Conceito:** Security groups exigem regras explícitas para permitir tráfego de entrada vindo de VPCs pareadas.

**Passo 32 - Acessando o security group da FinanceServer**
1. Na outra aba do navegador, volte para a página Instances no console Amazon EC2.
2. Escolha a instância FinanceServer.
3. Clique na aba **Security**.
4. Em Security groups, clique no nome do security group fornecido que contém **FinanceServerSecurityGroup**.
5. Vá para o próximo passo.

> 💡 **Conceito:** Security groups são **stateful** (com estado), permitindo automaticamente o tráfego de resposta, independente das regras de entrada ou saída configuradas.

**Passo 33 - Editando as regras de entrada**
1. Na aba Inbound rules, clique em **Edit inbound rules**.
2. Vá para o próximo passo.

> 💡 **Conceito:** Security groups filtram tráfego por protocolo e porta, usando apenas regras de permissão (allow).

**Passo 34 - Liberando o tráfego ICMP vindo da Marketing**
1. Clique em **Add rule**.
2. Em Type, escolha **Custom ICMP - IPv4**.
3. Em Source, na nova caixa de busca, digite e escolha: `10.10.0.0/16`
4. Clique em **Save rules**.
5. Vá para o próximo passo.

> 💡 **Conceito:** Serviços se comunicam entre VPCs pareadas quando os security groups permitem o tráfego.

**Passo 35 - Confirmando a comunicação entre as VPCs**
1. Na outra aba do navegador, volte para o terminal do Session Manager.
   - Se a linha de comando do Session Manager tiver expirado, você precisa se conectar novamente à instância MarketingServer pela aba Session Manager.
2. Na janela do terminal, substituindo o endereço IP de exemplo pelo IP que você copiou anteriormente, execute:
   ```
   ping 172.31.x.xx
   ```
   - Esse é o endereço IPv4 privado da instância FinanceServer.
3. Revise os dados de resposta.
   - A instância MarketingServer agora deve conseguir se comunicar com a instância FinanceServer. 🎉
4. Para encerrar o processo em execução, pressione **Ctrl+C** no teclado.
5. Vá para o próximo passo.

**Passo 36 - Conclusão da prática**
🎉 Parabéns! Você concluiu a seção Practice. Agora vá para a seção **DIY** para completar a solução.

---

## 🛠️ DIY (Faça Você Mesmo)

### Metas do DIY

1. Configurar VPC peering entre as VPCs dos departamentos **Developer** e **Finance**.

### Método de validação da solução

O servidor de teste vai verificar se a **instância EC2 do Developer** consegue se comunicar com a **instância EC2 da Finance** através das VPCs.

### 💡 Explicando o que você precisa fazer

Na Prática, você conectou **Marketing ↔ Finance**. Agora o objetivo é repetir **exatamente o mesmo padrão**, só que para o par **Developer ↔ Finance**:

- Já feito na Prática: Marketing ↔ Finance (peering + rotas + security group).
- Meta do DIY: Developer ↔ Finance (peering + rotas + security group).

Lembre-se da lógica de "quem conecta com quem, quem autoriza, quem aceita":

1. Uma das VPCs (por exemplo, **Developer**) será a **Requester** e a **Finance** será a **Accepter**.
2. A solicitação nasce como **Pending Acceptance** e alguém precisa **aceitar** (Accept request) para ela virar **Active**.
3. Depois de ativa, é preciso adicionar rota na route table da **Developer** apontando para o CIDR da **Finance**, através da conexão de peering.
4. E também adicionar a rota **inversa** na route table da **Finance**, apontando para o CIDR da **Developer**, através da mesma conexão de peering.
5. Por fim, revisar o **security group da instância Finance** para garantir que ele permite tráfego de entrada vindo da faixa de IP da VPC **Developer** (assim como você fez para a Marketing).

> 💡 **Lembrete importante:** o VPC Peering **não é transitivo**. O fato de Marketing↔Finance e Developer↔Finance já existirem **não cria** comunicação direta entre Marketing e Developer - e isso está correto e esperado pelo cenário, já que Marketing e Developer não precisam conversar entre si.

### Passo a passo sugerido para o DIY

1. No console AWS, acesse o serviço **VPC**.
2. No painel esquerdo, clique em **Your VPCs** e anote (ou copie) o **CIDR da VPC Developer** - você vai precisar dele para configurar as rotas mais adiante.
3. No painel esquerdo, clique em **Peering connections**.
4. Clique em **Create peering connection**.
5. Em **Name**, digite algo como: `Developer <> Finance`
6. Em **VPC ID (Requester)**, escolha a VPC **Developer**.
7. Em **VPC ID (Accepter)**, escolha a VPC **Finance**.
8. Revise os CIDRs exibidos para confirmar que são diferentes entre si (sem sobreposição).
9. Clique em **Create peering connection**.
10. Na seção Details, em **Status**, confirme que está como **Pending Acceptance**.
11. Clique em **Actions > Accept request**, e depois confirme clicando em **Accept request** novamente na janela pop-up.
12. Confirme que o **Status** mudou para **Active**.

**Configurando as rotas:**

13. Vá até o console **EC2 > Instances**, selecione a instância do Developer (ex: `DeveloperServer` ou nome equivalente do lab), copie o **Private IPv4 address** dela (você vai usar para testar depois) e confira o **Subnet ID**.
14. Clique no Subnet ID, depois em **Route table**, para abrir a route table da VPC **Developer**.
15. Na aba **Routes**, clique em **Edit routes** > **Add route**.
16. Em **Destination**, digite o **CIDR da VPC Finance** (`172.31.0.0/16`, o mesmo usado na Prática).
17. Em **Target**, escolha **Peering Connection** e selecione a conexão **Developer <> Finance**.
18. Clique em **Save changes**.
19. Agora repita o processo do lado da **Finance**: acesse a instância **FinanceServer**, encontre sua Subnet ID e sua route table.
20. Na route table da Finance, clique em **Edit routes** > **Add route**.
21. Em **Destination**, digite o **CIDR da VPC Developer** (o que você anotou no passo 2).
22. Em **Target**, escolha **Peering Connection** e selecione novamente a conexão **Developer <> Finance**.
23. Clique em **Save changes**.

**Ajustando o Security Group:**

24. Volte para a instância **FinanceServer**, clique na aba **Security**, e abra o security group (**FinanceServerSecurityGroup**).
25. Na aba **Inbound rules**, clique em **Edit inbound rules** > **Add rule**.
26. Em **Type**, escolha **Custom ICMP - IPv4** (o mesmo tipo usado na Prática).
27. Em **Source**, digite o **CIDR da VPC Developer**.
28. Clique em **Save rules**.

**Testando a comunicação:**

29. Selecione a instância do **Developer**, clique em **Connect > Session Manager > Connect** para abrir um terminal.
30. No terminal, execute:
    ```
    ping <IP privado do FinanceServer>
    ```
31. Confirme que a resposta do ping funciona, indicando que a instância Developer agora consegue se comunicar com a instância Finance através da conexão de peering.
32. Pressione **Ctrl+C** para encerrar o comando.

### ⚠️ Pontos de atenção (erros comuns)

- **Esquecer de aceitar a solicitação de peering**: criar a conexão não é suficiente - ela precisa ser **aceita** (Accept request) para sair de "Pending Acceptance" e virar "Active".
- **Configurar rota em apenas uma VPC**: o tráfego só flui nos dois sentidos se **ambas** as route tables (Developer e Finance) tiverem a rota apontando uma para a outra através da conexão de peering. Rota em uma via só faz o tráfego ir em um sentido (ou não ir em nenhum).
- **Esquecer o Security Group**: mesmo com peering ativo e rotas corretas, se o security group da instância de destino (FinanceServer) não liberar explicitamente o tráfego vindo da faixa de IP da VPC de origem (Developer), a conexão continua bloqueada.
- **Confundir CIDR de origem com CIDR de destino**: ao criar as rotas, lembre-se: na route table da Developer, o **Destination** é o CIDR da **Finance**; na route table da Finance, o **Destination** é o CIDR da **Developer**. É sempre "para onde eu quero mandar o tráfego", não a minha própria faixa.
- **Esperar que Marketing e Developer se enxerguem**: como o peering não é transitivo, não crie (nem espere) uma conexão automática entre Marketing e Developer só porque ambas estão pareadas com a Finance - isso não existe e não é o objetivo deste DIY.

---

## Validação final
<img width="800" height="400" alt="Image" src="https://github.com/user-attachments/assets/190a4c8f-beea-4c91-b925-ae8607a23738" />


## ✅ Resumo dos conceitos aprendidos

| Termo | Explicação resumida |
|---|---|
| **VPC (Virtual Private Cloud)** | Rede virtual isolada e privada dentro da AWS, totalmente controlada por você. |
| **VPC Peering** | Conexão que permite tráfego IP privado entre duas VPCs, sem passar pela internet pública. |
| **CIDR** | Faixa de endereços IP de uma VPC; precisa ser única entre VPCs pareadas. |
| **Requester** | A VPC que inicia (solicita) a conexão de peering. |
| **Accepter** | A VPC que precisa aceitar a solicitação de peering para ela ficar ativa. |
| **Route Table** | Conjunto de regras que direciona o tráfego de uma sub-rede para o destino certo (target). |
| **Security Group** | Firewall a nível de instância; controla o tráfego de entrada/saída e é stateful. |
| **Transitividade (ausência no peering)** | Peering entre A-B e B-C não cria comunicação automática entre A e C. |
| **Session Manager** | Ferramenta do AWS Systems Manager para acessar instâncias EC2 via terminal no navegador, sem precisar de chave SSH ou IP público. |

---
