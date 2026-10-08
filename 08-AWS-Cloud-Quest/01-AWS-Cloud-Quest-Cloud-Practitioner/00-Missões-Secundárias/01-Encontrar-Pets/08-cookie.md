# 🐕 Cookie - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/52cd36dc-7141-47fa-a11a-8fc3440e982c" width="250" alt="Cookie Pet" />
</p>

- **Espécie:** Cachorro (*Dobermann Preto*)
- **Domínio AWS:** Faturamento, Preços e Gestão (*Billing, Pricing & Management*)
- **Tópico Principal:** Monitorização, Auditoria e Observabilidade (Amazon CloudWatch & AWS CloudTrail)
- **Total de Quizzes:** 3 (Expandido para 10 para estudo prático)
- **Status:** Adotado ✅

---

### Quiz 1: Monitoramento de Desempenho e Métricas de Recursos (Amazon CloudWatch)
**Enunciado:** Um administrador de sistemas precisa monitorar a utilização de CPU de um grupo de instâncias EC2 e criar um alarme que envie uma notificação para a equipe de operações via e-mail caso a média de processamento passe de 80% por 5 minutos. Qual serviço deve ser utilizado?

- [ ] **A)** AWS CloudTrail
- [x] **B)** Amazon CloudWatch
- [ ] **C)** AWS Config
- [ ] **D)** AWS Health Dashboard

> **Por que esta é a resposta correta?**
> * O **Amazon CloudWatch** é a ferramenta de monitoramento e observabilidade da AWS que coleta métricas de desempenho (como uso de CPU, disco e rede) de recursos ativos, permitindo criar alarmes e ações automáticas com base em limites definidos.
> * *Incorretas:* O CloudTrail registra chamadas de API e chamadas de auditoria; o AWS Config monitora alterações na configuração dos recursos.
>
> 📖 **Documentação:** [O que é o Amazon CloudWatch?](https://docs.aws.amazon.com/pt_br/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)

---

### Quiz 2: Auditoria de Segurança e Rastreamento de APIs (AWS CloudTrail)
**Enunciado:** Após um incidente de segurança, o analista de redes precisa identificar **quem** apagou um bucket S3 de produção, **qual** endereço IP foi utilizado na requisição e **em qual** horário exato a chamada de API ocorreu. Qual serviço registra essa atividade?

- [ ] **A)** Amazon CloudWatch Logs
- [x] **B)** AWS CloudTrail
- [ ] **C)** AWS GuardDuty
- [ ] **D)** Amazon Inspector

> **Por que esta é a resposta correta?**
> * O **AWS CloudTrail** rastreia e registra continuamente as atividades da conta relativas a chamadas de API realizadas pelo console, SDKs ou linha de comando (CLI). Ele responde às perguntas de auditoria: "Quem fez o quê, onde e quando?".
>
> 📖 **Documentação:** [O que é o AWS CloudTrail?](https://docs.aws.amazon.com/pt_br/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)

---

### Quiz 3: Rastreamento Distribuído e Latência em Microsserviços (AWS X-Ray)
**Enunciado:** Uma equipe de desenvolvedores trabalha com uma aplicação baseada em microsserviços distribuídos em AWS Lambda, API Gateway e DynamoDB. A aplicação está apresentando gargalos de latência. Qual serviço ajuda a visualizar o mapa do sistema e rastrear requisições de ponta a ponta para identificar o ponto exato de lentidão?

- [ ] **A)** Amazon CloudWatch Synthetics
- [x] **B)** AWS X-Ray
- [ ] **C)** AWS CloudTrail
- [ ] **D)** AWS App Mesh

