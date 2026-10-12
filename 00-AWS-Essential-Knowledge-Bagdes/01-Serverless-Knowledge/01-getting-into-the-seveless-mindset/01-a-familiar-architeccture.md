# 01 - A Familiar Architecture

Nessa primeira aula (A Familiar Architecture), 
o Mike Deck apresenta o cenário tradicional de infraestrutura na nuvem 
e os problemas gerados por ele para, no final, introduzir a razão de existir (o porquê) 
da arquitetura Serverless.

Em resumo, ele apresenta a evolução de uma arquitetura tradicional em 5 etapas:

1. A Aplicação Inicial (1 EC2 + Banco de Dados): O ponto de partida mais
simples para rodar uma aplicação HTTP.

2. Escalabilidade Vertical *Scaling Up* e seus Limites: O problema de tentar resolver 
o aumento de tráfego apenas aumentando o tamanho daquela única máquina EC2.

3. Escalabilidade Horizontal (Load Balancer + Várias EC2s): A solução de colocar 
um balanceador de carga para distribuir tráfego entre múltiplos servidores e o 
surgimento do problema de gestão de custos (pagar por máquinas paradas fora do horário de pico).

4. Elasticidade e Alta Disponibilidade (Auto Scaling + Zonas de Disponibilidade): A adição
do Auto Scaling para ligar/desligar instâncias conforme o tráfego e a distribuição 
em múltiplas AZs para suportar falhas em data centers.

5. O Problema Final: "Undifferentiated Heavy Lifting" (Trabalho Pesado Não Diferenciado): 
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

---

<div align="center">
  <img src="https://github.com/user-attachments/assets/ad6ee311-9160-4240-a15d-58408d49c32c"  width="1000"/>
</div>



---


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
3. E se um post do seu produto viralizar às 21h de uma quarta-feira?
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

<p align="center">
  <img width="600" height="350" alt="Image" src="https://github.com/user-attachments/assets/3cda25aa-db04-4220-8649-fc7e0645f957"  />
</p>


A grande sacada do modelo Serverless é justamente
**NÃO precisar usar nem Load Balancer nem Auto Scaling manuais**,
porque a própria AWS assume esse trabalho por você.

**Arquitetura serveless: Duas formas de resolver o mesmo problema**

Quando a sua aplicação cresce e recebe muito tráfego, você tem dois caminhos de arquitetura na AWS:

**1. O Caminho Tradicional (IaaS - Infraestrutura como Serviço)**
EC2 + Load Balancer + Auto Scaling

Como funciona: Você aluga e gerencia os servidores virtuais (EC2).

Trabalho do Arquiteto/Dev: Você precisa configurar o Load Balancer para distribuir o tráfego,
criar o Auto Scaling para ligar/desligar instâncias quando a CPU subir, configurar Health Checks, 
cuidar de patches do sistema operacional, etc.

O que você paga: Paga pelo tempo que as máquinas ficarem ligadas (mesmo se ninguém estiver usando o site na madrugada).

**2. O Caminho Serverless (FaaS / PaaS - Função como Serviço)**
API Gateway + AWS Lambda + DynamoDB

Como funciona: Você NÃO aluga servidores. Você apenas envia o seu código (sua função) para a AWS.

Trabalho do Arquiteto/Dev: Você só escreve a regra de negócio do seu sistema.

**O que acontece quando o tráfego aumenta?**

- Se entra 1 usuário, a AWS executa 1 função Lambda.

- Se entram 10.000 usuários ao mesmo tempo, a AWS automaticamente dispara 10.000 invocações paralelas do Lambda de forma instantânea.

- Você NÃO precisa criar Load Balancer nem Auto Scaling. A própria infraestrutura gerenciada da AWS faz o balanceamento e a escala invisível para você.

O que você paga: Paga estritamente por milissegundo de execução do seu código. Se o site receber 0 acessos na madrugada, você paga R$ 0,00.

### Cenário serveless: API Gateway + Lambda

No cenário Serverless, o API Gateway e o AWS Lambda entram para SUBSTITUIR de vez a dupla Load Balancer + EC2.

O De-Para das Arquiteturas:

| Função no Sistema | Arquitetura Tradicional (EC2) | Arquitetura Serverless |
|---|---|---|
| Porta de Entrada (Receber HTTP e rotear) | Load Balancer (ALB) | Amazon API Gateway |
| Executar a aplicação (Processar o código) | Servidores EC2 | AWS Lambda |
| Escalabilidade e Saúde | Auto Scaling Group | Automático pelo próprio Lambda |

