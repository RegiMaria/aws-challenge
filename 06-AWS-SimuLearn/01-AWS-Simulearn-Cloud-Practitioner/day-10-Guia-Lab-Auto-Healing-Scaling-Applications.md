# AWS SimuLearn - Auto-Healing and Scaling Applications
### Guia completo para iniciantes (Overview, Contexto, Prática e DIY)

---

## 📘 Overview (Visão Geral)

### Cenário de negócio simulado

Um **cybercafé de jogos (gaming café)** quer implementar **servidores com auto-recuperação (auto-healing)** e **restringir os clientes a uma capacidade de provisionamento específica** - ou seja, limitar quantos servidores cada cliente pode criar.

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

## 🧩 Contexto: Auto-Healing and Scaling Applications

### Descrição do problema

O gaming café quer:
1. Implementar servidores com **auto-recuperação** (se um servidor quebrar, ele é substituído automaticamente).
2. **Restringir** os clientes a uma capacidade específica de provisionamento (limite diário de servidores).

### Anotações técnicas da solução

1. Criar um **Auto Scaling group** com **launch configurations** (modelos de configuração de lançamento).
2. Configurar **scheduled scaling** (escalonamento agendado).

### A conversa com o cliente (Rusty Ketchum)

> **Rusty Ketchum:** Oi! Eu sou dono do único gaming café da cidade, e a gente precisa de ajuda.
>
> **Você:** Claro. Eu também sou gamer - sempre pronto pro próximo desafio.
>
> **Rusty:** Nosso gaming café está bombando agora. A gente deixa os clientes hospedarem seus próprios servidores de jogo.
>
> **Rusty:** A gente tem um limite diário de quantas instâncias EC2 cada cliente pode provisionar, mas não temos como aplicar esse limite automaticamente.
>
> **Rusty:** Primeiro problema: se uma instância EC2 quebra, não temos como substituí-la automaticamente.
>
> **Rusty:** Segundo problema: precisamos impor o limite diário de servidores por cliente.
>
> **Você:** Eu acho que **Auto Scaling groups** seriam perfeitos para resolver esses dois desafios.
>
> **Você:** Você pode criar **launch templates** para os servidores de jogo dos seus clientes. Depois, pode configurá-los para aumentar ou diminuir automaticamente o número de instâncias EC2 conforme a demanda muda.
>
> **Você:** E você pode definir limites máximos de quantas instâncias cada cliente pode provisionar.
>
> **Rusty:** Ótimo! Mas temos outro problema.
>
> **Rusty:** Toda terça-feira a gente faz uma festa grande, das 20h à 1h da manhã. Precisamos que os servidores estejam prontos quando a festa começa e desliguem automaticamente quando ela acaba.
>
> **Rusty:** Tem como agendar automaticamente quando os servidores de jogo ligam e desligam?
>
> **Você:** Com Auto Scaling groups, você pode configurar o **scheduled scaling** para ligar e desligar instâncias com base em um horário que você define.
>
> **Rusty:** Show! Acho que Auto Scaling groups vão funcionar pra gente.

### 💡 Explicando os conceitos principais

| Conceito | O que significa (explicação simples) |
|---|---|
| **EC2 (Elastic Compute Cloud)** | São os "servidores virtuais" da AWS - máquinas que rodam suas aplicações na nuvem. |
| **Auto Scaling Group (ASG)** | Um grupo que gerencia automaticamente quantas instâncias EC2 existem, aumentando ou diminuindo conforme necessário. Também substitui instâncias que falham (auto-healing). |
| **Launch Template** | Um "modelo" que guarda as configurações padrão para criar novas instâncias (tipo de máquina, imagem, rede, chave de acesso etc.), evitando digitar tudo de novo toda vez. |
| **Scheduled Scaling (Escalonamento agendado)** | Permite programar horários fixos para aumentar ou diminuir o número de servidores - por exemplo, ligar antes de uma festa e desligar depois. |
| **Auto-healing** | Quando uma instância falha ou é encerrada inesperadamente, o Auto Scaling group cria automaticamente uma nova para substituí-la. |