> **Por que esta é a resposta correta?**
> * O **AWS X-Ray** ajuda os desenvolvedores a analisar e depurar aplicações distribuídas e em arquitetura de microsserviços. Ele fornece um mapa visual das conexões da aplicação e rastreia requisições (*traces*) à medida que transitam pelos serviços subjacentes.
>
> 📖 **Documentação:** [O que é o AWS X-Ray?](https://docs.aws.amazon.com/pt_br/xray/latest/devguide/aws-xray.html)

---

### Quiz 4: Coleta de Logs no Nível de Sistema Operacional (CloudWatch Agent)
**Enunciado:** Por padrão, o CloudWatch coleta métricas de hipervisor das instâncias EC2 (como % CPU). No entanto, um administrador precisa monitorar o uso de **memória RAM** e o espaço livre no **disco rígido**, que são métricas internas do SO. Como viabilizar essa coleta?

- [ ] **A)** Habilitar o CloudWatch Detailed Monitoring no console do EC2.
- [x] **B)** Instalar e configurar o agente do CloudWatch (*Unified CloudWatch Agent*) dentro do SO da instância.
- [ ] **C)** Ativar as métricas do AWS CloudTrail Insights.
- [ ] **D)** Associar uma função do IAM com acesso ao S3 na instância.

> **Por que esta é a resposta correta?**
> * A AWS não lê a memória ou arquivos do SO do usuário por questões de privacidade e acesso. Para coletar métricas do sistema operacional (como RAM e espaço em disco), é necessário instalar o **CloudWatch Agent** dentro da instância.
>
> 📖 **Documentação:** [Coleta de métricas e logs com o agente do CloudWatch](https://docs.aws.amazon.com/pt_br/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html)

---

### Quiz 5: Detecção de Anomalias de Comportamento em APIs (CloudTrail Insights)
**Enunciado:** Uma empresa deseja identificar automaticamente picos incomuns ou anômalos de chamadas de API dentro de sua conta (por exemplo, um surto de lançamentos de instâncias EC2 fora do horário comercial) sem precisar criar regras manuais complexas. Qual recurso atende a esse objetivo?

- [x] **A)** AWS CloudTrail Insights
- [ ] **B)** CloudWatch Alarms
- [ ] **C)** AWS Security Hub
- [ ] **D)** Amazon EventBridge

> **Por que esta é a resposta correta?**
> * O **CloudTrail Insights** usa modelos de aprendizado de máquina para analisar continuamente os eventos de gerenciamento do CloudTrail e identificar comportamentos anômalos ou picos atípicos no volume de chamadas de API.
>
> 📖 **Documentação:** [Registrar eventos do CloudTrail Insights](https://docs.aws.amazon.com/pt_br/awscloudtrail/latest/userguide/logging-insights-events-with-cloudtrail.html)

---

### Quiz 6: Automação Orientada a Eventos em Tempo Real (Amazon EventBridge)
**Enunciado:** Sempre que um arquivo for carregado em um bucket S3 específico ou uma instância EC2 mudar para o estado `terminated`, um script em AWS Lambda deve ser acionado imediatamente para processar a alteração. Qual serviço funciona como um barramento de eventos (*Event Bus*) para rotear essas notificações?

- [ ] **A)** Amazon Simple Notification Service (SNS)
- [ ] **B)** Amazon Simple Queue Service (SQS)
- [x] **C)** Amazon EventBridge (anteriormente CloudWatch Events)
- [ ] **D)** AWS Step Functions

> **Por que esta é a resposta correta?**
> * O **Amazon EventBridge** é um barramento de eventos sem servidor (*serverless*) que conecta dados de suas aplicações, SaaS e serviços da AWS em tempo real, disparando destinos (como o AWS Lambda) quando regras de eventos coincidem.
>
> 📖 **Documentação:** [O que é o Amazon EventBridge?](https://docs.aws.amazon.com/pt_br/eventbridge/latest/userguide/eb-what-is.html)

---

### Quiz 7: Retenção e Integridade dos Logs de Auditoria
**Enunciado:** Para cumprir exigências legais, uma empresa precisa garantir que todos os logs gerados pelo AWS CloudTrail sejam salvos em um local seguro e protegidos contra modificações ou exclusões acidentais por um período de 7 anos. Qual a arquitetura recomendada?

- [ ] **A)** Armazenar os logs apenas no CloudWatch Logs com retenção padrão.
- [x] **B)** Configurar o CloudTrail para enviar os logs para um bucket S3 com *S3 Object Lock* ativado e verificação de integridade de arquivo.
- [ ] **C)** Fazer download semanal dos logs via CLI para o computador pessoal do administrador.
- [ ] **D)** Enviar os registros diretamente para uma tabela do DynamoDB.