---

**Como funciona na prática o cenário do Serverless?**

Imagine que um usuário clica em "Comprar produto" no seu site:

1. A requisição chega no Amazon API Gateway:

- Ele funciona como a porta de entrada pública do seu sistema (a URL da sua API).

- Ele recebe a chamada HTTP, valida se a requisição está correta (ex: checa autenticação/tokens) e chama a função Lambda correspondente.

2. O AWS Lambda processa a requisição:

- A AWS cria instantaneamente um **micro-container**, roda a sua função (o código que processa a compra) e salva o resultado no banco de dados.

- Assim que a resposta é entregue para o cliente, esse micro-container desliga ou fica congelado.

**Por que o API Gateway + Lambda eliminam o Auto Scaling?**

No modelo tradicional com EC2:

Se entrarem 1.000 requisições simultâneas, o Auto Scaling tem que perceber o uso de CPU subindo,
ligar novas máquinas EC2 (processo que leva minutos) e o Load Balancer tem que atualizar a lista de servidores.

No modelo Serverless:

Se entrarem 1.000 requisições simultâneas:

- O API Gateway recebe as 1.000 chamadas de uma vez.

- O AWS Lambda aciona 1.000 execuções da sua função ao mesmo tempo em milissegundos.

Não existe regra de Auto Scaling para criar, nem métrica de CPU para configurar.
O próprio serviço cria **instâncias do seu código em paralelo** conforme a demanda chega.

Resumo em uma frase
No cenário da aula do Mike Deck:

- Load Balancer + EC2 + Auto Scaling: É como alugar uma frota de ônibus para o seu negócio
(você precisa ajustar quantos ônibus ficam na rua de acordo com a hora do dia).

- API Gateway + AWS Lambda (Serverless): É como chamar um Uber para cada passageiro exatamente
no segundo em que ele precisa viajar (você não gerencia frota nenhuma, a plataforma simplesmente
atende cada chamada individualmente e escala sozinha).


## Cenário: 50.000 requisições em 1 hora

<p align="center">
  <img width="1000" height="250" alt="Image" src="https://github.com/user-attachments/assets/d165c0b2-c13c-4986-9d0c-52bd676333d8"  />
</p>

---

> Pergunta:
> Se temos 50 mil requisições por hora, são 50 mil lambdas com mico-container em paralelo?

Resposta: Depende de quantas requisições acontecem no MESMO MILISSEGUNDO (ao mesmo tempo).

O Lambda não cria 1 container por requisição total do dia, ele cria 1 container por execução SIMULTÂNEA (concorrência).

Imagine que cada chamada da sua função Lambda seja rápida e leve apenas 100 milissegundos (0,1 segundo) para responder.

--- 

**Cenário A: O tráfego vem "espalhado" ao longo da hora**

50.000 requisições divididas em 1 hora dão uma média de ~14 requisições por segundo.

- Como a sua função responde super rápido (0,1s), um único container Lambda
consegue processar 10 requisições por segundo, uma atrás da outra.

- Nesse caso, para atender 50 mil requisições na hora, a AWS vai precisar de apenas
2 ou 3 containers Lambda rodando em paralelo (eles vão sendo reutilizados no modo Warm).

---

**Cenário B: O "Pico de Black Friday" (TUDO NO MESMO SEGUNDO)**

Agora imagine que uma promoção começou e as 50.000 requisições chegaram TODAS no exato mesmo segundo!

- Aí sim! Como a AWS precisa processar 50 mil coisas no mesmo instante,
o AWS Lambda vai criar milhares de micro-containers em paralelo em questão de
segundos para processar tudo de uma vez.

- Assim que todas as 50.000 requisições forem respondidas, esses micro-containers congelam e são destruídos.

---

<p align="center">
  <img width="1000" height="250" alt="Image" src="https://github.com/user-attachments/assets/9717577c-ead6-4a84-a1d7-7435baac932e"  />
</p>

---


**O Conceito-Chave: Concorrência (Concurrency)**
No mundo Serverless, o limite que você monitora se chama Concorrência (quantas funções estão rodando no mesmo exato milissegundo).

```
Concorrência = Requisições por segundo xTempo de execução da função (em segundos)
```

