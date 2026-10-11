# 🐂 Bubbles - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/2dc3966f-364b-4f8e-99eb-804d03d11bd8" width="250" alt="Bubbles Pet" />
</p>

- **Espécie:** Boi / Touro (*Bull*)
- **Domínio AWS:** Conceitos de Nuvem (*Cloud Concepts*)
- **Tópico Principal:** Modelos de Implantação e Vantagens da Computação em Nuvem
- **Total de Quizzes:** 1 (Expandido para 5 para estudo prático)
- **Status:** Adotado ✅

---

### Quiz 1: Modelos de Implantação em Nuvem
**Enunciado:** Uma instituição financeira deseja migrar parte das suas aplicações para a AWS para obter escalabilidade, mas precisa manter bancos de dados com regulamentações rígidas de dados dentro do seu próprio centro de dados físico (*on-premises*). Qual modelo de implantação em nuvem essa empresa está a adotar?

- [ ] **A)** Nuvem Totalmente Pública (*All-in Cloud*)
- [x] **B)** Nuvem Híbrida (*Hybrid Cloud*)
- [ ] **C)** Nuvem Privada Virtual Isolada (*Private On-Premises Only*)
- [ ] **D)** Multicloud Descentralizada

> **Por que esta é a resposta correta?**
> * O modelo de **Nuvem Híbrida** conecta a infraestrutura existente *on-premises* (centro de dados próprio) aos recursos da nuvem pública (AWS), permitindo que aplicações e dados sejam partilhados entre eles com baixa latência e conformidade.
> * *Incorretas:* Nuvem Pública move 100% da infraestrutura para a AWS; Nuvem Privada mantém tudo no centro de dados local sem usufruir da nuvem pública.
>
> 📖 **Documentação:** [O que é computação em nuvem? - Modelos de Implantação](https://aws.amazon.com/pt/what-is-cloud-computing/)

---

### Quiz 2: Economia e Substituição de Custos (CapEx vs. OpEx)
**Enunciado:** Qual das alternativas representa uma vantagem econômica direta ao migrar infraestrutura física para a nuvem AWS?

- [x] **A)** Substituição de despesas de capital (*CapEx*) por despesas operacionais variáveis (*OpEx*).
- [ ] **B)** Aumento de custos fixos recorrentes com manutenção e arrefecimento de hardware físico.
- [ ] **C)** Obrigatoriedade de adivinhar a capacidade futura de servidores antes de iniciar um projeto.
- [ ] **D)** Pagamento de licenças fixas anuais independentemente do uso real dos recursos.

> **Por que esta é a resposta correta?**
> * Na computação em nuvem, elimina-se o investimento maciço inicial em servidores e centros de dados físicos (**CapEx** ou *Capital Expenditures*) e adota-se um modelo de pagamento pelo uso (**OpEx** ou *Operational Expenditures*), pagando apenas pelos recursos consumidos.
>
> 📖 **Documentação:** [Vantagens da Computação em Nuvem AWS](https://aws.amazon.com/pt/what-is-cloud-computing/benefits/)

---

### Quiz 3: Agilidade e Escalabilidade Global
**Enunciado:** Uma startup deseja lançar uma nova aplicação e disponibilizá-la para usuários na América do Norte, Europa e Ásia em questão de minutos, sem precisar assinar contratos de longo prazo com centros de dados regionais. Qual pilar dos conceitos de nuvem viabiliza esse cenário?

- [ ] **A)** Alta Disponibilidade Manual (*Manual High Availability*)
- [x] **B)** Implantação Global em Minutos (*Go Global in Minutes*)
- [ ] **C)** Cota Fixa de Provisionamento (*Fixed Capacity Provisioning*)
- [ ] **D)** Arquitetura Monolítica de Nuvem (*Monolithic Cloud Architecture*)

> **Por que esta é a resposta correta?**
> * Graças à infraestrutura global da AWS (composta por Regiões e Zonas de Disponibilidade), desenvolvedores podem implantar aplicações para usuários finais em múltiplos continentes com apenas alguns cliques ou linhas de código.
>
> 📖 **Documentação:** [Infraestrutura Global da AWS](https://aws.amazon.com/pt/about-aws/global-infrastructure/)

---

### Quiz 4: Economia de Escala
**Enunciado:** Como a AWS consegue oferecer redução contínua nos preços de seus serviços à medida que centenas de milhares de clientes utilizam a plataforma?

- [ ] **A)** Repassando o custo de manutenção física diretamente aos usuários finais.
- [ ] **B)** Cobrando taxas fixas de transferência de dados dentro da mesma Zona de Disponibilidade.
- [x] **C)** Beneficiando-se de massivas economias de escala (*Economies of Scale*).
- [ ] **D)** Exigindo pagamentos antecipados para todos os tipos de serviços sem exceção.

> **Por que esta é a resposta correta?**
> * Devido ao volume gigantesco de clientes concentrados na nuvem, a AWS consegue obter custos de aquisição e operação muito menores, traduzindo essa eficiência em preços mais baixos de utilização por hora/gigabyte para os clientes.
>
> 📖 **Documentação:** [6 Vantagens da Computação em Nuvem](https://docs.aws.amazon.com/pt_br/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html)

---

### Quiz 5: Eliminação de Suposição de Capacidade
**Enunciado:** Antes da nuvem, as empresas precisavam comprar servidores com base em estimativas de pico de tráfego, frequentemente pagando por recursos ociosos. Como a nuvem AWS resolve esse problema?

- [x] **A)** Permitindo provisionar apenas a capacidade necessária e escalar automaticamente conforme a demanda.
- [ ] **B)** Exigindo a contratação de hardware com no mínimo 12 meses de antecedência.
- [ ] **C)** Limitando o tráfego máximo de dados de acordo com o plano mensal contratado.
- [ ] **D)** Desligando automaticamente todas as instâncias no final do dia útil.

> **Por que esta é a resposta correta?**
> * Ao eliminar a necessidade de adivinhar a capacidade necessária (*Stop guessing capacity*), você reduz custos: pode iniciar com poucos recursos e utilizar ferramentas de auto-escalonamento para expandir ou reduzir a infraestrutura dinamicamente.
>
> 📖 **Documentação:** [Conceitos e Princípios da Nuvem AWS](https://docs.aws.amazon.com/pt_br/whitepapers/latest/aws-overview/concept-cloud-computing.html)
