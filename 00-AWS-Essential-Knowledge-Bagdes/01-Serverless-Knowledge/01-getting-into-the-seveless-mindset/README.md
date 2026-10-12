<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" width="220" alt="AWS Logo"/>

</div>




---

Esse repositório possui 4 tópicos de estudos:

## 1. A familiar Architecture
- Link [para as notas de estudos aqui](https://github.com/RegiMaria/aws-challenge/blob/main/00-AWS-Essential-Knowledge-Bagdes/01-Serverless-Knowledge/01-getting-into-the-seveless-mindset/01-a-familiar-architeccture.md)

Neste primeiro tópico (A Familiar Architecture), 
o Mike Deck apresenta o cenário tradicional de infraestrutura na nuvem
e os problemas gerados por ele para, no final, introduzir a razão de existir (o porquê) do Serverless.

Em resumo, ele apresenta a evolução de uma arquitetura tradicional em 5 etapas:

1. A Aplicação Inicial (1 EC2 + Banco de Dados): O ponto de partida mais simples para rodar uma aplicação HTTP.

3. Escalabilidade Vertical e seus Limites: O problema de tentar resolver o aumento de tráfego
apenas aumentando o tamanho daquela única máquina EC2.

4. Escalabilidade Horizontal (Load Balancer + Várias EC2s): A solução de colocar um balanceador
de carga para distribuir tráfego entre múltiplos servidores e o surgimento do problema de gestão de
custos (pagar por máquinas paradas fora do horário de pico).

5. Elasticidade e Alta Disponibilidade (Auto Scaling + Zonas de Disponibilidade): A adição do Auto
Scaling para ligar/desligar instâncias conforme o tráfego e a distribuição em múltiplas AZs para suportar falhas em data centers.

**O Problema Final:** "Undifferentiated Heavy Lifting" (Trabalho Pesado Não Diferenciado): Ele mostra que, mesmo essa arquitetura sendo excelente e usada por milhares de empresas, ela exige que você perca tempo gerenciando infraestrutura (provisionamento, saúde dos servidores, métricas de escala, patches).

---

## 2. How can Serverless help?
- Link [para as notas de estudos aqui](https://github.com/RegiMaria/aws-challenge/blob/main/00-AWS-Essential-Knowledge-Bagdes/01-Serverless-Knowledge/01-getting-into-the-seveless-mindset/02-how-can-serverless-help.md)
  
Aqui o curso vai mostrar os benefícios reais para o negócio e para o desenvolvedor. 
Você já entendeu a parte técnica, agora vai ver o impacto prático:

- Foco no valor de negócio: Em vez de gastar tempo configurando SO, rede e regras de Auto Scaling,
seu tempo vai 100% para a lógica que traz dinheiro/resultado.

- Redução drástica de TCO (Custo Total de Propriedade): Pagar $0 quando ninguém usa e não precisar
de uma equipe dedicada só para manter servidores vivos.

- Agilidade no lançamento (Time-to-Market): Ideias viram código e vão para produção muito mais rápido.

---

## 3. Managed services connected by functions

- [Link para as notas de estudos aqui](https://github.com/RegiMaria/aws-challenge/blob/main/00-AWS-Essential-Knowledge-Bagdes/01-Serverless-Knowledge/01-getting-into-the-seveless-mindset/03-managed-services-connected-by-functions.md)

Aqui vamos aprender que o Serverless não é só o Lambda.

- O Lambda é o "cérebro" que executa o código, mas ele se conecta com vários outros serviços gerenciados da AWS.

- Exemplo de ecossistema Serverless:

```Bash

API Gateway (Entrada)---> Lambda (Lógica)---> DynamoDB (Banco de Dados NoSQL) ---> S3 (Arquivos)
```
- Todos esses serviços escalam de 0 a infinito sem você gerenciar nenhum servidor em nenhum deles!

---

## 4. From hours to minutes with parallelization

[- Link para notas dos estudos aqui](https://github.com/RegiMaria/aws-challenge/blob/main/00-AWS-Essential-Knowledge-Bagdes/01-Serverless-Knowledge/01-getting-into-the-seveless-mindset/04-parallelization-from-hours-to-minutes.md)

Vamos relembrar nossa conversa sobre Concorrência e Paralelismo do tópico 1.
Esse tópico vai aprofundar exatamente nisso!

- Mostra como o Lambda permite pegar tarefas gigantes (como processar 100.000 imagens
ou converter milhares de arquivos) e dividir em milhares de Lambdas rodando ao mesmo tempo.

- O que antes levava horas em uma máquina EC2 processando um por um, no Serverless leva minutos 
ou segundos porque roda tudo em paralelo.

---

## 5. More environments, more innovations

-Link para nots dos estudos aqui

- Como é muito barato e fácil criar recursos Serverless 
(você só paga quando usa), você pode criar múltiplos ambientes de teste
(dev, staging, teste-feature) sem pagar nada a mais por infraestrutura parada.

- Isso permite que a equipe teste mais ideias, erre rápido sem custo alto e inove muito mais.

## Certificado dessa trilha


<div align="center">

<img src="https://github.com/user-attachments/assets/49f253a9-a9ab-42cf-8ae9-eaaadcdc928b" width="420" alt="AWS Logo"/>

</div>


