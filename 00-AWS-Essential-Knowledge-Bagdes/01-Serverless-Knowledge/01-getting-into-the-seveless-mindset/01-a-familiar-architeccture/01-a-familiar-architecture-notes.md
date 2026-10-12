# 01 - A Familiar Architecture

Nessa primeiro aula (A Familiar Architecture), 
o Mike Deck apresenta o cenário tradicional de infraestrutura na nuvem 
e os problemas gerados por ele para, no final, introduzir a razão de existir (o porquê) 
do Serverless.

Em resumo, ele apresenta a evolução de uma arquitetura tradicional em 5 etapas:

1.A Aplicação Inicial (1 EC2 + Banco de Dados): O ponto de partida mais
simples para rodar uma aplicação HTTP.

2.Escalabilidade Vertical e seus Limites: O problema de tentar resolver 
o aumento de tráfego apenas aumentando o tamanho daquela única máquina EC2.

3.Escalabilidade Horizontal (Load Balancer + Várias EC2s): A solução de colocar 
um balanceador de carga para distribuir tráfego entre múltiplos servidores e o 
surgimento do problema de gestão de custos (pagar por máquinas paradas fora do horário de pico).

4.Elasticidade e Alta Disponibilidade (Auto Scaling + Zonas de Disponibilidade): A adição
do Auto Scaling para ligar/desligar instâncias conforme o tráfego e a distribuição 
em múltiplas AZs para suportar falhas em data centers.

5.O Problema Final: "Undifferentiated Heavy Lifting" (Trabalho Pesado Não Diferenciado): 
Ele mostra que, mesmo essa arquitetura sendo excelente e usada por milhares de empresas, 
ela exige que você perca tempo gerenciando infraestrutura (provisionamento, saúde dos servidores, 
métricas de escala, patches).

## Cenário

Um serviço web muito simples, no qual os clientes se comunicam com o seu serviço via HTTP.
A arquitetura é bem simples, implantamos uma aplicação web em um único servidor, como uma instância do Amazon EC2.
Configuramos um de banco de dados para persistir os dados.

<p align="center">
  <img width="600" height="350" alt="Image" src="https://github.com/user-attachments/assets/9ee92526-b728-4da1-8e3d-fa899bef374e" />
</p>

## Mas o que acontece quando você começa a receber mais usuários e mais tráfego?

## Solução 1: Scaling UP (Escalar verticalmente)

Aumentar o tamanho do mesmo servidor (trocar uma EC2 com 2 CPUs e 4 GB de RAM por uma com 16 CPUs e 64 GB de RAM).

<p align="center">
  <img width="600" height="350" alt="Image" src="https://github.com/user-attachments/assets/cb4693e9-fe85-4ab7-864d-59e826f0e3b3" />
</p>

**Quais são os problemas de só "Escalar Up" a instância EC2?**

1. Tempo de Inatividade (Downtime) Obrigatório
Para aumentar a capacidade de uma máquina EC2 (trocar o tipo da instância no console da AWS),
você precisa desligar o servidor, alterar o tipo e ligá-lo novamente.

- Resultado: No momento exato em que seu site mais precisa estar no ar (pico de clientes querendo comprar),
o seu site fica fora do ar por alguns minutos para fazer essa troca.

2. Existe um Limite Físico (Teto de Hardware)
Não existe uma máquina infinitamente grande. As maiores instâncias do mercado
têm limites (ex: 128 vCPUs, 4TB de RAM).
Se o seu tráfego explodir além desse teto, você simplesmente não consegue mais
fazer *Scale UP*. Não existe servidor maior para comprar.

3. Ponto Único de Falha (Single Point of Failure)
Se você tiver apenas 1 servidor EC2 gigante rodando tudo:

- Se a fonte, o processador virtual, a rede do data center ou o
sistema operacional dessa máquina falharem/travarem, a sua aplicação inteira cai 100%.

- Não há redundância. Se a máquina morrer, todos os seus usuários perdem o acesso instantaneamente.

4. Custo Ineficiente e Desperdício
Se o seu pico de tráfego acontece das 14h às 18h de segunda a sexta,
para aguentar esse pico você terá que contratar um servidor gigante.

- Nas madrugadas e fins de semana, quando ninguém usa o sistema,
você continuará pagando caro por aquele "supercomputador" parado.

**Escalonamento Vertical Agendado: Para evitar o desperdício, consigo Schedular o escalonamento?**

Sim, você até consegue **agendar a troca do tamanho do servidor**,
mas isso continua trazendo problemas graves.

Essa técnica se chama **Escalonamento Vertical Agendado**
(modificar o tipo da instância EC2 via script ou automação da AWS em horários fixos).

Apesar de ser teoricamente possível, veja por que essa abordagem quase nunca 
é usada em produção:

1. O site AINDA vai cair todos os dias (Downtime Forçado)
Para mudar o tamanho de uma máquina EC2 (por exemplo,
mudar de uma t3.medium para uma t3.2xlarge), a AWS exige que a máquina esteja desligada.

