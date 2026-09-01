# AWS SimuLearn: File Systems in the Cloud

## 📋 Descrição

A agência de modelagem de pets (a única da cidade) precisa de uma forma de compartilhar arquivos entre suas filiais sem precisar gerenciar infraestrutura física de armazenamento.

## 🗣️ Contexto do Cenário

A empresa abriu três novas filiais ao longo do último ano. Cada filial possui seu próprio servidor de imagens de pets, conectado a uma aplicação local de gerenciamento de clientes.

**Principais problemas identificados:**

- Cada servidor armazena imagens dos clientes (pets) junto com metadados informativos.
- Existe uma aplicação customizada para sincronizar os dados dos clientes entre as três filiais, mas o acesso às imagens é lento e inconsistente.
- A capacidade de armazenamento **não é igual** entre os servidores das filiais.
- A sincronização falha às vezes porque um servidor de filial pode ficar sem espaço de armazenamento.
- Todas as filiais acessam e atualizam os **mesmos arquivos**.
- Existem pastas para clientes VIP que devem ser acessadas **somente pela equipe de concierge** (permissões de arquivo/pasta).

**Necessidade:** uma solução que centralize o armazenamento de imagens e escale automaticamente, com controle de acesso por permissões.

### Solução proposta: Amazon EFS

Com base nos requisitos, o **Amazon Elastic File System (Amazon EFS)** foi indicado como a solução ideal:

- É um serviço de armazenamento de arquivos **serverless**, que gerencia capacidade e infraestrutura automaticamente.
- Permite criar unidades de rede compartilhadas (*shared network drives*), possibilitando que as filiais acessem as fotos dos clientes a partir de um local centralizado.
- Permite restringir o acesso com **permissões em nível de arquivo**.
- Oferece armazenamento em **escala de petabytes**, que cresce e diminui automaticamente conforme arquivos são adicionados ou removidos.
- É altamente disponível e projetado para **99.999999999% (onze noves) de durabilidade**, com os dados protegidos em múltiplas Zonas de Disponibilidade (Availability Zones).

**Objetivo do laboratório:** criar um sistema de arquivos no Amazon EFS onde todas as filiais possam acessar as fotos dos clientes pet a partir de um único local centralizado.

---

## 🧩 Visão Geral da Solução

**Solicitação de solução:** implementar o Amazon EFS para fornecer um sistema de arquivos totalmente gerenciado e escalável, que possa ser acessado por múltiplas filiais.

1. Essa solução simplifica a administração dos servidores web ao implementar um sistema de arquivos Amazon EFS centralizado para armazenamento compartilhado de conteúdo.
2. O sistema de arquivos EFS permite o acesso simultâneo por instâncias EC2 dentro da mesma VPC, facilitando o gerenciamento rápido de conteúdo dinâmico.
3. Cada Zona de Disponibilidade (AZ) contém um *mount target*, que fornece um endpoint de IP para que as instâncias se conectem ao sistema de arquivos.
4. Os servidores web montam o *mount target* de sua respectiva AZ como uma pasta local na instância.
5. As instâncias interagem com o sistema de arquivos EFS como se fosse um armazenamento local, e as alterações ficam visíveis para todas as instâncias autorizadas. O sistema de arquivos escala automaticamente conforme o volume de dados.
6. O sistema de arquivos comporta instâncias adicionais por meio da criação e associação de *mount targets* na AZ apropriada.

---

## 🎯 Prática Guiada - Conceito e Objetivos

**Conceito:** nesta prática guiada (*practice lab*), você vai:

- Lançar e configurar um sistema de arquivos Amazon EFS.
- Montar o sistema de arquivos em uma instância Amazon EC2.
- Conectar uma segunda instância EC2 ao mesmo sistema de arquivos.
- Compartilhar arquivos entre as duas instâncias EC2.

**Objetivos da Prática:**