- Se você recebe 1.000 requisições/seg e cada uma demora 0,1s --> Você precisa de 100 Lambdas em paralelo.
- Se você recebe 1.000 requisições/seg e cada uma demora 2,0s (uma função mais pesada) --> Você precisa de 2.000 Lambdas em paralelo.

- Por padrão, a AWS dá um limite inicial de 1.000 execuções simultâneas/concorrentes por região para a sua conta (que você pode pedir para aumentar com um clique se precisar de mais).

É por isso que o modelo Serverless é tão poderoso: se o seu código for rápido, um punhado de containers reaproveitados consegue responder centenas de milhares de pessoas sem você gastar quase nada!

---

**Conconrrência vs Paralelismo**
O cientista da computação Rob Pike (um dos criadores do Go) resume isso em uma frase perfeita:
"Concorrência é sobre lidar com muitas coisas ao mesmo tempo. Paralelismo é sobre fazer muitas coisas ao mesmo tempo."

Para entender a diferença de forma bem visual, imagine uma cafeteria:

1. Concorrência (Lidar com várias coisas)
Significa que o seu sistema foi projetado para gerenciar múltiplas tarefas que
estão em andamento no mesmo período, mas elas não precisam estar sendo executadas
no mesmo exato milissegundo.

A metáfora do Café: Pense em 1 barbearia com 1 barbeiro e 5 clientes na fila. 
O barbeiro atende o Cliente A por 5 minutos (faz a barba), manda ele esperar no sofá 
para secar a toalha quente, e enquanto isso atende o Cliente B (corta o cabelo).

Ele está lidando com vários clientes ao mesmo tempo (concorrência), mas como ele só tem 2 mãos,
ele só executa uma tarefa por vez trocando rapidamente entre elas.

<p align="center">
  <img width="600" height="350" alt="Image" src="https://github.com/user-attachments/assets/b29ea253-6911-45ec-b623-f9980e24e2c0"  />
</p>

--- 

2. Paralelismo (Fazer várias coisas ao mesmo tempo)
Significa que você tem hardware físico suficiente (múltiplos núcleos de CPU ou múltiplos containers)
para executar mais de uma tarefa no mesmo exato instante físico.

A metáfora do Café: Pense em 5 barbeiros trabalhando em paralelo em 5 cadeiras, cortando o cabelo 
de 5 clientes exatamente no mesmo segundo.

<p align="center">
  <img width="600" height="350" alt="Image" src="https://github.com/user-attachments/assets/6a05069d-6242-4e50-a166-e4659ed0137f"  />
</p>

---

Como isso se aplica no AWS Lambda?
No AWS Lambda, ocorrem as duas coisas juntas, mas a métrica principal que a AWS mede é a Concorrência:

Cada micro-container do Lambda é Single-Threaded por padrão para uma requisição: Ele pega 1 requisição, processa e devolve. Ele não faz paralelismo dentro do mesmo container para requisições diferentes.

Quando chegam 100 requisições no mesmo milissegundo: A AWS usa o Paralelismo de Infraestrutura — ela cria 100 micro-containers em núcleos/máquinas físicas diferentes para rodar todos juntos ao mesmo tempo.

À medida que a resposta de um termina: Esse container fica livre ("morno") e o sistema de Concorrência da AWS redireciona a próxima requisição da fila para ele.

---


## Iniciaização de servidores: Estados Cold, Hot e Warm-Up

<p align="center">
  <img width="1000" height="250" alt="Image" src="https://github.com/user-attachments/assets/442dde1a-5664-4485-9ef6-665896d0fada"   />
</p>

---

O termos **Cold, Hot e Warm-Up** são usados exatamente nesse contexto de inicialização de servidores.
Por exemplo, por padrão na AWS, os servidores que entram no *Auto Scaling* começam como "Cold"
e passam pelo processo de aquecimento até ficarem "Hot".

Vamos entender o que acontece em cada estado e como isso impacta a sua aplicação:

**O que significa Hot e Cold aqui?**

1. Servidores que já estão rodando (Estado "Hot" / Quente)

São as instâncias EC2 que já foram ligadas, carregaram o sistema operacional,
iniciaram a aplicação (ex: Nginx, Docker, Node.js, Java) e passaram nos Health Checks do Load Balancer.

O que acontece com o tráfego?
O Load Balancer já joga as requisições dos usuários direto para elas.
A resposta é instantânea.

