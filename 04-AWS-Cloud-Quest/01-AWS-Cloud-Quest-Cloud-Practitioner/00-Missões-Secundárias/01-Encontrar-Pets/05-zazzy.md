# 🐱 Zazzy - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/534c0427-1e49-49e5-be8f-c24887886e92" width="250" alt="Zazzy Pet" />
</p>

- **Espécie:** Gato (*Cat*)
- **Domínio AWS:** Faturamento, Preços e Gestão (*Billing, Pricing & Management*)
- **Tópico Principal:** Planos de Suporte AWS e AWS Trusted Advisor
- **Total de Quizzes:** 2 (Expandido para 5 para estudo prático)
- **Status:** Adotada ✅

---

### Quiz 1: Planos de Suporte e Tempo de Resposta para Eventos Críticos
**Enunciado:** Uma empresa com aplicações críticas de negócios precisa do plano de suporte da AWS que ofereça atendimento técnico 24x7 por telefone, chat e e-mail, e garanta um tempo de resposta inferior a 15 minutos para falhas críticas de produção (*business-critical system down*). Qual é o plano de suporte mínimo necessário?

- [ ] **A)** AWS Basic Support
- [ ] **B)** AWS Developer Support
- [ ] **C)** AWS Business Support
- [x] **D)** AWS Enterprise Support

> **Por que esta é a resposta correta?**
> * O **AWS Enterprise Support** é o único plano que garante tempo de resposta menor que 15 minutos para eventos críticos de sistemas indisponíveis (*business-critical* ou *mission-critical*), além de incluir um Gerente Técnico de Conta (*TAM - Technical Account Manager*) dedicado.
> * *Incorretas:* O Developer Support cobre apenas horário comercial e e-mail; o Business Support garante resposta em até 1 hora para falhas do sistema.
>
> 📖 **Documentação:** [Comparação dos Planos de Suporte da AWS](https://aws.amazon.com/pt/premiumsupport/plans/)

---

### Quiz 2: Otimização e Boas Práticas com AWS Trusted Advisor
**Enunciado:** Um engenheiro de DevOps quer automatizar a verificação do ambiente AWS para identificar portas de grupos de segurança abertas desnecessariamente para a internet, instâncias EC2 ociosas e buckets S3 públicos. Qual ferramenta fornece esses relatórios automatizados de boas práticas?

- [x] **A)** AWS Trusted Advisor
- [ ] **B)** AWS Health Dashboard
- [ ] **C)** AWS Systems Manager
- [ ] **D)** AWS Control Tower

> **Por que esta é a resposta correta?**
> * O **AWS Trusted Advisor** analisa o ambiente em tempo real e fornece recomendações em 5 pilares fundamentais: Otimização de Custos, Desempenho, Segurança, Tolerância a Falhas e Limites de Serviço (*Service Quotas*).
>
> 📖 **Documentação:** [O que é o AWS Trusted Advisor?](https://docs.aws.amazon.com/pt_br/awssupport/latest/user/trusted-advisor.html)

---

### Quiz 3: Gerente Técnico de Conta (TAM)
**Enunciado:** Uma grande organização precisa de um especialista técnico dedicado da AWS para orientar a equipe em revisões de arquitetura, gerenciar chamados de suporte prioritários e coordenar o planejamento de eventos de pico de tráfego. Qual recurso está incluído exclusivamente nos planos de suporte *Enterprise*?

- [ ] **A)** AWS Partner Network Manager
- [ ] **B)** Concierge de Suporte para todos os planos
- [x] **C)** Gerente Técnico de Conta (*Technical Account Manager - TAM*)
- [ ] **D)** Agente de Atendimento ao Cliente do Plano Básico

> **Por que esta é a resposta correta?**
> * O **TAM** é o ponto de contato primário dedicado para clientes dos planos *Enterprise Support* e *Enterprise On-Ramp*. Ele fornece orientação proativa, ajuda na arquitetura de soluções e atua como defensor do cliente dentro da AWS.
>
> 📖 **Documentação:** [Função do Technical Account Manager (TAM)](https://aws.amazon.com/pt/premiumsupport/plans/enterprise/)

---

### Quiz 4: Monitoramento da Saúde dos Serviços AWS
**Enunciado:** Uma equipe de operações percebeu instabilidade numa aplicação e deseja verificar se há algum evento de degradação global nos serviços da AWS ou se o problema afeta especificamente os recursos da sua própria conta. Qual ferramenta deve ser consultada para ver o status personalizado da conta?

- [ ] **A)** AWS CloudTrail
- [x] **B)** AWS Health Dashboard (*Personal Health Dashboard*)
- [ ] **C)** AWS Trusted Advisor
- [ ] **D)** AWS Service Catalog

> **Por que esta é a resposta correta?**
> * O **AWS Health Dashboard** fornece visualizações personalizadas do estado de funcionamento dos serviços da AWS que estão a ser utilizados diretamente pela sua conta, enviando alertas em caso de manutenções programadas ou interrupções específicas de recursos.
>
> 📖 **Documentação:** [Guia do Usuário do AWS Health Dashboard](https://docs.aws.amazon.com/pt_br/health/latest/ug/what-is-aws-health.html)

---

### Quiz 5: Atendimento ao Cliente no Plano Gratuito (Basic Support)
**Enunciado:** Um utilizador possui apenas o plano de suporte gratuito (**AWS Basic Support**). Quais tipos de suporte e recursos estão disponíveis para esse utilizador sem custos adicionais? (Selecione DOIS)

- [x] **A)** Acesso 24x7 ao atendimento ao cliente para dúvidas sobre faturamento e suporte a cotas de conta.
- [x] **B)** Acesso à documentação oficial, fóruns da comunidade e Whitepapers da AWS.
- [ ] **C)** Abertura de chamados para suporte técnico 24x7 com engenheiros de software.
- [ ] **D)** Análise ilimitada e completa de todas as verificações do AWS Trusted Advisor.

> **Por que estas são as respostas corretas?**
> * O **Basic Support** é gratuito para todas as contas e inclui suporte administrativo/faturamento (Billing), acesso a documentos técnicos e verificações core/básicas de segurança e limites do Trusted Advisor. O suporte técnico direto a código/arquitetura exige planos pagos.
>
> 📖 **Documentação:** [Recursos do AWS Basic Support](https://aws.amazon.com/pt/premiumsupport/plans/basic/)