### Passo a passo da solução proposta (etapa de design)

**Objetivo:** Configurar regras de escalonamento agendado (scheduled scaling) para um Auto Scaling group do EC2.

1. É criado um **Amazon EC2 Auto Scaling group**, responsável por manter o número ideal de instâncias EC2 necessárias para lidar com as variações de carga de trabalho do servidor de aplicação.
2. É criada uma **AMI (Amazon Machine Image)** do servidor de aplicação. A AMI funciona como um "molde"/template usado para criar novas instâncias EC2 com a mesma configuração do servidor original.
3. É criado um **launch template**, que agiliza o processo de lançamento de instâncias EC2, especificando parâmetros de configuração como: ID da AMI, tipo de instância, par de chaves (key pair) e grupos de segurança (security groups).
4. O launch template é então usado para configurar um **Auto Scaling group**, que gerencia o escalonamento das instâncias com base em gatilhos (triggers) e agendamentos (schedules).
5. O Auto Scaling group é configurado com **limites mínimo e máximo de capacidade** e uma **política de target tracking** (rastreamento de meta). Quando o uso médio de CPU atinge o limite definido, alarmes do **CloudWatch** disparam a adição de novas instâncias para distribuir a carga de trabalho.
6. Quando o uso de CPU permanece **abaixo** do limite por um período determinado, as instâncias excedentes são encerradas.
7. Para padrões de carga de trabalho previsíveis (como a festa de terça-feira), podem ser configuradas **ações agendadas (scheduled actions)** para provisionar instâncias automaticamente em horários específicos, antecipando aumentos de demanda.
8. Uma **ação agendada complementar** encerra todas as instâncias depois que a demanda diminui, até que sejam necessárias novamente no dia seguinte.

---

## 🧪 Prática (Practice Lab)

### Objetivo da prática

Neste laboratório prático, você vai:
- Criar um **Amazon EC2 Auto Scaling group**.
- Atribuir instâncias EC2 a esse Auto Scaling group.

### Metas do laboratório prático
- Criar um Amazon EC2 Auto Scaling group.
- Atribuir instâncias EC2 ao Auto Scaling group.

> ⚠️ **Nota:** Neste ambiente de laboratório, apenas os serviços da AWS necessários para o exercício estão habilitados. Além disso, as funcionalidades desses serviços são limitadas ao que o lab exige.

---

### 📍 Passo a passo completo (38 passos)

**Passo 1 - Introdução**
1. Revise os objetivos do lab prático na seção Concept.
2. Clique em **Start Lab** ou **Open AWS Console** para começar.
3. Siga as instruções cuidadosamente, usando as setas para navegar entre os passos.

> 💡 **Conceito:** O **Amazon EC2 (Elastic Compute Cloud)** fornece servidores virtuais seguros e redimensionáveis na nuvem AWS.

**Passo 2 - Acessando o console EC2**
1. Na barra de navegação superior, na caixa de busca, digite: `ec2`
2. Nos resultados, em Services, clique em **EC2**.
3. Vá para o próximo passo.

**Passo 3 - Verificando a instância existente**
1. No painel esquerdo, clique em **Instances**.
2. Na seção Instances, revise o número de instâncias em execução.
3. Para a instância **Game Server**, clique no ID fornecido em Instance ID.
4. Vá para o próximo passo.

> 💡 **Conceito:** Endereços **IPv4 públicos** permitem que suas instâncias EC2 se comuniquem com a internet em ambas as direções (enviar e receber tráfego).

**Passo 4 - Copiando o IP público**
1. Em Public IPv4 address, clique no ícone de cópia para copiar o endereço fornecido.
2. Vá para o próximo passo.

**Passo 5 - Testando o servidor**
1. Em uma nova aba (ou janela) do navegador, na barra de endereço, digite: `http://`
2. Para completar a URL, cole o IP público copiado logo após o protocolo (`http://`) e pressione Enter.
3. Volte para a aba/janela do console AWS.
4. Vá para o próximo passo.