> **Por que esta é a resposta correta?**
> * O CloudTrail integra-se nativamente com o **Amazon S3**, fornecendo *File Integrity Validation* para detectar se um log foi adulterado. O uso da funcionalidade *S3 Object Lock* em modo WORM (Write Once, Read Many) impede a alteração ou exclusão de logs durante o período estipulado.
>
> 📖 **Documentação:** [Validar a integridade de arquivos de log do CloudTrail](https://docs.aws.amazon.com/pt_br/awscloudtrail/latest/userguide/cloudtrail-log-file-validation-intro.html)

---

### Quiz 8: Painéis Visuais de Monitoramento (CloudWatch Dashboards)
**Enunciado:** A equipe de operações precisa manter uma tela exibindo em tempo real métricas de múltiplos serviços (status de banco RDS, tráfego de rede da VPC e uso de CPU de instâncias) de regiões AWS distintas em um único painel centralizado. Qual recurso oferece essa funcionalidade?

- [x] **A)** CloudWatch Dashboards
- [ ] **B)** AWS Management Console Home
- [ ] **C)** Amazon QuickSight
- [ ] **D)** AWS Systems Manager OpsCenter

> **Por que esta é a resposta correta?**
> * Os **CloudWatch Dashboards** são páginas personalizáveis no console do CloudWatch que permitem criar gráficos visuais de métricas em tempo real, inclusive combinando dados de diferentes serviços e Regiões da AWS na mesma visualização.
>
> 📖 **Documentação:** [Uso de painéis do Amazon CloudWatch](https://docs.aws.amazon.com/pt_br/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html)

---

### Quiz 9: Análise e Pesquisa de Logs em Texto (CloudWatch Logs Insights)
**Enunciado:** Um desenvolvedor precisa pesquisar rapidamente falhas de erro `HTTP 500` dentro de terabytes de logs de aplicações web acumulados no CloudWatch Logs. Qual recurso nativo permite executar consultas no estilo SQL diretamente sobre esses registros de log?

- [ ] **A)** Amazon Athena
- [ ] **B)** AWS CloudTrail Lake
- [x] **C)** CloudWatch Logs Insights
- [ ] **D)** Amazon OpenSearch Service

> **Por que esta é a resposta correta?**
> * O **CloudWatch Logs Insights** é uma funcionalidade interativa de análise de dados que permite executar consultas complexas e rápidas usando uma linguagem de consulta própria (com suporte a comandos de filtragem e agregação similares a SQL) diretamente sobre os grupos de logs armazenados.
>
> 📖 **Documentação:** [Análise de dados de log com o CloudWatch Logs Insights](https://docs.aws.amazon.com/pt_br/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)

---

### Quiz 10: Monitoramento Sintético de Aplicações (CloudWatch Synthetics)
**Enunciado:** Uma empresa quer testar e monitorar proativamente a disponibilidade e o tempo de carregamento de sua página web principal a cada 1 minuto, simulando a navegação de um usuário real mesmo que não haja tráfego no momento. Qual funcionalidade do CloudWatch executa essa verificação?

- [ ] **A)** CloudWatch Metric Streams
- [x] **B)** CloudWatch Synthetics (Canaries)
- [ ] **C)** AWS X-Ray Insights
- [ ] **D)** AWS Health Checks do Route 53

> **Por que esta é a resposta correta?**
> * O **CloudWatch Synthetics** permite criar scripts (*Canaries*) que rodam em intervalos regulares para simular as mesmas rotas e ações que os seus clientes realizam no site, permitindo descobrir problemas na aplicação antes que os usuários os relatem.
>
> 📖 **Documentação:** [Uso do monitoramento sintético do CloudWatch](https://docs.aws.amazon.com/pt_br/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html)
