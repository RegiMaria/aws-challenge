<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" width="220" alt="AWS Logo"/>

#  05 - More Environments, More Innovation

</div>

## Principais Aprendizados deste Tópico

1. Democratização de Ambientes de Teste / Dev:

- No modelo tradicional (EC2): Ter múltiplos ambientes (dev, qa, staging, prod)
é caríssimo, porque você paga pelos servidores parados 24/7 mesmo quando ninguém está testando.

- No modelo Serverless: Como não há custo ocioso, você pode criar um ambiente
isolado completo para cada desenvolvedor ou até para cada feature branch no Git. Se
ninguém usar aquele ambiente no fim de semana, o custo é zero.

--- 

<div align="center">

<img src="https://github.com/user-attachments/assets/8577c638-9714-43e9-a34c-5bca8216b023" width="600" alt="AWS Logo"/>

</div>

---

2. Fim dos Conflitos no Ambiente de QA:

- Desenvolvedores não precisam disputar quem usa o "servidor de testes"
da empresa ou perder tempo tentando rodar mocks complexos no próprio computador local.
Cada um testa seu código em uma réplica exata do ambiente de produção na própria nuvem.

3. A Automação (IaC e CI/CD) virou ponto obrigatório:

Como criar ambientes ficou fácil e barato, a quantidade de ambientes dispara. 
Para não virar um caos manual, a automação com Infraestrutura como Código (IaC) -
usando ferramentas como AWS SAM, Serverless Framework ou Terraform -
e esteiras de CI/CD torna-se o motor indispensável do Serverless.

---

**Resumo do Módulo: Getting into the Serverless Mindset**

Parabéns! Concluímos a primeira aula fundamental da trilha Serverless Knowledge Badge.

Passamos por todos os 5 tópicos:

1. A Familiar Architecture: Entendemos a evolução e os problemas da infraestrutura tradicional com EC2,
Load Balancer e Auto Scaling (Undifferentiated Heavy Lifting).

2. How Can Serverless Help: Vimos os 4 pilares do Serverless (Sem gerenciamento de host,
escala flexível, alta disponibilidade nativa e nunca pagar pelo ocioso).

3. Managed Services Connected by Functions: Aprendemos que Serverless é usar o Lambda como "cola"
para conectar serviços gerenciados (API Gateway, DynamoDB, S3, Step Functions) em uma arquitetura orientada a eventos.

4. From Hours to Minutes with Parallelization: Descobrimos o poder do paralelismo massivo (ex: transcodificação de vídeos
em 10 minutos em vez de 6 horas, economizando 90% dos custos).

5. More Environments, More Innovations: Vimos como a ausência de custo ocioso permite criar ambientes ilimitados para devs e branches,
acelerando a inovação e o tempo de lançamento no mercado (time-to-market).