> 💡 **Conceito:** Você pode salvar a configuração e o conteúdo de uma instância EC2 como uma **AMI (Amazon Machine Image)**. Essa AMI funciona como um "molde" para lançar novas instâncias com a mesma configuração.

**Passo 6 - Criando a imagem (AMI)**
1. Clique em **Actions** para abrir o menu.
2. Escolha **Image and templates**.
3. Escolha **Create image**.
4. Vá para o próximo passo.

> 💡 **Conceito:** Ao criar uma AMI, o EC2 automaticamente: desliga a instância, tira snapshots (fotos) dos volumes, cria a AMI, e depois reinicia a instância. Se você não quiser que a instância reinicie, pode marcar a opção **"No reboot"**.

**Passo 7 - Configurando a imagem**
1. Em Image name, digite: `GameServer`
2. Em Image description, digite: `Game server image`
3. Role a página até a seção Tags.
4. Vá para o próximo passo.

> 💡 **Conceito:** Sua AMI só fica disponível na Região AWS onde foi criada. Para usá-la em outra Região, é preciso **copiá-la** para lá.

**Passo 8 - Finalizando a criação da AMI**
1. Em Tags, mantenha a opção padrão **Tag image and snapshots together**.
2. Clique em **Create image**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Um **launch template** armazena as configurações de uma instância EC2 para que você possa reutilizá-las ao lançar novas instâncias.

**Passo 9 - Verificando a AMI e acessando Launch Templates**
1. No painel esquerdo, em Images, clique em **AMIs**.
2. Na seção AMIs, verifique se o status da AMI GameServer está como **Available** (pode levar até 5 minutos - atualize a página periodicamente).
3. No painel esquerdo, clique em **Launch Templates**.

> 💡 **Conceito:** Launch templates economizam tempo, guardando configurações comuns de instância (como ID da AMI, tipo de instância e configurações de rede), evitando que você as digite toda vez.

**Passo 10 - Criando o launch template**
1. Na página inicial de launch templates do EC2, clique em **Create launch template**.
2. Vá para o próximo passo.

> 💡 **Conceito:** Cada Região da AWS permite até 5.000 launch templates, e cada template pode ter até 10.000 versões.

**Passo 11 - Nomeando o template**
1. Em Launch template name, digite: `GameServerTemplate`
2. Em Template version description, digite: `Game server template`
3. Em Auto Scaling guidance, marque a caixa **Provide guidance to help me...**
4. Vá para o próximo passo.

> 💡 **Conceito:** Ao adicionar uma AMI ao seu launch template, você pode escolher entre: AMIs próprias (que você criou), AMIs compartilhadas por outras contas, ou Quick Start AMIs (com sistemas operacionais comuns já prontos).

**Passo 12 - Selecionando a AMI**
1. Na seção Application and OS Images, clique na aba **My AMIs**.
2. Escolha **Owned by me**.
3. Em Amazon Machine Image (AMI), na lista suspensa, escolha **GameServer**.
4. Vá para o próximo passo.

> 💡 **Conceito:** Um **key pair (par de chaves)** é uma credencial de segurança necessária para acessar suas instâncias EC2:
> - Em instâncias Linux: usada para login via SSH.
> - Em instâncias Windows: usada para obter a senha de administrador para login via RDP.

**Passo 13 - Definindo tipo de instância e criando key pair**
1. Em Instance type, escolha **t3.micro**.
2. Em Key pair name, clique em **Create new key pair**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Um key pair tem duas partes:
> - Uma **chave pública**, que fica armazenada no EC2.
> - Uma **chave privada**, que você baixa e deve manter em segurança.
>
> ⚠️ **Importante:** Nunca compartilhe sua chave privada nem faça upload dela para armazenamento em nuvem.