- Lançar e configurar um sistema de arquivos Amazon EFS.
- Montar o sistema de arquivos em uma instância Amazon EC2.
- Conectar uma segunda instância EC2 ao mesmo sistema de arquivos.
- Compartilhar arquivos entre as duas instâncias EC2.

---

## Arquitetura

<img width="794" height="659" alt="Image" src="https://github.com/user-attachments/assets/25ca24d8-279d-40c6-9b86-8a2cb03ebbe3" />

--- 
## 🛠️ Prática Guiada - Passo a Passo

### 1. Introdução ao lab

Revise as tarefas do lab na seção de Conceito, clique em **Start Lab** ou **Open AWS Console** para começar, e siga as instruções usando as setas para navegar entre os passos.

> ⚠️ Serviços da AWS não usados neste lab ficam desabilitados no ambiente. Além disso, as capacidades dos serviços usados são limitadas ao necessário para o lab.

### 2. Acessar o console do EC2

Na barra de busca do console, digite `ec2` e clique em **EC2** nos resultados.

> 💡 **Conceito:** o Amazon EC2 fornece capacidade computacional escalável e sob demanda na nuvem AWS. Reduz custos de hardware e permite lançar quantas instâncias virtuais forem necessárias, configurando rede, segurança e armazenamento.

### 3. Revisar as instâncias existentes

No menu à esquerda, clique em **Instances** e revise os nomes das três instâncias disponíveis. Role a tela para a direita para ver todos os detalhes.

### 4. Revisar as Availability Zones (AZs)

Em **Availability Zone**, revise a AZ de cada instância. Em seguida, no menu à esquerda, em **Network & Security**, clique em **Security Groups**.

> 💡 **Conceito:** AZs são identificadas por uma letra após o código da Região (ex.: `us-east-1a`) e são conectadas por redes de fibra de baixa latência dentro da mesma Região.

### 5. Revisar o Security Group existente

Na seção **Security Groups**, revise o grupo `Web_Server_SG` (já vinculado aos servidores web). Clique em **Create security group**.

> 💡 **Conceito:** toda instância EC2 deve pertencer a pelo menos um security group, que controla o tráfego permitido para as instâncias daquele grupo.

### 6. Criar o Security Group do EFS

1. **Security group name:** `PetModels-EFS-1-SG`
2. **Description:** `Restrict access to web servers only.`
3. **VPC:** escolha a VPC `PetModels` (remova a VPC padrão clicando no X, se necessário).
4. Na seção **Inbound rules**, clique em **Add rule**.

> 💡 **Conceito:** security groups são vinculados a uma única VPC. Você pode associar um grupo a uma ou mais instâncias EC2, desde que estejam na mesma VPC do grupo.

### 7. Configurar a regra de entrada (NFS)

1. **Type:** `NFS`
2. **Source:** selecione o security group dos servidores web (`Web_Server_SG`)
3. Role até o final da página e clique em **Create security group**.

> 💡 **Conceito:** sistemas de arquivos Amazon EFS exigem uma regra de entrada NFS. Ao selecionar um security group como origem, todas as instâncias EC2 vinculadas a esse grupo terão acesso NFS ao sistema de arquivos.

### 8. Revisar o Security Group criado

Revise o nome do grupo e, na aba **Inbound rules**, confira a regra recém-criada.

> 💡 **Conceito:** alterações em regras de security groups têm efeito imediato. As regras são *stateful*: se o tráfego de entrada é permitido, o tráfego de saída correspondente é automaticamente liberado.

### 9. Conhecer o Amazon FSx (contexto)

Na barra de busca, digite `fsx` e clique em **FSx** nos resultados.

> 💡 **Conceito:** o Amazon FSx é um serviço totalmente gerenciado que fornece sistemas de arquivos de terceiros na nuvem AWS, eliminando a necessidade de gerenciar servidores e armazenamento de arquivos.

### 10. Revisar o console do FSx

Revise as informações e descrições na página inicial do console do Amazon FSx.

