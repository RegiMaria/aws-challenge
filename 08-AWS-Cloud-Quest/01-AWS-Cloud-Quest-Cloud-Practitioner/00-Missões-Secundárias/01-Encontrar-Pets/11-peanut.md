# 🐴 Peanut - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/d9f822cc-6e8a-437b-b0cf-e398e79c2766" width="250" alt="Peanut Pet" />
</p>

- **Espécie:** Cavalo (*Horse*)
- **Domínio AWS:** Tecnologia e Serviços em Nuvem (*Cloud Technology & Services*)
- **Tópico Principal:** Escalabilidade, Elasticidade e Alta Disponibilidade (Auto Scaling & Elastic Load Balancing)
- **Total de Quizzes:** 6
- **Status:** Adotado ✅

---

### Quiz 1: Tipos de Balanceadores de Carga (ALB vs. NLB)
**Enunciado:** Uma empresa precisa implantar um balanceador de carga para rotear tráfego HTTP/HTTPS baseado na URL da requisição (ex: `/api` para um grupo de instâncias e `/images` para outro). Qual tipo de Elastic Load Balancer (ELB) deve ser utilizado?

- [x] **A)** Application Load Balancer (ALB)
- [ ] **B)** Network Load Balancer (NLB)
- [ ] **C)** Gateway Load Balancer (GWLB)
- [ ] **D)** Classic Load Balancer (CLB)

> **Por que esta é a resposta correta?**
> * O **Application Load Balancer (ALB)** opera na Camada 7 do modelo OSI (Camada de Aplicação). Ele suporta recursos avançados de roteamento baseados em conteúdo, como caminhos de URL (*path-based*), cabeçalhos HTTP e nomes de host.
> * *Incorretas:* O Network Load Balancer (NLB) opera na Camada 4 (TCP/UDP) focado em baixíssima latência e IPs estáticos, mas não inspeciona URLs HTTP.
>
> 📖 **Documentação:** [O que é um Application Load Balancer?](https://docs.aws.amazon.com/pt_br/elasticloadbalancing/latest/application/introduction.html)

---

### Quiz 2: Alta Performance em Camada de Rede (Network Load Balancer - NLB)
**Enunciado:** Uma aplicação financeira de alta frequência precisa processar milhões de solicitações por segundo via protocolo TCP/UDP mantendo latências de sub-milisegundo e exigindo um endereço IP estático fixo para cada Zona de Disponibilidade. Qual balanceador de carga atende a esse requisito?

- [ ] **A)** Application Load Balancer (ALB)
- [x] **B)** Network Load Balancer (NLB)
- [ ] **C)** AWS Transit Gateway
- [ ] **D)** Amazon Route 53

> **Por que esta é a resposta correta?**
> * O **Network Load Balancer (NLB)** funciona na Camada 4 do modelo OSI. Ele é capaz de lidar com milhões de requisições por segundo, com latências extremamente baixas, e atribui um endereço IP estático por Zona de Disponibilidade ativada.
>
> 📖 **Documentação:** [O que é um Network Load Balancer?](https://docs.aws.amazon.com/pt_br/elasticloadbalancing/latest/network/introduction.html)

---

### Quiz 3: Verificações de Integridade (Health Checks)
**Enunciado:** Como o Elastic Load Balancing (ELB) garante que os usuários finais não sejam direcionados para instâncias EC2 que falharam ou deixaram de responder?

- [ ] **A)** Reiniciando a instância EC2 automaticamente após cada requisição.
- [x] **B)** Executando *Health Checks* periódicos e roteando o tráfego apenas para instâncias classificadas como saudáveis (*Healthy*).
- [ ] **C)** Redirecionando todo o tráfego diretamente para o Amazon S3 quando há falhas.
- [ ] **D)** Excluindo o grupo de segurança associado à instância com defeito.

