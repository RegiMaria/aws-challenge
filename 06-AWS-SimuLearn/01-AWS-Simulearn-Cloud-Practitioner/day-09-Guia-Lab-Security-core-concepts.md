# AWS SimuLearn: Core Security Concepts

## 📋 Descrição

A bolsa de valores (stock exchange) quer restringir o acesso ao sistema dos seus engenheiros de suporte, permitindo apenas as ações necessárias para as funções específicas deles, reforçando os controles de segurança da empresa.

## 🗣️ Contexto do Cenário

**Diálogo com Morgana Key (cliente):**

- A empresa tem uma equipe de **Support Engineering** em crescimento, e precisa dar a **todos os membros da equipe as mesmas permissões de acesso**, de forma eficiente.
- **AWS Identity and Access Management (IAM)** é perfeito para isso: você cria **grupos** com permissões específicas e depois adiciona **usuários** a esses grupos, gerenciando o acesso de forma eficiente conforme a equipe cresce.
- Os engenheiros de suporte da empresa só precisam **visualizar** (view-only) recursos do **Amazon EC2** e do **Amazon RDS** - não precisam alterar nada.
- Não é necessário configurar permissões do zero para cada serviço da AWS: a AWS já disponibiliza **managed policies** (políticas gerenciadas) prontas para uso, incluindo políticas de acesso somente leitura para EC2 e RDS.
- Também é possível criar **políticas personalizadas (custom policies)**, caso as políticas prontas da AWS não atendam a uma necessidade específica.
- Os engenheiros de suporte precisam de acesso tanto ao **AWS Management Console** quanto a ferramentas de desenvolvedor (developer tools), e a empresa quer poder **adicioná-los diretamente à equipe de Support Engineering já no momento da criação da conta**.
- Ao criar um usuário, é possível conceder acesso ao Console, à AWS CLI e a ferramentas de desenvolvedor, e também **adicionar esse usuário diretamente a um grupo** onde as permissões já estão configuradas.

**Anotações técnicas da solução:**

1. Criar um grupo do IAM para os engenheiros de suporte.
2. Anexar uma política gerenciada (managed policy) de acesso somente leitura ao EC2 nesse grupo do IAM.
3. Criar um usuário e anexá-lo ao grupo.

---

### Solução proposta: AWS IAM (Groups + Managed Policies)

**Solicitação de solução:** implementar grupos do IAM com permissões de privilégio mínimo (least-privilege) para controlar o acesso dos engenheiros de suporte aos recursos da AWS.

---

## 🧩 Visão Geral da Solução

1. Esta solução demonstra a criação de um **grupo de usuários (user group)** no AWS IAM para gerenciar as permissões de vários usuários dentro de uma organização, de forma centralizada.
2. Um grupo do IAM chamado **`SupportEngineers`** funciona como um **contêiner de usuários IAM**, permitindo o gerenciamento centralizado de permissões através da **herança de permissões do grupo** - ou seja, todo usuário adicionado ao grupo passa a ter automaticamente as permissões dele.
3. Uma política de **acesso somente leitura (read-only)** para o **Amazon EC2** é anexada ao grupo `SupportEngineers`, limitando os membros do grupo a operações de leitura, como visualizar informações de instâncias - sem permitir criar, alterar ou encerrar recursos.
4. Um novo usuário do IAM, **`support-engineer-1`**, é criado e adicionado ao grupo `SupportEngineers`, recebendo automaticamente as permissões de leitura de EC2 do grupo.
5. Esse usuário consegue, com sucesso, **consultar informações de instâncias EC2** através de operações de leitura.
6. Já as tentativas de **encerrar (terminate)** uma instância resultam em **erro de permissão**, já que a política restringe as ações a operações somente leitura.
7. Ao adicionar, posteriormente, uma política de leitura para o **Amazon RDS** ao grupo, os membros do `SupportEngineers` passam a poder visualizar também informações do RDS - sem, novamente, conseguir alterar nada.

---

## 🎯 Prática Guiada - Conceito e Objetivos

**Conceito:** nesta prática guiada (*practice lab*), você vai:

- Criar um grupo do IAM e usuários.
- Anexar uma política gerenciada da AWS (AWS managed policy) ao grupo de usuários.

**Objetivos da Prática:**

- Criar um grupo do IAM e usuários.
- Anexar uma política gerenciada da AWS ao grupo de usuários.