> 💡 **Conceito:** o Amazon FSx suporta protocolos padrão da indústria, com conectividade para usuários e aplicações Linux, Windows e macOS, além de baixa latência e alta taxa de transferência.

### 11. Acessar o console do EFS

Na barra de busca, digite `efs` e clique em **EFS** nos resultados.

> 💡 **Conceito:** o Amazon EFS é um sistema de arquivos serverless que você usa para compartilhar dados sem provisionar ou gerenciar armazenamento. Ele escala sob demanda, até petabytes de capacidade, sem interromper aplicações.

### 12. Criar o sistema de arquivos EFS

Na página inicial do console do EFS, clique em **Create file system**.

> 💡 **Conceito:** o Amazon EFS cria um sistema de arquivos compartilhado, disponível simultaneamente para múltiplas instâncias EC2.

### 13. Configurar nome e VPC

1. **Name:** `PetModels-EFS-1`
2. **Virtual Private Cloud (VPC):** escolha a VPC `PetModels`
3. Clique em **Customize**.

> 💡 **Conceito:** por padrão, instâncias EC2 devem estar na mesma VPC do sistema de arquivos EFS.

### 14. Configurações do sistema de arquivos

1. Em **File system type**, revise os tipos disponíveis.
2. Em **Automatic backups**, desmarque a opção **Enable automatic backups**.

> 💡 **Conceito:** você pode escolher entre as classes de armazenamento **Regional** (dados armazenados dentro e entre múltiplas AZs) ou **One Zone** (dados armazenados redundantemente em uma única AZ, com custo menor, para workloads que não exigem resiliência Multi-AZ).

### 15. Configurar o ciclo de vida (Lifecycle management)

1. **Transition into Infrequent Access (IA):** `None`
2. **Transition into Archive:** `None`

> 💡 **Conceito:** a classe **Infrequent Access** é indicada para arquivos de longa duração acessados raramente, de forma econômica. A classe **Archive** é indicada para dados acessados muito raramente - poucas vezes por ano ou menos.

### 16. Configurar o desempenho (Throughput)

1. Em **Performance settings**, **Throughput mode:** `Bursting`
2. Clique em **Next**.

> ⚠️ **Nota (console em português):** se o console da AWS estiver em português, os nomes exibidos podem ser: **Avançado**, **Intermitente**, **Elastic** e **Provisionado**. Isso pode confundir, pois a nomenclatura muda um pouco em relação à documentação em inglês.
>
> Segundo a documentação atual da AWS, os três modos reais de throughput são: **Elastic**, **Provisioned** e **Bursting**. Na tradução para português, **Bursting = Intermitente**.
>
> Portanto, para seguir exatamente o laboratório, selecione: ✅ **Intermitente**

### 17. Configurar o acesso à rede (Mount targets)

1. Na etapa **Network access**, em **Mount targets**, remova o subnet da AZ `us-east-1c` (**Remove**).
2. Remova o subnet da AZ `us-east-1b` (**Remove**).
3. Remova o security group da AZ `us-east-1a` clicando no **X** ao lado do grupo exibido.

> 💡 **Conceito:** após criar o sistema de arquivos EFS, você cria *mount targets* em cada subnet. O mount target permite a comunicação das instâncias EC2 daquele subnet. O Amazon EFS usa o protocolo NFSv4, e as instâncias que se conectam ao sistema de arquivos são clientes NFS.

### 18. Associar o Security Group ao Mount Target

1. Para a AZ `us-east-1a`, em **Security group**, escolha `PetModels-EFS-1-SG`.
2. Clique em **Next**.

> 💡 **Conceito:** ao associar seu security group customizado ao mount target, você controla de onde pode se originar o tráfego de entrada para o sistema de arquivos.

### 19. Política do sistema de arquivos

Na etapa **File system policy**, clique em **Next** (sem alterações).

### 20. Revisar e criar

Na etapa **Review and create**, role até o final da página e clique em **Create**.

