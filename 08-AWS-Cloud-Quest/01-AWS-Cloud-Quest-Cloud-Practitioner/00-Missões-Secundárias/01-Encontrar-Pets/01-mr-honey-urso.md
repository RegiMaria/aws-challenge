# 🐻 Mr. Honey - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/3c855b52-2367-425e-a3fc-2525a4e74ed3" width="350" alt="Image" />
</p>




- **Espécie:** Urso Pardo (*Grizzly Bear*)
- **Domínio AWS:** Armazenamento e Gerenciamento de Dados (*Storage*)
- **Serviço Central:** Amazon Simple Storage Service (Amazon S3)
- **Total de Quizzes:** 6
- **Status:** Domesticado ✅
- **Conferir status:* [Perfil estudante AWS Skill Builder](https://skillsprofile.skillbuilder.aws/user/regileneataide/cloudquest)

---

### Quiz 1: Modelo Estrutural de Objetos
**Enunciado:** Um arquiteto de soluções precisa explicar a estrutura lógica de armazenamento do Amazon S3 para uma equipe recém-chegada à nuvem. Quais afirmações descrevem corretamente como os dados são contidos no serviço? (Selecione DUAS)
- [x] **A)** O Amazon S3 cria buckets em uma Região AWS que o usuário especifica.
- [ ] **B)** Os buckets S3 são replicados exclusivamente dentro de uma única Zona de Disponibilidade física.
- [x] **C)** Um bucket é um contêiner fundamental para objetos armazenados no Amazon S3.
- [ ] **D)** Por padrão, todo novo bucket criado é exposto com acesso público irrestrito à internet.

> **Por que esta é a resposta correta?**
> * Um bucket é o contêiner raiz de nível superior para objetos no S3 e sempre pertence a uma Região geográfica específica escolhida no momento da criação.
> * *Incorretas:* O S3 Standard replica dados automaticamente por pelo menos 3 Zonas de Disponibilidade (*AZs*), e novos buckets vêm com o recurso *S3 Block Public Access* ativo por padrão (estritamente privados).
>
> 📖 **Documentação:** [O que é o Amazon S3? - Guia do Usuário](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/userguide/Welcome.html)

---

### Quiz 2: Hospedagem de Conteúdo Estático de Baixo Custo
**Enunciado:** Uma empresa deseja publicar um portal institucional estático contendo arquivos HTML, CSS, JavaScript, imagens e vídeos. O objetivo primordial é o menor custo operacional e financeiro possível, sem gerenciamento de servidores web. Qual arquitetura atende a esses requisitos?
- [x] **A)** Armazenar os arquivos em um bucket Amazon S3 e habilitar o recurso de Hospedagem Estática de Site (*Static Website Hosting*).
- [ ] **B)** Provisionar uma instância Amazon EC2 t4g.nano com Apache HTTP Server configurado.
- [ ] **C)** Implementar uma aplicação conteinerizada no AWS Elastic Beanstalk.
- [ ] **D)** Criar um banco de dados NoSQL no Amazon DynamoDB para servir o código-fonte via API.

> **Por que esta é a resposta correta?**
> * O Amazon S3 permite hospedar sites estáticos diretamente a partir do bucket sem provisionar nem pagar por máquinas virtuais ativas 24 horas por dia. Paga-se apenas pelo volume armazenado e pela transferência de dados consumida.
> * *Incorretas:* EC2 e Elastic Beanstalk cobram por hora de computação e exigem manutenção de sistema operacional ou ambientes de execução desnecessários para arquivos estáticos.
>
> 📖 **Documentação:** [Hospedagem de um site estático usando o Amazon S3](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/userguide/WebsiteHosting.html)

---

### Quiz 3: Limites e Ferramental de Upload
**Enunciado:** Um engenheiro de dados precisa enviar um arquivo único de backup compactado de 250 GB para um bucket Amazon S3. Ele tenta realizar a operação diretamente pelo AWS Management Console no navegador, mas o upload falha. Qual é o motivo e a solução recomendada pela AWS?
- [ ] **A)** O bucket atingiu a cota máxima de volume; deve-se criar um novo bucket na mesma região.
- [ ] **B)** O arquivo excede a capacidade máxima suportada pelo S3 para um único objeto (que é de 100 GB).
- [x] **C)** O console web impõe um limite máximo de 160 GB por upload; para arquivos maiores, deve-se usar a AWS CLI, SDKs ou API REST com Multipart Upload.
- [ ] **D)** Arquivos acima de 100 GB exigem obrigatoriamente a contratação do serviço AWS Direct Connect.