2. Servidores novatos/entrando no grupo (Estado "Cold" / Frio)

Quando o tráfego dispara e o Auto Scaling decide criar uma nova EC2,
essa máquina não nasce pronta em 1 milissegundo.

Ela passa por um processo:

1. Provisionamento: A AWS aloca o hardware virtual.

2. Boot do Sistema Operacional: O Linux/Windows é iniciado (Boot).

3. Inicialização da Aplicação: O seu código, dependências e serviços são carregados.

4. Passagem no Health Check: O Load Balancer envia testes para garantir que a máquina responde com HTTP 200 (OK).

3. Warm-Up
   
Durante todo esse tempo de inicialização (que pode levar de 30 segundos a 5 minutos,
dependendo do tamanho da sua aplicação), a máquina está no estado "Cold" ou "Warm-up" (aquecendo).

> ⚠️ Importante: O Load Balancer NÃO envia nenhum tráfego de usuários para a nova máquina
>  enquanto ela não terminar de aquecer e passar no Health Check. Isso garante os 0 segundos de downtime.


**O Desafio da "Cold Start" (Inicialização Fria)**

Se o tráfego subir de forma muito rápida e violenta 
(ex: um ataque DDoS ou uma promoção relâmpago), as instâncias "Hot" 
que já estão rodando podem ficar sobrecarregadas antes que a nova instância "Cold" termine de aquecer.

Para resolver esse problema, a AWS tem recursos avançados de Auto Scaling:

- Auto Scaling Warm Pools (Piscina de Servidores Mornos):
A AWS permite que você mantenha instâncias em um estado "Warm" (Morno)
ou Stopped (Paradas com tudo instalado e pré-configurado).

- Quando o tráfego sobe, em vez de criar uma máquina do zero ("Cold"), o Auto Scaling pega uma máquina "Warm"
e liga ela em questão de segundos, cortando drasticamente o tempo de inicialização.

- Cooldown Period (Período de Esfriamento):
Após ligar ou desligar um servidor, o Auto Scaling
espera alguns minutos (período de cooldown) para a métrica de
CPU/tráfego se estabilizar antes de tomar outra decisão de ligar ou
desligar mais máquinas.

**E quando o tráfego cai? Como ele desliga?**

Quando o movimento diminui, o Auto Scaling inicia a escala para dentro (Scale In):

1. Ele seleciona o servidor sobressalente.

2. O Load Balancer coloca essa instância no estado de Deregistration Delay (ou Connection Draining).

3. O que isso faz?
O Load Balancer para de enviar novas requisições para essa máquina,
mas dá um tempo (ex: 300 segundos) para que os usuários que já estavam usando
aquela máquina terminem o que estavam fazendo.

4. Quando as conexões terminam, a EC2 é encerrada de forma segura e sem interromper ninguém.

**Resumo prático**

- Servidores que já atendem o site: Estão Hot (prontos e recebendo tráfego).

- Novos servidores chamados pelo Auto Scaling: Nascem Cold (são inicializados e testados em segundo plano).

- Inclusão no Load Balancer: Acontece de forma transparente só quando o servidor passa de Cold para Hot.

**Função Lambda**

o conceito de Cold Start existe no AWS Lambda e é inclusive um dos temas mais famosos (e debatidos) do mundo Serverless!
A diferença é que, enquanto no EC2 o Cold Start leva de 30 segundos a 5 minutos, no AWS Lambda ele dura alguns
milissegundos a poucos segundos.

Como funciona o Cold Start no Lambda?
No modelo Serverless, não existem servidores rodando 24 horas por dia esperando requisições.
O código fica "dormindo" no armazenamento da AWS.

Quando uma requisição HTTP chega (via API Gateway, por exemplo),
a AWS precisa criar o ambiente para rodar a sua função. Esse processo tem duas vias:
```bash

                  ┌─────────────────────────────────────────┐
                  │          Chega uma requisição           │
                  └────────────────────┬────────────────────┘
                                       │
                         Existe um container pronto?
                                      / \
                                 SIM /   \ NÃO
                                    /     \
                                   ▼       ▼
                        ┌─────────────┐ ┌─────────────┐
                        │  WARM START │ │  COLD START │
                        └─────────────┘ └─────────────┘

```

1. Cold Start (Inicialização Fria)
Acontece quando não há nenhum container pronto para atender a chamada. Isso ocorre principalmente quando:

- É a primeira requisição após muito tempo sem acessos.