**Passo 14 - Criando o key pair**
1. Na janela pop-up, em Key pair name, digite: `GameServerKeyPair`
2. Em Key pair type, escolha **RSA**.
3. Em Private key file format, escolha **.pem**.
4. Clique em **Create key pair**.
5. Quando solicitado, salve o arquivo `GameServerKeyPair` no seu dispositivo.
6. Vá para o próximo passo.

> 💡 **Conceito:** Ao adicionar um security group (grupo de segurança) à sua configuração de lançamento, todas as instâncias criadas a partir dela terão as mesmas regras de acesso (firewall).

**Passo 15 - Configurando o security group**
1. Em Firewall (security groups), escolha **Select existing security group**.
2. Em Security groups, escolha **WebServerSecurityGroup**.
3. Vá para o próximo passo.

**Passo 16 - Revisando e criando o template**
1. No final da página, revise a seção Summary.
2. Clique em **Create launch template**.
3. Vá para o próximo passo.

> 💡 **Conceito:** O launch template ajuda o Amazon EC2 Auto Scaling a criar novas instâncias automaticamente quando necessário. Por exemplo, se um servidor falhar, o EC2 Auto Scaling usa o template para lançar uma instância substituta (isso é o **auto-healing**).

**Passo 17 - Visualizando o template criado**
1. No alerta de sucesso, revise a mensagem.
2. No final da página, clique em **View launch templates**.
3. Vá para o próximo passo.

> 💡 **Conceito:** O **Amazon EC2 Auto Scaling** ajuda a manter o número correto de instâncias para as necessidades da sua aplicação:
> - Você cria Auto Scaling groups para gerenciar coleções de instâncias EC2.
> - Cada grupo precisa de um número **mínimo**, **máximo** e **desejado** de instâncias.
> - O EC2 Auto Scaling adiciona ou remove instâncias automaticamente conforme a demanda da aplicação.

**Passo 18 - Acessando Auto Scaling Groups**
1. No painel esquerdo, clique em **Auto Scaling Groups** (talvez seja necessário clicar no ícone de menu ☰ para expandir o painel).
2. Na página inicial do EC2 Auto Scaling, clique em **Create Auto Scaling group**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Como já mencionado, launch templates guardam todas as configurações necessárias para que os Auto Scaling groups criem instâncias, incluindo ID da AMI, tipo de instância, key pair e security groups.

**Passo 19 - Nomeando o Auto Scaling Group**
1. Em Choose launch template or configuration, em Auto Scaling group name, digite: `RegularCustomerGameServer`
2. Em Launch template, escolha **GameServerTemplate**.
3. Role até o final da página e clique em **Next**.
4. Vá para o próximo passo.

> 💡 **Conceito:** Distribuir seus recursos em várias **Zonas de Disponibilidade (Availability Zones, AZs)** ajuda a manter suas aplicações funcionando mesmo se uma AZ falhar. Cada AZ é um data center localizado separadamente dentro da mesma Região.

**Passo 20 - Configurando rede**
1. Em Choose instance launch options, em VPC, escolha o nome da VPC que termina com `auto-healing-and-scaling/GameServerVPC`.
2. Em Availability Zones and subnets, escolha as duas sub-redes que contêm `game-server-netSubnet`.
3. Role até o final da página e clique em **Next**.
4. Vá para o próximo passo.

**Passo 21 - Balanceamento de carga**
1. Em Integrate with other services, em Load balancing, escolha **No load balancer**.
2. Na seção de integração com VPC Lattice, mantenha as configurações padrão.
3. Vá para o próximo passo.

> 💡 **Conceito:** O Amazon EC2 Auto Scaling verifica a saúde das instâncias de duas formas:
> - **EC2 status checks**: detectam problemas de hardware e software.
> - **Load balancer health checks**: monitoram se as instâncias estão funcionando corretamente.

**Passo 22 - Configurando health checks**
1. Na seção Health checks, em Health check grace period, digite: `240`
2. Clique em **Next**.
3. Vá para o próximo passo.