> **Por que esta é a resposta correta?**
> * O AWS Management Console suporta uploads de no máximo 160 GB para um único arquivo. Para arquivos maiores (o S3 suporta até 5 TB por objeto), a AWS exige o uso de ferramentas programáticas como a AWS CLI ou SDKs, que dividem o arquivo em partes via *Multipart Upload*.
>
> 📖 **Documentação:** [Carregar um objeto usando upload fracionado (Multipart)](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/userguide/mpuoverview.html)

---

### Quiz 4: Capacidade Escalar e Limites de Armazenamento
**Enunciado:** Uma startup em rápido crescimento está preocupada com o número máximo de registros e imagens que poderá salvar dentro de um único bucket S3 nos próximos cinco anos. Qual é o limite de quantidade de objetos que um bucket S3 pode comportar?
- [ ] **A)** 10 milhões de objetos por bucket.
- [ ] **B)** 5 TB divididos pela quantidade média do arquivo.
- [x] **C)** Não há limite; a capacidade de contagem de objetos é virtualmente infinita.
- [ ] **D)** 100.000 objetos por partição física.

> **Por que esta é a resposta correta?**
> * Não existe cota para a quantidade de objetos nem para o volume total de gigabytes armazenados dentro de um bucket. O único limite existente de tamanho aplica-se a cada arquivo individual (máximo de 5 TB por objeto).
>
> 📖 **Documentação:** [Regras e limites de buckets do Amazon S3](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/userguide/BucketRestrictions.html)

---

### Quiz 5: Automação de Custos com Políticas de Ciclo de Vida
**Enunciado:** Uma organização armazena logs de auditoria no S3 Standard. Esses logs devem ficar disponíveis para acesso imediato durante os primeiros 30 dias. Entre 31 e 90 dias, o acesso torna-se infrequente. Após 90 dias, os dados devem ser arquivados para conformidade regulatória por 5 anos gastando o mínimo possível. Qual recurso executa essas transições automaticamente de acordo com as regras definidas pelo usuário?
- [ ] **A)** S3 Storage Lens.
- [x] **B)** Regras de Ciclo de Vida do S3 (*S3 Lifecycle*).
- [ ] **C)** AWS CloudWatch Synthetics.
- [ ] **D)** S3 Cross-Region Replication (CRR).

> **Por que esta é a resposta correta?**
> * O *S3 Lifecycle Management* permite definir ações de transição com base na idade dos arquivos, migrando os objetos automaticamente (por exemplo: S3 Standard ➔ S3 Standard-IA aos 30 dias ➔ S3 Glacier Flexible Retrieval aos 90 dias ➔ Exclusão definitiva após 5 anos).
>
> 📖 **Documentação:** [Gerenciamento do ciclo de vida de objetos](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)

---

### Quiz 6: Categorização e Gestão de Metadados
**Enunciado:** Um administrador de nuvem precisa atribuir metadados personalizados a objetos e buckets para fins de relatórios de faturamento detalhados por centro de custo e projeto. Qual mecanismo de chave-valor da AWS deve ser empregado para essa finalidade?
- [x] **A)** Alocação de Tags de Recursos (*Resource Tags*).
- [ ] **B)** Chaves de Criptografia KMS (*KMS Key Aliases*).
- [ ] **C)** Identificadores de ETag de Objeto.
- [ ] **D)** Prefixo de URL estática.

> **Por que esta é a resposta correta?**
> * As Tags são pares de chave-valor (`Key: Value`) configuráveis em buckets e objetos. Elas são amplamente utilizadas para categorizar custos nos relatórios do *AWS Cost Allocation Tags*, além de permitirem controle de acesso via políticas do IAM (*ABAC*).
>
> 📖 **Documentação:** [Categorizar buckets com tags de alocação de custos](https://docs.aws.amazon.com/pt_br/AmazonS3/latest/userguide/CostAllocTags.html)