- O tráfego aumenta de repente e a AWS precisa criar novos ambientes paralelos para dar conta do volume.

Nesse momento, a AWS faz o trabalho pesado nos bastidores:

1. Baixa o seu código do armazenamento.

2. Cria um micro-container seguro (ambiente de execução Firecracker).

3. Inicializa o runtime da linguagem (Node.js, Python, Java, etc.).

4. Executa o código de inicialização global (conexões com banco de dados, importação de bibliotecas/SDKs).

5. Executa a função em si (Handler) e retorna a resposta ao usuário.

> ⏱️ Impacto no tempo: O usuário que pegou o Cold Start sente um pequeno atraso
> (latência) na resposta, que costuma variar de 100ms a 2 segundos.


2. Warm Start (Inicialização Quente)
Após terminar de responder à primeira requisição, a AWS não destrói
o container imediatamente. Ela mantém esse micro-container ativo
("morno/quente") na memória durante algum tempo (geralmente cerca
 de 5 a 15 minutos de inatividade).

Se uma segunda requisição chegar logo em seguida:

1. A AWS reusa o container que já está pronto.

2. O código roda instantaneamente.

> ⏱️ Impacto no tempo: Resposta super rápida, na casa dos 5ms a 20ms.

O que influencia a duração do Cold Start no Lambda?

Não todo Cold Start é igual. Os principais fatores são:

1. Linguagem de Programação:

- Mais rápidas no boot (Leves): Python, Node.js, Go.

- Mais lentas no boot (Pesadas): Java, C# (.NET) - porque a JVM/CLR precisa ser inicializada.

Tamanho do Pacote / Dependências: Quanto mais bibliotecas pesadas você importar fora da função (ex: carregar um modelo de Machine Learning ou SDKs gigantes), mais tempo o container leva para subir.

Rede (VPC): Antigamente, colocar a função Lambda dentro de uma VPC privada aumentava o tempo de Cold Start em até 10 segundos. Hoje a AWS otimizou esse processo (AWS Hyperplane), reduzindo essa diferença.

2. Como minimizar ou eliminar o Cold Start?
Se a sua aplicação precisa de respostas em tempo
real sem nenhuma variação de latência, a AWS oferece soluções:

- Provisioned Concurrency (Concorrência Provisionada):
Você paga um valor extra para avisar a AWS: "Mantenha sempre 5 (ou 50) containers
'quentes' e prontos 24/7". Isso elimina 100% o Cold Start para esse limite de acessos.

- Código de Inicialização Leve: Importar apenas o necessário dos pacotes e otimizar a
conexão com banco de dados fora do handler.

- SnapStart (para Java): A AWS tira um "snapshot" da memória da função pré-inicializada
e restaura essa imagem em milissegundos quando precisa escalar.

**Tabela Comparativa: Cold Start no EC2 vs. Lambda**
**Sim, o conceito de *Cold Start* existe no AWS Lambda e é inclusive um dos temas mais famosos (e debatidos) do mundo Serverless!**

A diferença é que, enquanto no EC2 o *Cold Start* leva de **30 segundos a 5 minutos**, no AWS Lambda ele dura **alguns milissegundos a poucos segundos**.

---

### Como funciona o Cold Start no Lambda?

No modelo Serverless, não existem servidores rodando 24 horas por dia esperando requisições. O código fica "dormindo" no armazenamento da AWS.

Quando uma requisição HTTP chega (via API Gateway, por exemplo), a AWS precisa criar o ambiente para rodar a sua função. Esse processo tem duas vias:

```
                  ┌─────────────────────────────────────────┐
                  │          Chega uma requisição           │
                  └────────────────────┬────────────────────┘
                                       │
                         Existe um container pronto?
                                      / \
                                 SIM /   \ NÃO
                                    /     \
                                   ▼       ▼
                        ┌─────────────┐ ┌─────────────┐
                        │  WARM START │ │  COLD START │
                        └─────────────┘ └─────────────┘

```

#### 1. Cold Start (Inicialização Fria)

Acontece quando **não há nenhum container pronto** para atender a chamada. Isso ocorre principalmente quando:

* É a **primeira requisição** após muito tempo sem acessos.
* O tráfego **aumenta de repente** e a AWS precisa criar novos ambientes paralelos para dar conta do volume.

Nesse momento, a AWS faz o trabalho pesado nos bastidores:

1. **Baixa o seu código** do armazenamento.
2. **Cria um micro-container** seguro (ambiente de execução Firecracker).
3. **Inicializa o runtime** da linguagem (Node.js, Python, Java, etc.).
4. **Executa o código de inicialização global** (conexões com banco de dados, importação de bibliotecas/SDKs).
5. **Executa a função em si** (*Handler*) e retorna a resposta ao usuário.

> ⏱️ **Impacto no tempo:** O usuário que pegou o *Cold Start* sente um pequeno atraso (*latência*) na resposta, que costuma variar de **100ms a 2 segundos**.

#### 2. Warm Start (Inicialização Quente)

Após terminar de responder à primeira requisição, a AWS **não destrói o container imediatamente**. Ela mantém esse micro-container ativo ("morno/quente") na memória durante algum tempo (geralmente cerca de 5 a 15 minutos de inatividade).

Se uma **segunda requisição** chegar logo em seguida:

1. A AWS reusa o container que já está pronto.
2. O código roda instantaneamente.

> ⏱️ **Impacto no tempo:** Resposta super rápida, na casa dos **5ms a 20ms**.

---

### O que influencia a duração do Cold Start no Lambda?

Não todo *Cold Start* é igual. Os principais fatores são:

1. **Linguagem de Programação:**
* **Mais rápidas no boot (Leves):** Python, Node.js, Go.
* **Mais lentas no boot (Pesadas):** Java, C# (.NET) — porque a JVM/CLR precisa ser inicializada.


2. **Tamanho do Pacote / Dependências:** Quanto mais bibliotecas pesadas você importar fora da função (ex: carregar um modelo de Machine Learning ou SDKs gigantes), mais tempo o container leva para subir.
3. **Rede (VPC):** Antigamente, colocar a função Lambda dentro de uma VPC privada aumentava o tempo de *Cold Start* em até 10 segundos. Hoje a AWS otimizou esse processo (*AWS Hyperplane*), reduzindo essa diferença.

---

### Como minimizar ou eliminar o Cold Start?

Se a sua aplicação precisa de respostas em tempo real sem nenhuma variação de latência, a AWS oferece soluções:

* **Provisioned Concurrency (Concorrência Provisionada):** Você paga um valor extra para avisar a AWS: *"Mantenha sempre 5 (ou 50) containers 'quentes' e prontos 24/7"*. Isso elimina 100% o *Cold Start* para esse limite de acessos.
* **Código de Inicialização Leve:** Importar apenas o necessário dos pacotes e otimizar a conexão com banco de dados fora do *handler*.
* **SnapStart (para Java):** A AWS tira um "snapshot" da memória da função pré-inicializada e restaura essa imagem em milissegundos quando precisa escalar.

---

### Tabela Comparativa: Cold Start no EC2 vs. Lambda

| Característica | EC2 (Com Auto Scaling) | AWS Lambda (Serverless) |
| --- | --- | --- |
| **O que é o Cold Start?** | Subir uma Máquina Virtual inteira do zero (Boot de SO + App). | Criar um micro-container leve e carregar o código. |
| **Tempo médio de Cold Start** | **30 segundos a 5 minutos** | **100 milissegundos a 2 segundos** |
| **Como o tráfego é afetado?** | O Load Balancer bloqueia o tráfego até o servidor ficar *Hot*. | O usuário que disparou a requisição aguarda o tempo do *Cold Start*. |
| **Gerenciamento** | Você configura regras de Auto Scaling e AMI. | 100% gerenciado e transparente pela AWS. |

---

## Resumo visual: Abordagem tradicional vs Abrodagem Serveless

```bash

 ┌─────────────────────────────────────────────────────────┐
 │               ABORDAGEM TRADICIONAL                     │
 │  Clientes ──► [ Load Balancer ] ──► [ Auto Scaling ]    │
 │                                       ├── EC2 (Servidor 1)│
 │                                       └── EC2 (Servidor 2)│
 └─────────────────────────────────────────────────────────┘
   * Você configura, monitora e gerencia os servidores.


 ┌─────────────────────────────────────────────────────────┐
 │               ABORDAGEM SERVERLESS                      │
 │  Clientes ──► [ API Gateway ] ──► [ AWS Lambda ]        │
 └─────────────────────────────────────────────────────────┘
   * Zero servidores para gerenciar. 
   * A AWS escala de 0 a milhares automaticamente.


```