> 💡 **Conceito:** O Amazon EC2 Auto Scaling mantém o número desejado de instâncias em um grupo. Esse número desejado deve estar:
> - No mínimo igual ao tamanho mínimo do grupo.
> - No máximo igual ao tamanho máximo do grupo.

**Passo 23 - Definindo capacidade do grupo**
1. Em Configure group size and scaling, em Desired capacity, digite: `2`
2. Em Min desired capacity, digite: `2`
3. Em Max desired capacity, digite: `4`
4. Role até o final da página e clique em **Next**.
5. Vá para o próximo passo.

> 💡 **Conceito:** O Amazon EC2 Auto Scaling pode enviar notificações quando adiciona ou remove instâncias do seu grupo. Essas notificações funcionam através do **Amazon SNS (Simple Notification Service)**.

**Passo 24 - Notificações**
1. Em Add notifications, clique em **Skip to review**.
2. Vá para o próximo passo.

**Passo 25 - Revisão final e criação**
1. Na etapa Review, revise suas configurações.
2. No final da página, clique em **Create Auto Scaling group**.
3. Vá para o próximo passo.

> 💡 **Conceito:** Depois de criar uma política de escalonamento (scaling policy), o Amazon EC2 Auto Scaling começa a monitorar as métricas escolhidas.

**Passo 26 - Verificando o grupo criado**
1. No alerta de sucesso, revise a mensagem.
2. Em Status, revise o status do Auto Scaling group (você pode continuar enquanto o grupo está sendo atualizado).
3. Clique em **RegularCustomerGameServer**.
4. Vá para o próximo passo.

> 💡 **Conceito:** Você pode verificar o histórico do seu Auto Scaling group para ver o que causou mudanças no número de instâncias.

**Passo 27 - Revisando o histórico de atividades**
1. Clique na aba **Activity**.
2. Na seção Activity history, verifique que duas instâncias foram criadas para atingir a "capacidade desejada e real" (desired and actual capacity).
3. Clique na aba **Automatic scaling**.
4. Vá para o próximo passo.

**Passo 28 - Criando política de escalonamento dinâmico**
1. Na seção Dynamic scaling policies, clique em **Create dynamic scaling policy**.
2. Vá para o próximo passo.

> 💡 **Conceito:** Com **target tracking scaling** (escalonamento por rastreamento de meta), você define um valor-alvo para uma métrica, como o uso de CPU. O Amazon EC2 Auto Scaling então adiciona ou remove instâncias automaticamente para manter esse valor.

**Passo 29 - Configurando target tracking**
1. Em Policy type, escolha (ou mantenha) **Target tracking scaling**.
2. Em Scaling policy name, digite: `CPU Utilization`
3. Em Metric type, escolha **Average CPU utilization**.
4. Em Target value, digite: `70`
5. Clique em **Create**.
6. Vá para o próximo passo.

**Passo 30 - Verificando a política criada**
1. No alerta de sucesso, revise a mensagem.
2. Revise a política de target tracking criada.
3. Vá para o próximo passo.

> 💡 **Conceito:** O **scheduled scaling** (escalonamento agendado) permite ajustar o número de instâncias em horários específicos. Por exemplo, você pode adicionar instâncias toda segunda-feira de manhã quando espera mais visitantes no site.

**Passo 31 - Criando ação agendada**
1. Na seção Scheduled actions, clique em **Create scheduled action**.
2. Vá para o próximo passo.

> 💡 **Conceito:** O escalonamento agendado usa o horário **UTC** por padrão, mas você pode alterar para o seu fuso horário local.

**Passo 32 - Configurando a ação agendada (festa de terça-feira)**
1. Na janela pop-up, em Name, digite: `SecondWaveOfRegulars`
2. Em Desired capacity, digite: `3`
3. Em Min, digite: `3`
4. Em Max, digite: `4`
5. Em Recurrence, escolha **Every week**.
6. Em Specific start time, escolha uma data futura.
7. No campo seguinte, digite `20:00`.
8. Clique em **Create**.
9. Vá para o próximo passo.