### 21. Confirmar a criação

1. Revise a mensagem de sucesso.
   - Se aparecer um alerta de erro `iam:CreateServiceLinkedRole`, pode ignorar com segurança.
2. Em **File system ID**, anote o ID fornecido - **você vai precisar dele na seção DIY**.
3. Clique no nome do sistema de arquivos `PetModels-EFS-1`.

### 22. Confirmar disponibilidade e iniciar o Attach

1. Em **File system state**, confirme que o status é **Available** (se não estiver, aguarde um minuto e atualize a página).
2. Clique em **Attach**.

### 23. Copiar o comando de montagem

1. Na janela pop-up, mantenha a opção padrão **Mount via DNS**.
2. Em **Using the EFS mount helper**, clique no ícone de copiar para copiar o comando de montagem e cole-o em um editor de texto (você vai usá-lo nos próximos passos).
3. Clique em **Close**.

> 💡 **Conceito:** para configurar um mount do EFS em um sistema Amazon Linux, instale o EFS mount helper e depois execute o comando de mount.

### 24. Conectar-se à instância WebServer1

1. Navegue até o console do Amazon EC2 (**Instances**).
2. Selecione o checkbox de `WebServer1`.
3. Clique em **Connect**.

> 💡 **Conceito:** para se conectar a uma instância, o Amazon EC2 suporta SSH, Session Manager (recurso do AWS Systems Manager) ou Amazon EC2 Instance Connect.

### 25. Abrir sessão via Session Manager

1. Clique na aba **Session Manager**.
2. Clique em **Connect** (abre um terminal em uma nova aba do navegador - mantenha a aba original aberta).

> 💡 **Conceito:** o Session Manager fornece gerenciamento seguro e auditável de nós, sem necessidade de abrir portas de entrada, manter bastion hosts ou gerenciar chaves SSH.

### 26. Instalar o cliente do EFS (WebServer1)

No terminal:

```bash
sudo -i
sudo yum install -y amazon-efs-utils
```

> 💡 **Conceito:** o `amazon-efs-utils` é uma coleção open source de ferramentas do Amazon EFS, usada para montar e acessar o sistema de arquivos. Está disponível nos repositórios do Amazon Linux e também pode ser compilado para outras distribuições Linux.

### 27. Revisar os pacotes instalados

Revise os pacotes instalados pelo comando `yum` anterior.

### 28. Criar diretório, montar o EFS e gerar o log (WebServer1)

```bash
mkdir data
# Se der erro de Permission Denied, rode:
# cd ~/
# e repita o comando mkdir data
```

Cole o comando `sudo mount ...` copiado do console do EFS, substituindo o nome da pasta `efs` por `data` no final do comando, e pressione Enter.

> 💡 **Como fica na prática:** o comando copiado do console do EFS termina com a palavra `efs` (nome da pasta de destino, que ainda não existe). Você troca só essa última palavra por `data` (a pasta criada no `mkdir` acima) - o ID do sistema de arquivos **não muda**.
>
> Exemplo, com o ID `fs-03a9da52f791ef733` (usando o assistente de montagem do EFS):
>
> **Como vem do console:**
> ```bash
> sudo mount -t efs -o tls fs-03a9da52f791ef733:/ efs
> ```
>
> **Como você deve rodar (trocando `efs` por `data` no final):**
> ```bash
> sudo mount -t efs -o tls fs-03a9da52f791ef733:/ data
> ```

```bash
cd data
sudo bash -c "cat >> efs-1-setup.log"
```

No cursor que aparece, digite:

```
efs-1 mounted in site A
```

Pressione **Ctrl+C** para encerrar a sessão do `cat`. Depois, visualize o conteúdo do log:

```bash
cat efs-1-setup.log
```

### 29. Voltar ao console do EFS

1. Na outra aba do navegador, navegue até a página **File systems** no console do Amazon EFS.
2. Clique no nome do sistema de arquivos `PetModels-EFS-1`.