---

## 🛠️ Prática Guiada - Passo a Passo

### 1. Introdução ao lab

Revise as tarefas do lab na seção de Conceito, clique em **Start Lab** ou **Open AWS Console** para começar, e siga as instruções com atenção, usando as setas para navegar entre os passos.

> ⚠️ Serviços da AWS não usados neste lab ficam desabilitados no ambiente. Além disso, as capacidades dos serviços usados são limitadas ao necessário para o lab.

### 2. Acessar o console do IAM

1. Na barra de busca do console, digite `iam`.
2. Nos resultados, em **Services**, clique em **IAM**.

> 💡 **Conceito:** o AWS Identity and Access Management (IAM) gerencia o acesso seguro aos serviços e recursos da AWS. **Não há cobrança adicional pelo IAM em si** - você paga apenas pelos serviços da AWS que os seus usuários acessam.

### 3. Iniciar a criação do grupo

1. No menu à esquerda, clique em **User groups**.
2. Na seção **User groups**, clique em **Create group**.

> 💡 **Conceito:** grupos do IAM definem permissões para **vários usuários de uma vez**. Todo usuário adicionado a um grupo **herda automaticamente** todas as permissões desse grupo.

### 4. Nomear o grupo

1. Em **User group name**, digite exatamente: `SupportEngineers`
   - ⚠️ O nome do grupo é **case sensitive** (diferencia maiúsculas de minúsculas) e precisa ser digitado exatamente como mostrado.

> 💡 **Conceito - regras dos user groups:**
> - Um grupo pode conter **vários usuários**.
> - Um usuário pode pertencer a **vários grupos** ao mesmo tempo.
> - Um grupo **não pode conter outro grupo** (não existe "grupo dentro de grupo" no IAM).

### 5. Anexar a política de leitura do EC2

1. Na caixa de busca **Attach permissions policies**, digite: `AmazonEC2ReadOnlyAccess` e pressione Enter.
2. Marque a caixa de seleção ao lado de **AmazonEC2ReadOnlyAccess**.
3. Em **Description**, revise a descrição da política.
4. Clique em **Create user group**.
   - Se aparecer um erro, verifique a grafia e a capitalização do nome do grupo. Você deve usar `SupportEngineers` exatamente como mostrado.

> 💡 **Conceito:** uma **policy** (política) define permissões na AWS. Quando um usuário ou role faz uma requisição, a AWS verifica as políticas associadas a ele para determinar o que ele **pode** ou **não pode** fazer. `AmazonEC2ReadOnlyAccess` é uma **managed policy** (política gerenciada) mantida pela própria AWS, pronta para uso, que concede apenas ações de visualização sobre o EC2.

### 6. Iniciar a criação do usuário

1. No menu à esquerda, clique em **Users**.
2. Na seção **Users**, clique em **Create user**.

> 💡 **Conceito:** **IAM users** são identidades criadas na AWS para representar pessoas ou aplicações. Cada usuário recebe um nome e credenciais próprias, que controlam o acesso dele à AWS.

### 7. Definir o nome do usuário e o acesso ao Console

1. Na etapa **Specify user details**, em **User name**, digite exatamente: `support-engineer-1`
   - ⚠️ O nome do usuário também é **case sensitive**.
2. Marque a caixa de seleção **Provide user access to the AWS Management Console**.
3. Em **Console password**, escolha **Custom password**.
4. Logo abaixo, no campo de texto, digite: `supportPassword!123`

> 💡 **Conceito - princípio do privilégio mínimo (least privilege):** a recomendação da AWS é dar aos usuários do IAM **apenas o mínimo de permissões** necessário para que eles façam o trabalho deles no AWS Management Console - nem mais, nem menos.

### 8. Adicionar o usuário ao grupo

1. Na etapa **Set permissions**, em **Permissions options**, escolha **Add user to group**.
2. Na seção **User groups**, marque a caixa de seleção de **SupportEngineers**.
3. Clique em **Next**.

> 💡 **Conceito:** um usuário pode receber permissões de duas formas:
> - **Entrando em grupos** (herdando as permissões do grupo) - é o que estamos fazendo aqui.
> - Através de **permissões anexadas diretamente** à conta do usuário (menos recomendado para gerenciar equipes, pois não escala bem).

### 9. Adicionar uma tag ao usuário