> ⚠️ **Atenção - sobre a data usada no exercício:** O material original do lab sugere usar a data `2025/08/11` como exemplo. Porém, essa data é apenas ilustrativa - como o tempo passa, ela pode já estar **no passado** em relação ao dia em que você está fazendo o lab.
>
> O AWS **não permite criar uma scheduled action com Start time no passado** - se você tentar, vai aparecer o erro:
> ```
> Não foi possível criar ou editar a ação agendada
> Given start time is in the past
> ```
>
> **Solução:** sempre escolha uma data **futura** em relação ao dia atual - pode ser o dia seguinte (amanhã) ou qualquer data à frente. Por exemplo, se hoje é `2026/09/03`, você pode usar `2026/09/04` (amanhã) como Specific start time.
>
> Isso não muda o objetivo do exercício: o "start time" é apenas o primeiro disparo da ação. Como a Recurrence está definida como **Every week**, a partir dessa primeira execução futura ela vai continuar se repetindo semanalmente, toda terça-feira às 20:00 - exatamente como o cenário do Rusty pede.

**Passo 33 - Revisando a ação agendada**
1. Na seção Scheduled actions, revise a nova ação criada.
2. Vá para o próximo passo.

**Passo 34 - Verificando as novas instâncias**
1. Volte para a página Instances no console EC2.
2. Revise as duas novas instâncias sendo criadas pelo Auto Scaling group (se não aparecerem, clique no ícone de atualizar da seção).
3. Vá para o próximo passo.

> 💡 **Conceito:** Ao criar uma política de target tracking, o Amazon EC2 Auto Scaling cria automaticamente alarmes do **Amazon CloudWatch** que:
> - Adicionam instâncias quando uma métrica ultrapassa o alvo definido.
> - Removem instâncias quando a métrica fica abaixo do alvo.

**Passo 35 - Acessando o CloudWatch**
1. Navegue até o console do Amazon CloudWatch (use a caixa de busca Services na barra superior).
2. No painel esquerdo, clique em **Alarms**.
3. Na seção Alarms, clique no alarme cujo nome começa com `TargetTracking-RegularCustomerGameServer-AlarmHigh-`.
4. Vá para o próximo passo.

> 💡 **Conceito:** O CloudWatch pode monitorar o uso de CPU de cada instância EC2 individualmente, para identificar quais estão com uso elevado.

**Passo 36 - Analisando o gráfico de CPU**
1. No gráfico CPUUtilization, observe a linha vermelha **In alarm** no eixo dos 70% (esse valor foi definido quando você criou a política de escalonamento CPU Utilization).
2. Observe a linha azul **CPUUtilization**, que representa o uso médio de CPU atual (se não aparecer, os dados podem ainda não ter sido coletados).
3. Vá para o próximo passo.

> 💡 **Conceito:** Quando uma métrica atinge o limite do alarme, o CloudWatch avisa o Auto Scaling group para adicionar ou remover instâncias, de acordo com a política definida.

**Passo 37 - Verificando a ação do alarme**
1. No final da página, clique na aba **Actions**.
2. Para a ação Auto Scaling, revise a descrição.
3. Vá para o próximo passo.

**Passo 38 - Conclusão da prática**
🎉 Parabéns! Você concluiu a seção Practice. Agora vá para a seção **DIY** para completar a solução.

---

## 🛠️ DIY (Faça Você Mesmo)

### Metas do DIY

1. Configurar uma política de auto scaling para **reduzir a capacidade para 0 instâncias às 01:00 da manhã, todos os dias**.

### Método de validação da solução

O servidor de teste irá verificar se você configurou corretamente o seu Auto Scaling group. Dicas:
- A ação de auto scaling deve **começar diariamente às 01:00 (1:00 AM)**.
- A ação **não deve ter horário de término definido** (sem end time).
- A ação deve **reduzir o número de instâncias para 0**.

### 💡 Explicando o que você precisa fazer