> 💡 **Conceito:** sistemas de arquivos podem ser montados em recursos de computação como Amazon EC2, Amazon ECS e AWS Lambda.

### 30. Gerenciar a rede do sistema de arquivos

Na aba **Network**, clique em **Manage**.

> 💡 **Conceito:** o acesso ao sistema de arquivos é fornecido através do protocolo NFS.

### 31. Adicionar um novo Mount Target

Em **Mount targets**, clique em **Add mount target**.

> 💡 **Conceito:** um mount target é um endpoint de VPC que fornece acesso ao seu sistema de arquivos EFS.

### 32. Configurar o novo Mount Target

1. **Availability zone:** `us-east-1b`
2. **Subnet ID:** `PetModels-Subnet2`

> 💡 **Conceito:** você pode criar mount targets usando o Console de Gerenciamento da AWS, a AWS CLI, ou de forma programática usando os AWS SDKs - seja na criação do sistema de arquivos ou depois.

### 33. Associar o Security Group ao novo Mount Target

1. **Security groups:** `PetModels-EFS-1-SG`
2. Clique em **Save**.
   - Se aparecer o erro *"User is not authorized to perform that action on the specified resource"*, pode ignorar com segurança.

### 34. Confirmar disponibilidade do novo Mount Target

1. Após alguns minutos, na seção **Network**, clique no ícone de atualizar (refresh).
2. Para o novo mount target, em **Mount target state**, confirme que o status é **Available** (aguarde até mudar antes de continuar).

> 💡 **Conceito:** cada mount target instala uma Elastic Network Interface (ENI) no subnet escolhido. Uma ENI é um componente lógico de rede na VPC que representa uma placa de rede virtual, recebendo automaticamente um IP da VPC.

### 35. Conectar-se à instância WebServer2

1. Navegue até a página **Instances** no console do EC2.
2. Selecione o checkbox de `WebServer2`.
3. Clique em **Connect**.

### 36. Abrir sessão via Session Manager (WebServer2)

1. Clique na aba **Session Manager**.
2. Clique em **Connect**.

### 37. Instalar o cliente do EFS (WebServer2)

```bash
sudo -i
sudo yum install -y amazon-efs-utils
```

> 💡 **Conceito:** você pode usar o EC2 User Data para automatizar (bootstrap) a montagem de sistemas de arquivos EFS em novas instâncias no momento do lançamento.

### 38. Criar diretório, montar o EFS e atualizar o log (WebServer2)

```bash
mkdir data
# Se der erro de Permission Denied, rode:
# cd ~/
# e repita o comando mkdir data
```

Cole o comando `sudo mount ...` copiado do console do EFS, substituindo o nome da pasta `efs` por `data` no final do comando (mesma lógica do passo 28), e pressione Enter.

```bash
cd data
sudo bash -c "cat >> efs-1-setup.log"
```

No cursor que aparece, digite:

```
efs-1 mounted in site B
```

Pressione **Ctrl+C** para encerrar a sessão do `cat`.

### 39. Confirmar o compartilhamento de arquivos entre instâncias

```bash
cat efs-1-setup.log
```

Revise as duas entradas do log - as alterações feitas tanto no `WebServer1` quanto no `WebServer2` estão no mesmo arquivo `efs-1-setup.log`, comprovando o compartilhamento de arquivos entre as instâncias através do EFS. 🎉

### 40. Conclusão da Prática

Parabéns! Você concluiu a seção de Prática. O próximo passo é a seção **DIY** para completar a solução.

---

## 🧪 DIY (Do It Yourself)

### 🎯 Objetivos do DIY

1. Montar um endpoint do EFS em uma terceira instância EC2 (`WebServer3`).
2. Testar se os arquivos estão acessíveis a partir dessa instância EC2.

### ✅ Método de Validação da Solução

O servidor de teste vai verificar duas ações:

- Você lançou (ou já possui) um sistema de arquivos Amazon EFS e o montou na instância EC2 `WebServer3`.
- Você adicionou os arquivos corretos a ele.

> 💡 **Dica:** o terceiro mount target do EFS deve estar na Availability Zone `us-east-1c`.

### 🔑 Informação necessária

- **ID do sistema de arquivos Amazon EFS:** o mesmo `PetModels-EFS-1` criado na prática guiada (ex.: `fs-03a9da52f791ef733` - use o ID real que você anotou no passo 21).

### 🛠️ Passo a Passo sugerido

Seguindo a mesma lógica aplicada ao `WebServer2` na prática, para o `WebServer3`:

**1. Criar o terceiro Mount Target (AZ `us-east-1c`)**

1. No console do Amazon EFS, acesse **File systems** → `PetModels-EFS-1`.
2. Na aba **Network**, clique em **Manage**.
3. Em **Mount targets**, clique em **Add mount target**.
4. **Availability zone:** `us-east-1c`
5. **Subnet ID:** escolha o subnet correspondente a essa AZ (ex.: `PetModels-Subnet3`).
6. **Security groups:** `PetModels-EFS-1-SG`
7. Clique em **Save**.
8. Aguarde alguns minutos e atualize a página até o **Mount target state** ficar **Available**.

**2. Conectar-se à instância WebServer3**

1. No console do Amazon EC2, vá em **Instances**.
2. Selecione o checkbox de `WebServer3`.
3. Clique em **Connect**.
4. Na aba **Session Manager**, clique em **Connect**.

**3. Instalar o cliente do EFS**

```bash
sudo -i
sudo yum install -y amazon-efs-utils
```

**4. Criar o diretório e montar o EFS**

```bash
mkdir data
# Se der erro de Permission Denied, rode:
# cd ~/
# e repita o comando mkdir data
```

Monte o sistema de arquivos usando o ID anotado anteriormente (troque `fs-03a9da52f791ef733` pelo seu ID real):

```bash
sudo mount -t efs -o tls fs-03a9da52f791ef733:/ data
```

**5. Adicionar/atualizar o arquivo de log compartilhado**

```bash
cd data
sudo bash -c "cat >> efs-1-setup.log"
```

Digite:

```
efs-1 mounted in site C
```

Pressione **Ctrl+C** para encerrar a sessão do `cat`.

**6. Testar o acesso aos arquivos**

```bash
cat efs-1-setup.log
```

Confirme que as **três** entradas de log aparecem no arquivo (site A, site B e site C), comprovando que o `WebServer3` está acessando o **mesmo** sistema de arquivos compartilhado que as outras duas instâncias.

```bash
ls -la data/
```

Revise se os demais arquivos criados anteriormente pelo `WebServer1` e `WebServer2` também estão visíveis a partir do `WebServer3` - esse é o teste final que comprova o compartilhamento de arquivos entre as três filiais via Amazon EFS. 🎉

---

## ✅ Conclusão

Com a conclusão da prática e do DIY, a agência de modelagem de pets passa a ter:

- Um **único sistema de arquivos Amazon EFS** (`PetModels-EFS-1`) compartilhado entre as três filiais (`WebServer1`, `WebServer2` e `WebServer3`), uma em cada Availability Zone (`us-east-1a`, `us-east-1b`, `us-east-1c`).
- **Armazenamento centralizado e elástico**, que escala automaticamente conforme fotos e metadados dos pets são adicionados, eliminando o problema de servidores ficarem sem espaço.
- **Acesso simultâneo e consistente** aos arquivos entre as filiais, sem necessidade de scripts de sincronização.
- **Controle de acesso via security groups e permissões de arquivo**, possibilitando restringir pastas sensíveis (como as dos clientes VIP) somente à equipe de concierge.
- **Alta durabilidade (11 noves - 99.999999999%)**, com os dados protegidos em múltiplas Availability Zones.
