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
Configuramo um de banco de dados para persistir os dados.


## Mas o que acontece quando você começa a receber mais usuários e mais tráfego?