Isso significa que:

- Às 13:55 (antes do pico): Seu script vai desligar o servidor.
O site cai por 3 a 5 minutos enquanto o tipo de instância é alterado
e a máquina inicializa.

- Às 18:05 (após o pico): O script desliga a máquina de novo para diminuir o tamanho.
O site cai por mais 3 a 5 minutos.

Seu público/clientes teriam dois momentos de **indisponibilidade garantidos** todos os dias úteis.

2. O tráfego do mundo real não segue horários perfeitos
3. 
E se um post do seu produto viralizar às 21h de uma quarta-feira?
Ou se você fizer uma campanha de marketing que traga milhares de acessos num domingo à tarde?

- Com 1 servidor agendado: A máquina estará "pequena" no horário agendado.
O servidor vai travar, estourar a memória/CPU e o site vai cair,
porque o agendamento não reage ao tráfego de verdade, ele só **reage** ao relógio.

- Com Auto Scaling + Load Balancer: Solução que vamos estudar a seguir, as métricas
de CPU e tráfego são monitoradas em tempo real. Se o tráfego subir de repente num domingo,
novos servidores são criados automaticamente e em segundos, sem derrubar o site.

3. Sessões de usuários e conexões abertas são derrubadas
Quando você reinicia um servidor único para mudar o tamanho dele,
todas as conexões HTTP ativas naquele segundo são **cortadas abruptamente**:

- Usuários no meio de um pagamento/checkout perdem a transação.

- Uploads de arquivos são interrompidos.

- Pessoas usando o sistema veem uma tela de erro (502 Bad Gateway ou 504 Gateway Timeout).

Em resumo:
Funciona usar só 1 EC2 e ir aumentando a capacidade?
Sim, para projetos pequenos, ambientes de teste ou 
fases iniciais de um produto, funciona perfeitamente e é super simples.

No entanto, para sistemas de produção que não podem cair e têm tráfego imprevisível,
depender de apenas um servidor é considerado um risco alto de arquitetura,
por isso vamos estudar outras soluções.

## Solução 2: Load balance + Auto Sacling

O número de usuários aumentou, o tráfego aumentou.

<p align="center">
  <img width="600" height="350" alt="Image" src="https://github.com/user-attachments/assets/05c9d5b7-80ad-4968-88b4-804033f5d6af"  />
</p>

Para resolver isso, você pode dimensionar a aplicação horizontalmente 
colocando um **balanceador de carga** (load balancer) na frente de múltiplas instâncias.

E isso é ótimo, agora você pode adicionar **mais servidores**
facilmente para lidar com a carga que precisar, mas surge a **questão da gestão de custos**.
A maioria das aplicações não tem uma carga perfeitamente constante o tempo todo.

Por exemplo, você pode receber mais tráfego no meio do dia durante a semana do que na madrugada
de um fim de semana e, com a arquitetura atual, você precisa provisionar infraestrutura para
o seu pico de carga e continuar pagando por esses servidores mesmo quando eles estiverem ociosos.

**O que o Load Balancer (Balanceador de Carga) faz?**
A função principal do Load Balancer é atuar como um "guarda de trânsito" ou uma "recepção".

Quando um usuário digita o endereço do seu site (HTTP/HTTPS),
a requisição não vai direto para o servidor EC2. Ela chega primeiro no Load Balancer.
O Load Balancer então:

- Recebe as requisições de todos os clientes.

- Distribui essas requisições entre os vários servidores
EC2 que estão atrás dele (ex: manda 1º acesso pro Servidor A, 2º pro Servidor B, 3º pro Servidor C).

- Checa a saúde dos servidores (Health Checks): Se o Servidor B quebrar ou travar,
o Load Balancer percebe e para de mandar tráfego para ele,
direcionando tudo para os servidores que estão funcionando.

Lembre-se:
- Cada servidor EC2 tem o seu próprio endereço IP individual.
- Se você tiver 10 servidores EC2, você não vai dar 10 endereços IP diferentes para os seus clientes escolherem.
- Com o Load Balancer, os seus clientes acessam um único ponto de entrada (um único nome de domínio/IP do Load Balancer).
- À medida que você adiciona mais servidores EC2 para aguentar o aumento de usuários (escalonamento horizontal), você só precisa "conectar" esses novos servidores ao Load Balancer. Ele passa a distribuir o tráfego entre todos eles automaticamente.

**Dá para usar Load Balancer com apenas 1 servidor EC2? Ou só se usa quando tem mais servidores?**

Sim, dá para usar (e é muito comum usar) com apenas 1 servidor EC2!

Embora o nome seja "balanceador" (o que sugere dividir entre vários), 
você pode sim colocar o Load Balancer na frente de um único servidor.

Aqui estão os motivos de por que alguém faz isso:

- Alta Disponibilidade e Migração Transparente:
Se o seu único servidor EC2 falhar e você precisar criar outro,
ou se você quiser trocar esse servidor por um mais potente,
o endereço (URL/IP) do Load Balancer continua o mesmo.
O usuário nem percebe a troca.

- Gerenciamento de Certificados SSL/TLS (HTTPS):
O Load Balancer pode cuidar da criptografia (o "cadeado verde" HTTPS).
O servidor EC2 não precisa gastar processamento decifrando o tráfego HTTPS;
o Load Balancer decifram e entrega o tráfego limpo para o EC2.

- Preparação para o futuro:
Se o seu site crescer de repente, você já tem a estrutura pronta para simplesmente
ligar um 2º ou 3º servidor EC2 atrás do Load Balancer sem precisar alterar as
configurações de DNS do seu domínio.

Resumo prático

- 1 Servidor sozinho: O cliente fala direto com o IP daquela máquina. Se ela cair ou lotar, o site sai do ar.

- 1 Servidor + Load Balancer: O cliente fala com o Load Balancer, que repassa para o único servidor. Traz facilidade de segurança e manutenção.

- Múltiplos Servidores + Load Balancer: O cliente fala com o Load Balancer, e ele divide o tráfego entre todos os servidores disponíveis. É assim que grandes sistemas suportam milhões de acessos simultâneos.

**O que mais o Load Balancer garante?**

Além de ajudar no balanceamento, ele entrega outras capacidades essenciais:

- Roteamento Inteligente: Direciona o tráfego com base na URL ou no cabeçalho
  (exemplo: /api vai para o grupo de servidores de backend, enquanto / vai para os servidores web).

- Descarregamento de Criptografia (SSL/TLS Termination): O Load Balancer gerencia os certificados
de segurança HTTPS. Isso tira a carga computacional de criptografar e descriptografar o
tráfego dos servidores EC2.

- Absorção de Surtos de Tráfego: Ele aguenta picos repentinos de acessos antes mesmo de repassar
as requisições para as suas instâncias EC2, protegendo a sua rede interna.

- Proteção de Segurança Básica: Esconde os IP privados das suas instâncias EC2
para que elas não fiquem expostas diretamente à internet pública.

**Auto Scaling: provisionar e encerrar instâncias automaticamente**

Mas, em um ambiente de nuvem, esse problema é bem fácil de resolver.
Você pode simplesmente adicionar um grupo de **Auto Scaling** para **provisionar**
e **encerrar** instâncias automaticamente com base nos padrões de tráfego da sua aplicação.

> Assim, você consegue lidar com todo o tráfego nos momentos de alta carga
> e minimizar seus custos quando não houver tanto movimento.

Agora, além de dimensionar sua aplicação de forma elástica,
você provavelmente também quer garantir que ela seja **altamente disponível e tolerante a falhas**.

**Zon de disponibilidade:Altamente disponível e toelarante a falhas**

Você quer ter a certeza de que suas instâncias estão distribuídas
por múltiplas Zonas de Disponibilidade (Availability Zones)
para garantir que, mesmo se houver uma falha em um único data center,
seus usuários ainda consigam acessar o serviço.

Isso também é fácil de implementar. Basta garantir que o balanceador
de carga e o grupo de auto scaling estejam configurados para **distribuir**
suas instâncias entre as **Zonas de Disponibilidade**.

Com isso, você tem uma arquitetura altamente disponível
que escala elasticamente para suportar o pico de tráfego,
**economizando custos nos horários de menor movimento**.

Esta é uma arquitetura muito, muito comum, implantada com sucesso milhares de vezes por diversos clientes.

**O problema dessa solução:**

No entanto, há várias coisas com as quais você,
como arquiteto de aplicações ou desenvolvedor,
precisa se preocupar ao construir e operar um sistema como esse.

Primeiro, há o seu próprio código e toda a lógica personalizada que é única para o seu negócio ou produto.

Depois, há toda uma lista de outras considerações sobre como gerenciar
a infraestrutura: como provisionar instâncias, como garantir que os 
servidores estejam saudáveis, qual métrica utilizar para determinar 
o momento de dimensionar para mais ou para menos (scale out / scale in)?

Conceitos chaves:

- Escalar para cima/baixo (Escalabilidade Vertical ou Scale Up / Scale Down):
Aumentar o tamanho do mesmo servidor (trocar uma EC2 com 2 CPUs e 4 GB de RAM por uma com 16 CPUs e 64 GB de RAM).

- Escalar para os lados (Escalabilidade Horizontal ou Scale Out / Scale In):
Adicionar mais servidores do mesmo tamanho (em vez de 1 servidor, colocar 5 servidores iguais).


## Solução 3: Architectura Serveless

A grande sacada do modelo Serverless é justamente
NÃO precisar usar nem Load Balancer nem Auto Scaling manuais,
porque a própria AWS assume esse trabalho por você.