> **Por que esta é a resposta correta?**
> * O ELB realiza **verificações de integridade (Health Checks)** contínuas nos alvos registrados (*Target Groups*). Se um alvo não responder às requisições do teste dentro do tempo estipulado, o balanceador de carga para de enviar tráfego para ele até que volte a responder corretamente.
>
> 📖 **Documentação:** [Health checks para seus grupos de alvos](https://docs.aws.amazon.com/pt_br/elasticloadbalancing/latest/application/target-group-health-checks.html)

---

### Quiz 4: Politicas de Escalonamento Automático (Target Tracking Scaling)
**Enunciado:** Um engenheiro deseja que o Auto Scaling adicione ou remova instâncias EC2 dinamicamente para manter a utilização média de CPU do grupo exatamente em 60%. Qual tipo de política de escalonamento é a mais simples e recomendada para essa finalidade?

- [x] **A)** Rastreamento de Meta (*Target Tracking Scaling Policy*)
- [ ] **B)** Escalonamento Baseado em Cronograma (*Scheduled Scaling*)
- [ ] **C)** Escalonamento Manual
- [ ] **D)** Escalonamento de Passo Simples (*Simple Step Scaling*)

> **Por que esta é a resposta correta?**
> * A política de **Target Tracking Scaling** funciona como um termostato: você define uma métrica de destino (ex: 60% de uso de CPU) e o Auto Scaling ajusta automaticamente o número de instâncias para manter a métrica perto desse valor alvo.
>
> 📖 **Documentação:** [Políticas de escalonamento de rastreamento de metas](https://docs.aws.amazon.com/pt_br/autoscaling/ec2/userguide/as-scaling-target-tracking.html)

---

### Quiz 5: Escalonamento Baseado em Agenda (Scheduled Scaling)
**Enunciado:** Uma rede de varejo sabe que todas as sextas-feiras, exatamente às 18:00, o tráfego de seu e-commerce aumenta em 300% devido a promoções semanais. Como preparar a infraestrutura com antecedência para evitar lentidão antes do pico de tráfego começar?

- [ ] **A)** Usar apenas escalonamento reativo baseado em alarme de CPU.
- [x] **B)** Configurar uma Política de Escalonamento Agendado (*Scheduled Scaling*) no EC2 Auto Scaling.
- [ ] **C)** Reiniciar manualmente o Application Load Balancer na sexta-feira às 17:59.
- [ ] **D)** Alterar o tipo da instância de `t3.micro` para `m5.large` semanalmente via script.

> **Por que esta é a resposta correta?**
> * O **Scheduled Scaling** permite escalar a capacidade da aplicação de acordo com datas e horários pré-determinados. É ideal para variações previsíveis de demanda em horários conhecidos.
>
> 📖 **Documentação:** [Escalonamento agendado para o Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/pt_br/autoscaling/ec2/userguide/ec2-auto-scaling-scheduled-scaling.html)

---

### Quiz 6: Manutenção de Capacidade Mínima, Desejada e Máxima
**Enunciado:** Um grupo de Auto Scaling foi configurado com os seguintes limites: `Capacidade Mínima = 2`, `Capacidade Desejada = 4`, e `Capacidade Máxima = 10`. O que acontece se duas instâncias do grupo sofrerem uma falha física de hardware simultaneamente?

- [ ] **A)** O grupo encerra todas as instâncias restantes por segurança.
- [ ] **B)** O grupo para de funcionar até que o administrador ajuste os parâmetros no console.
- [x] **C)** O Auto Scaling detecta a falha e lança automaticamente 2 novas instâncias para restaurar a capacidade desejada de 4 instâncias.
- [ ] **D)** O grupo é reduzido permanentemente para a capacidade mínima de 2 instâncias.

> **Por que esta é a resposta correta?**
> * O **Auto Scaling** monitora ativamente a integridade das instâncias. Se uma instância for encerrada ou falhar na verificação de integridade, o Auto Scaling lança uma nova instância automaticamente para manter o número definido na **Capacidade Desejada** (*Desired Capacity*).
>
> 📖 **Documentação:** [Capacidade do grupo de Auto Scaling](https://docs.aws.amazon.com/pt_br/autoscaling/ec2/userguide/as-capacity-limits.html)