Lembra da conversa com o Rusty? A festa de terça-feira vai das 20h à 1h da manhã. Você já criou uma ação agendada (`SecondWaveOfRegulars`) para **ligar** os servidores extras às 20h. Agora falta a ação complementar: **desligar tudo (zerar as instâncias) às 01:00**, quando a festa termina - todos os dias, sem data para parar de repetir.

Isso é feito criando uma **nova scheduled action (ação agendada)** no mesmo Auto Scaling group (`RegularCustomerGameServer`), configurada para rodar diariamente à 1h da manhã, com capacidade **mínima, máxima e desejada = 0**, e **sem data de término**.

### Passo a passo sugerido para o DIY

1. No console AWS, acesse o serviço **EC2**.
2. No painel esquerdo, clique em **Auto Scaling Groups**.
3. Clique no grupo **RegularCustomerGameServer** (o mesmo criado na prática).
4. Clique na aba **Automatic scaling**.
5. Na seção **Scheduled actions**, clique em **Create scheduled action**.
6. Preencha os campos da nova ação agendada:
   - **Name**: digite um nome descritivo, por exemplo `ScaleDownToZero`.
   - **Desired capacity**: digite `0`
   - **Min**: digite `0`
   - **Max**: digite `0`
   - **Recurrence**: escolha **Every day** (ou use uma expressão cron equivalente a "todos os dias à 1h", por exemplo `0 1 * * *`).
   - **Start time**: defina o horário de início às `01:00`.
   - **End time**: **deixe em branco** (não defina data de término, pois a ação deve se repetir indefinidamente).
7. Clique em **Create** para salvar a ação agendada.
8. Revise a seção **Scheduled actions** para confirmar que a nova ação aparece corretamente, com capacidade 0/0/0, recorrência diária às 01:00 e sem data de término.

### ⚠️ Pontos de atenção (erros comuns)

- **Não esqueça de zerar os três valores**: Desired, Min e Max capacity devem ser `0` - se deixar o Min maior que 0, o Auto Scaling group não vai conseguir reduzir para zero instâncias.
- **Não defina um End time (data de término)** - a validação verifica justamente que a ação **não tenha** horário de término, ou seja, ela deve continuar se repetindo todos os dias indefinidamente.
- **Recorrência diária**: certifique-se de escolher a opção que repete **todos os dias** (não semanal), já que a meta é zerar a capacidade toda madrugada, não apenas uma vez por semana.
- **Fuso horário**: lembre-se de que, por padrão, o scheduled scaling usa **UTC**. Verifique se o horário 01:00 configurado está de acordo com o fuso horário esperado pela validação (ajuste se a plataforma permitir escolher o fuso horário local).

---

## ✅ Resumo dos conceitos aprendidos

| Termo | Explicação resumida |
|---|---|
| **AMI** | Molde/template de uma instância EC2, usado para criar novas instâncias idênticas. |
| **Launch Template** | Guarda as configurações padrão (AMI, tipo de instância, key pair, security group) para facilitar a criação de instâncias. |
| **Auto Scaling Group (ASG)** | Gerencia automaticamente a quantidade de instâncias EC2, aumentando, diminuindo ou substituindo-as conforme necessário. |
| **Auto-healing** | Substituição automática de instâncias que falham. |
| **Target Tracking Scaling** | Escalonamento automático baseado em uma métrica (ex: CPU) tentando manter um valor-alvo. |
| **Scheduled Scaling** | Escalonamento baseado em horários fixos e recorrentes (ex: ligar às 20h, desligar à 1h). |
| **CloudWatch Alarm** | Alarme que monitora métricas (como CPU) e aciona ações do Auto Scaling quando um limite é ultrapassado. |
| **Security Group** | Conjunto de regras de firewall aplicadas às instâncias. |
| **Key Pair** | Par de chaves (pública/privada) usado para acesso seguro às instâncias. |

---

*Guia elaborado a partir do conteúdo do AWS SimuLearn - Auto-Healing and Scaling Applications, com explicações adicionais para facilitar o entendimento de iniciantes.*