1. Na etapa **Review and create**, na seção **Tags**, clique em **Add new tag**.
2. Em **Key**, digite: `job-title`
3. Em **Value**, digite: `Support Engineer`
4. Clique em **Create user**.

> 💡 **Conceito:** **tags** são etiquetas (rótulos) para recursos da AWS que ajudam a: organizar recursos, pesquisar e filtrar, e gerenciar recursos de forma mais fácil. Cada tag tem uma **chave (key)** e um **valor (value)** opcional.

### 10. Salvar as credenciais do novo usuário

1. Na etapa **Retrieve password**, em **Console sign-in URL**, clique no ícone de copiar para copiar a URL fornecida e cole em um editor de texto de sua preferência.
   - Você também pode clicar em **Download .csv file**, que contém o link de login do Console e as credenciais.
2. Clique em **Return to users list**.
3. No pop-up (não exibido), clique em **Continue**.

### 11. Acessar os detalhes do usuário criado

1. Clique no nome de usuário `support-engineer-1`.

### 12. Revisar as credenciais de segurança do usuário

1. Clique na aba **Security credentials**.
2. Revise a seção **Multi-factor authentication (MFA)**.
   - MFA adiciona uma camada extra de segurança: mesmo que alguém descubra a senha, essa pessoa não consegue acessar a conta sem o segundo código de autenticação vindo do dispositivo MFA.
3. Revise a seção **Access keys**.
   - Access keys fornecem acesso **programático** à AWS. Elas são compostas por um **access key ID** e uma **secret access key** - as duas partes são necessárias para fazer requisições. São credenciais permanentes, usadas por usuários do IAM ou pelo usuário root.

> 💡 **Conceito - tarefas de segurança do administrador:**
> - **Testar novos usuários:** entrar (sign in) como os novos usuários para verificar o nível de acesso deles.
> - **Gerenciar credenciais:** atualizar regularmente as suas próprias senhas e access keys, e garantir que os usuários do IAM também troquem as credenciais deles periodicamente.
> - **Política de senha:** criar requisitos de senha forte, caso os usuários possam definir a própria senha.

### 13. Entrar no Console como o novo usuário

1. Em uma nova janela anônima/privada do navegador, cole na barra de endereço a URL de login do Console que você copiou no Passo 10, e pressione Enter.
   - Usar uma janela anônima/privada permite que você continue logado com suas credenciais originais na outra janela do navegador.
2. Em **IAM username**, digite: `support-engineer-1`
3. Em **Password**, digite: `supportPassword!123`
4. Clique em **Sign in**.

### 14. Acessar o console do EC2

1. Na barra de busca do console, digite `ec2`.
2. Nos resultados, em **Services**, clique em **EC2**.

> 💡 **Conceito:** o Amazon Elastic Compute Cloud (Amazon EC2) é um serviço web que fornece capacidade computacional redimensionável na nuvem, projetado para facilitar a computação em escala web para desenvolvedores.

### 15. Confirmar a região

1. Na barra de navegação superior, clique no seletor de região (Region selector) para expandir a lista suspensa.
2. Mantenha ou escolha **US East (N. Virginia) us-east-1**.

### 16. Acessar as instâncias em execução

1. No Dashboard do EC2, na seção **Resources**, clique em **Instances (running)**.

> 💡 **Conceito:** o Dashboard do Amazon EC2 mostra métricas sobre a quantidade de recursos por tipo (instâncias, volumes, snapshots, etc.).

### 17. Tentar encerrar uma instância

1. Na seção **Instances**, marque a caixa de seleção da instância **WebServer**.
2. Clique em **Instance state** para expandir a lista suspensa.
3. Escolha **Terminate (delete) instance**.

### 18. Confirmar a tentativa de encerramento

1. No pop-up, clique em **Terminate (delete)**.

> 💡 **Conceito:** você pode deletar - ou encerrar (terminate) - uma instância quando não precisa mais dela. Assim que o estado da instância muda para *shutting-down* ou *terminated*, você para de ser cobrado por ela. Porém, aqui esperamos que essa ação **falhe**, já que o usuário `support-engineer-1` só tem permissão de leitura.

### 19. Confirmar que a ação foi bloqueada

1. No alerta de erro, revise a mensagem **Failed to terminate**, indicando que não é possível deletar a instância devido às permissões do usuário.

