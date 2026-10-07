# 🐕 Golden Nugget - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/853d6635-6a6e-4a37-8eca-a473e1ba7a19" width="250" alt="Golden Nugget Pet" />
</p>

- **Espécie:** Cachorro (*Golden Retriever*)
- **Domínio AWS:** Faturamento, Preços e Gestão (*Billing, Pricing & Management*)
- **Tópico Principal:** Gestão de Custos, Orçamentos e Alocação Financeira
- **Total de Quizzes:** 1 (Expandido para 5 para estudo prático)
- **Status:** Adotado ✅

---

### Quiz 1: Visualização e Análise Histórica de Custos
**Enunciado:** Um gerente de TI precisa de uma ferramenta gráfica que permita visualizar, analisar e projetar os custos e o uso de recursos da AWS ao longo do tempo, identificando tendências de gastos por serviço nos últimos 12 meses. Qual ferramenta atende a esse requisito?

- [ ] **A)** AWS Trusted Advisor
- [x] **B)** AWS Cost Explorer
- [ ] **C)** AWS Cost & Usage Report (CUR)
- [ ] **D)** AWS Service Catalog

> **Por que esta é a resposta correta?**
> * O **AWS Cost Explorer** possui uma interface gráfica intuitiva que permite criar relatórios personalizados, visualizar custos passados de até 12 meses, analisar padrões de uso e até prever gastos estimados para os próximos 12 meses.
> * *Incorretas:* O AWS Trusted Advisor oferece verificações de boas práticas; o Cost & Usage Report gera arquivos brutos detalhados em formato CSV/Parquet para análise de dados e BI.
>
> 📖 **Documentação:** [O que é o AWS Cost Explorer?](https://docs.aws.amazon.com/pt_br/cost-management/latest/userguide/ce-whatis.html)

---

### Quiz 2: Alertas Preventivos de Faturamento
**Enunciado:** Uma empresa quer garantir que seus gastos com instâncias Amazon EC2 não ultrapassem o limite orçamentário mensal de US$ 1.000. Ela precisa enviar uma notificação por e-mail para a equipe financeira quando os custos reais ou projetados atingirem 80% desse valor. Qual serviço deve ser configurado?

- [ ] **A)** AWS CloudTrail
- [x] **B)** AWS Budgets
- [ ] **C)** AWS Config
- [ ] **D)** AWS Pricing Calculator

> **Por que esta é a resposta correta?**
> * O **AWS Budgets** permite definir orçamentos personalizados para custos ou uso e disparar alertas automáticos (via e-mail ou SNS) quando os gastos reais ou previstos (*forecasted*) ultrapassarem os limites percentuais ou de valor absoluto estabelecidos.
>
> 📖 **Documentação:** [Gerenciando custos com o AWS Budgets](https://docs.aws.amazon.com/pt_br/cost-management/latest/userguide/budgets-managing-costs.html)

---

### Quiz 3: Estimativa de Custos Antes da Implantação
**Enunciado:** Um arquiteto de soluções está projetando uma nova arquitetura para a empresa e precisa apresentar uma estimativa de custos aproximada para a diretoria **antes** de criar qualquer recurso dentro da conta AWS. Qual ferramenta pública deve ser utilizada?

- [ ] **A)** AWS Cost Explorer
- [ ] **B)** AWS Billing Dashboard
- [x] **C)** AWS Pricing Calculator
- [ ] **D)** AWS Compute Optimizer

> **Por que esta é a resposta correta?**
> * O **AWS Pricing Calculator** é uma ferramenta pública baseada na web que permite modelar soluções, estimar custos mensais de serviços AWS com base nas configurações desejadas (como tipo de máquina, armazenamento e tráfego) sem a necessidade de criar uma conta ou provisionar recursos.
>
> 📖 **Documentação:** [Guia do Usuário do AWS Pricing Calculator](https://docs.aws.amazon.com/pt_br/pricing-calculator/latest/userguide/what-is-pricing-calculator.html)

---

### Quiz 4: Categorização de Gastos por Centro de Custo
**Enunciado:** Para organizar a fatura mensal, o departamento de contabilidade exige que todos os custos da AWS sejam divididos claramente por projeto e departamento (ex: `Projeto: Alpha`, `Departamento: Marketing`). Qual mecanismo permite associar esses metadados aos recursos para relatórios de faturamento?

- [x] **A)** Tags de Alocação de Custos (*Cost Allocation Tags*)
- [ ] **B)** Grupos de Segurança (*Security Groups*)
- [ ] **C)** Organizações da AWS (*AWS Organizations*)
- [ ] **D)** Políticas de Controle de Serviços (*SCPs*)

> **Por que esta é a resposta correta?**
> * As **Cost Allocation Tags** são pares de chave-valor atribuídos aos recursos AWS. Uma vez ativadas no painel de faturamento, a AWS utiliza essas tags para categorizar e agrupar os custos detalhadamente nos relatórios contábeis.
>
> 📖 **Documentação:** [Uso de tags de alocação de custos](https://docs.aws.amazon.com/pt_br/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html)

---

### Quiz 5: Consolidação de Faturas em Múltiplas Contas
**Enunciado:** Uma corporação possui mais de 20 contas AWS separadas para diferentes times de desenvolvimento. A equipe financeira deseja pagar apenas **uma única fatura mensal** centralizada e aproveitar descontos por volume combinando o uso de todas as contas. Qual serviço habilita essa funcionalidade?

- [ ] **A)** AWS Cost Anomaly Detection
- [x] **B)** Faturamento Consolidado via AWS Organizations (*Consolidated Billing*)
- [ ] **C)** AWS License Manager
- [ ] **D)** AWS Service Catalog

> **Por que esta é a resposta correta?**
> * O recurso de **Faturamento Consolidado** do *AWS Organizations* consolida o pagamento de todas as contas vinculadas em uma única conta principal (*Management Account*), agregando o consumo de todos os recursos para alcançar faixas de desconto por volume mais altas.
>
> 📖 **Documentação:** [Faturamento consolidado para o AWS Organizations](https://docs.aws.amazon.com/pt_br/organizations/latest/userguide/orgs_manage_accounts_consolidated-billing.html)
