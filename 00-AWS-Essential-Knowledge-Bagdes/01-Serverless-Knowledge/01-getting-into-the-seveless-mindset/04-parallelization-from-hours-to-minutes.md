<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" width="220" alt="AWS Logo"/>

#  04 - From Hours to Minutes with Parallelization

</div>

Neste capítulo (From Hours to Minutes with Parallelization), aprendemos como o modelo Serverless
transforma tarefas computacionais demoradas em execuções ultra-rápidas ao democratizar o paralelismo massivo.

## Aqui estão os principais aprendizados e sacadas deste tópico:

**1. A Matemática da Troca: Do Sequencial para o Paralelo**

No modelo tradicional com servidores, realizar um processamento muito massivo em paralelo exigia provisionar
milhares de instâncias, o que era caro, complexo de configurar e gerenciar.

Graças ao pilar do "nunca pagar por ociosidade" do AWS Lambda:

- Processamento Sequencial: 1 tarefa de 1 hora (3.600 segundos) em 1 máquina.

- Processamento Paralelo: 360 tarefas de 10 segundos rodando ao mesmo tempo em 360 funções Lambda.

> O Custo é Praticamente o Mesmo: Como o Lambda cobra estritamente por milissegundo de uso
>  360 x 10s = 3.600 de execução acumulada, executar 360 tarefas em 10 segundos custa praticamente
>  o mesmo do que rodar 1 tarefa por 1 hora, com a grande vantagem de entregar o resultado 360 vezes mais rápido!

---

**2. Estudo de Caso: Transcodificação de Vídeo (De Horas para Minutos)**

O vídeo apresentado mostrou um caso real de uma empresa de vídeo que mudou o paradigma do processamento:

- Abordagem Tradicional: Um arquivo de vídeo grande era processado do início ao fim de forma sequencial
em um único servidor. Demorava de 4 a 6 horas.

- Abordagem Serverless Paralela:

1. O vídeo original é enviado para o Amazon S3.

2. A aplicação faz requisições por intervalo (ranged gets) no S3 para quebrar o vídeo em centenas de pequenos trechos.

3. Múltiplas funções Lambda transcodificam esses trechos simultaneamente em paralelo.

4. Uma função Lambda final junta (merge) todos os trechos transcodificados de volta.

**Resultados Obtidos pelo Cliente:**

- Tempo de processamento: Reduzido de 4–6 horas para apenas 10 minutos.

- Custo de infraestrutura: Reduzido de US$ 60.000/mês para US$ 6.000/mês (uma economia de 10 vezes!).

---

**3. Computação de Alta Performance (HPC) ao Alcance de Todos**

O capítulo menciona o projeto open-source PyWren:

- Permite disparar tarefas massivas de processamento de dados e simulações matemáticas em Python
a partir do seu próprio computador, delegando o trabalho pesado para milhares de Lambdas em segundo plano.

- Benchmark impressionante: O projeto atingiu 40 Teraflops de capacidade computacional e taxa de download no S3 de 80 GB/s.

- Conclusão: Historicamente, alcançar essa performance exigia um supercomputador ou cluster HPC milionário.
No Serverless, qualquer pessoa com uma conta na AWS tem acesso a esse nível de poder computacional em minutos
e paga apenas pelos segundos utilizados.

---

Exemplo do vídeo


Este cliente desenvolveu um mecanismo para transcodificar segmentos individuais de um arquivo maior em paralelo
e juntar as partes novamente no backend. O Amazon S3 é usado como uma área de preparação (staging area) para 
enviar os arquivos de origem, e eles utilizam range gets dentro do Amazon S3 para extrair subsegmentos 
específicos de cada arquivo de vídeo individual. Assim, eles conseguem transcodificar cada pequena fatia
de um arquivo em paralelo. Em seguida, eles pegam essas fatias individuais transcodificadas, 
colocam-nas em um bucket intermediário e executam outra função Lambda para mesclar as partes.

---

<img width="800" height="350" alt="Image" src="https://github.com/user-attachments/assets/b1cfa763-dc33-4db1-8acf-9a0cd692f3c1" />

---

Resumo do aprendizado

| Métrica / Visão | Processamento Tradicional (EC2) | Processamento Serverless (Lambda) |
| --- | --- | --- |
| Abordagem de Execução | Sequencial (uma tarefa pesada por vez) | Paralela em massa (divide em centenas de subtarefas) |
| Tempo de Resposta | Horas | Minutos ou segundos |
| Gestão de Infraestrutura | Provisionar, limpar e desligar clusters | Automática pela AWS (cria e destrói os contêineres) |
| Barreira Financeira | Alta (paga pela infraestrutura ociosa) | Nenhuma (paga só pelo tempo exato de execução) |