> 💡 **Conceito - deny por padrão:** usuários só podem realizar as ações permitidas pelas políticas do IAM associadas a eles. Como um **principal** do IAM (usuário, grupo ou role) é **negado por padrão**, é preciso conceder explicitamente a permissão para cada ação - caso contrário, o acesso fica **implicitamente negado** (implicit deny). É exatamente por isso que a tentativa de terminar a instância falhou: nenhuma política concedeu essa permissão ao usuário.

### 20. Conclusão da Prática

Parabéns! Você concluiu a seção de Prática. O próximo passo é a seção **DIY** para completar a solução.

---

## 🧪 DIY (Do It Yourself)

### 🎯 Objetivos do DIY

1. Conceder ao grupo **`SupportEngineers`** acesso somente leitura (read-only) ao **Amazon RDS**.

### ✅ Método de Validação da Solução

O servidor de teste vai verificar se o grupo `SupportEngineers` tem, ao mesmo tempo, as políticas:

- `AmazonEC2ReadOnlyAccess` (já anexada na prática guiada)
- `AmazonRDSReadOnlyAccess` (a que você vai anexar agora no DIY)

> 💡 **Dica:** o DIY não pede para criar um grupo novo - é para **editar o grupo `SupportEngineers` que você já criou** na prática guiada, anexando uma segunda managed policy a ele. A lógica é praticamente idêntica ao Passo 5 da prática, só que dessa vez em um grupo já existente.

### 🔑 Informação necessária

- **Grupo já existente:** `SupportEngineers` (criado na prática guiada).
- **Política a anexar:** `AmazonRDSReadOnlyAccess`.

### 🛠️ Passo a Passo sugerido

**1. Acessar o console do IAM**

1. Na barra de busca do console, digite `iam` e clique em **IAM** nos resultados.

**2. Localizar o grupo existente**

1. No menu à esquerda, clique em **User groups**.
2. Clique no grupo `SupportEngineers`.

**3. Anexar a nova política gerenciada**

1. Na aba **Permissions**, clique em **Add permissions**.
2. Na lista suspensa, escolha **Attach policies**.
3. Na caixa de busca, digite: `AmazonRDSReadOnlyAccess` e pressione Enter.
4. Marque a caixa de seleção ao lado de **AmazonRDSReadOnlyAccess**.
5. Clique em **Add permissions** (ou **Attach policies**, dependendo da versão do console).

> 💡 **Conceito:** assim como `AmazonEC2ReadOnlyAccess`, a política `AmazonRDSReadOnlyAccess` é uma **managed policy** mantida pela AWS, que concede apenas ações de visualização (como `rds:Describe*`) sobre os recursos do Amazon RDS - sem permitir criar, modificar, apagar instâncias de banco de dados ou alterar configurações.

**4. Confirmar a configuração final**

1. Na aba **Permissions** do grupo `SupportEngineers`, confirme que **duas políticas** estão anexadas:
   - `AmazonEC2ReadOnlyAccess`
   - `AmazonRDSReadOnlyAccess`

> 💡 **Conceito:** como o usuário `support-engineer-1` **já pertence** ao grupo `SupportEngineers`, ele recebe automaticamente essa nova permissão assim que ela é anexada ao grupo - sem precisar editar o usuário individualmente. Essa é justamente a vantagem de gerenciar permissões por grupo: você atualiza o acesso de **toda a equipe de uma vez só**, em um único lugar.

---

## ✅ Conclusão

Com a conclusão da prática e do DIY, a bolsa de valores passa a ter:

- Um **grupo do IAM** (`SupportEngineers`) que centraliza o acesso de toda a equipe de Support Engineering, facilitando adicionar novos membros conforme a equipe cresce.
- Um usuário (`support-engineer-1`) com acesso ao **AWS Management Console**, adicionado diretamente ao grupo já no momento da criação da conta.
- Acesso de **somente leitura (read-only)** tanto ao **Amazon EC2** quanto ao **Amazon RDS**, através das managed policies `AmazonEC2ReadOnlyAccess` e `AmazonRDSReadOnlyAccess`.
- Aplicação prática do princípio de **least privilege**: os engenheiros de suporte conseguem visualizar instâncias e bancos de dados para investigar problemas, mas **não conseguem alterar ou encerrar** nenhum recurso - como comprovado na tentativa (bloqueada) de terminar a instância `WebServer`.
