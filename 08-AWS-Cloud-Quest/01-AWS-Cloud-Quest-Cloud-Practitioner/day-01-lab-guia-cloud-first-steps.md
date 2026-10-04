<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" width="220" alt="AWS Logo"/>

#  01 -Guia AWS Cloud Quest: Primeiros Passos na Nuvem | Cloud First Steps

</div>


**Nível:** Iniciante | **Foco:** Prática na AWS

# 🏝️ A Primeira Missão na Ilha do Cloud Quest

> **Sua jornada na Nuvem começa agora!**

Ao desembarcar na **Ilha de Cloud Quest**, a cidade precisa da sua ajuda para dar
os primeiros passos na transformação digital. O primeiro desafio que os moradores e
empresas locais trouxeram para você é simples, mas fundamental:
**colocar a primeira aplicação web no ar com alta disponibilidade**.

Nesta missão, você assumirá o papel de um profissional de nuvem e aprenderá a dominar os recursos fundamentais da AWS.

---

### O Desafio do Cliente
O time de operações da cidade precisa exibir o status e as métricas dos servidores em tempo real em um navegador web. Eles precisam que você:
1. Recupere o script de automação armazenado com segurança no **Amazon S3**.
2. Suba o primeiro servidor web na **Amazon EC2** de forma 100% automatizada durante o boot (**User Data**).
3. Garanta a **Alta Disponibilidade** da solução duplicando a infraestrutura em uma segunda **Zona de Disponibilidade (AZ)**, garantindo que o sistema continue no ar mesmo se um data center físico falhar.

Aperte os cintos, prepare o console da AWS e boa sorte em sua primeira missão! 🚀

---

<img width="1537" height="861" alt="Image" src="https://github.com/user-attachments/assets/0227683f-19e6-4774-9044-a18ddf956a36" />

---

## 📖 PARTE 1: APRENDER (Conceitos Essenciais)

Antes de abrir o console da AWS, é fundamental entender os blocos de construção da nuvem.

### 1. Infraestrutura Global: Regiões e Zonas de Disponibilidade (AZs)
* **Região (Region):** Localização geográfica no mundo onde a AWS agrupa múltiplos data centers (exemplo: `us-east-1` em N. Virginia). Garante soberania dos dados e menor latência.
* **Zona de Disponibilidade (Availability Zone - AZ):** Um ou mais data centers físicos isolados dentro de uma Região, com infraestrutura independente de energia e rede (exemplo: `us-east-1a`, `us-east-1b`). Distribuir recursos em múltiplas AZs garante **Alta Disponibilidade** e **Tolerância a Falhas**.

### 2. Amazon S3 (Simple Storage Service)
Serviço de armazenamento de objetos na nuvem para arquivos, imagens, vídeos e scripts.
* **Buckets:** Contêineres onde os arquivos são salvos. O nome de um bucket S3 deve ser **globalmente único** em toda a infraestrutura mundial da AWS.
* **Regra de Exclusão:** Para apagar um bucket no S3, é **obrigatório deletar todos os objetos dentro dele primeiro**.

### 3. Amazon EC2 (Elastic Compute Cloud)
Servidores virtuais sob demanda na nuvem.
* **Modelo Financeiro:** Elimina investimentos em hardware físico e adota o modelo *Pay-as-you-go* (pague apenas pelo que usar).
* **Famílias de Instâncias:** Agrupadas por tipo de uso (exemplo: família **t3**, focada em propósito geral, equilibrando CPU, memória e desempenho de rede).
* **Estados da Instância:**
  * `Pending`: Instância em processo de inicialização.
  * `Running`: Instância ligada e em execução (gerando cobrança de computação).
  * `Stopping / Stopped`: Instância desligada. Para de cobrar por CPU/RAM, mas continua cobrando pelo armazenamento em disco vinculado a ela.
  * `Terminated`: Instância excluída permanentemente.

### 4. AMI (Amazon Machine Image) vs. Snapshot
* **AMI:** Um modelo pré-configurado contendo o Sistema Operacional, configurações e permissões para lançar novas instâncias.
* **Snapshot:** Uma foto/cópia de segurança estática (backup ponto-no-tempo) de um volume de disco (EBS). A AMI é o molde de criação; o snapshot é a cópia de backup do disco.

### 5. Par de Chaves (Key Pairs) e Criptografia
Mecanismo de acesso seguro a instâncias via criptografia de chave pública:
* A AWS armazena a **Chave Pública** no servidor e você mantém a **Chave Privada** (arquivo `.pem`).
* Sem essa chave, o acesso direto (SSH/RDP) ao sistema operacional é bloqueado.

### 6. Redes na AWS: Amazon VPC e Sub-redes
* **VPC (Virtual Private Cloud):** Uma rede virtual isolada dentro da sua conta AWS. A VPC é um recurso **regional** (abrange todas as AZs de uma região).
* **Sub-rede (Subnet):** Uma divisão de faixa de endereços IP dentro da VPC. **Toda sub-rede pertence a apenas uma única AZ**.

### 7. Security Groups (Grupos de Segurança)
Firewall virtual que filtra o tráfego da instância no nível de rede.
* **Permissões:** Por padrão, bloqueia todo o tráfego de entrada e exige regras explícitas (exemplo: liberar a **Porta 80 / HTTP**).
* **Stateful:** Se o tráfego de entrada for permitido, a resposta de saída é liberada automaticamente.

### 8. User Data vs. Instance Metadata
* **User Data:** Script executado automaticamente no **primeiro boot** da instância para instalar programas ou configurar o ambiente.
* **Instance Metadata:** Informações sobre a própria máquina (ID, IP, AZ), acessadas de dentro da instância pelo IP local `169.254.169.254`.

---

### 💡 Resumo Teórico

| Conceito | Definição Rápida |
| :--- | :--- |
| **Região** | Localização geográfica com múltiplas Zonas de Disponibilidade. |
| **AZ (Zona de Disponibilidade)** | Um ou mais data centers isolados dentro de uma região. |
| **Bucket S3** | Armazenamento de objetos com nome único no mundo. |
| **EC2** | Servidor virtual na nuvem. |
| **AMI** | Modelo com Sistema Operacional usado para iniciar uma EC2. |
| **Security Group** | Firewall virtual da instância (Stateful). |
| **Sub-rede** | Divisão de IP da VPC vinculada a uma única AZ. |
| **User Data** | Script executado na primeira inicialização da instância. |

---

## 🧪 PARTE 2: PRATICAR (Laboratório Prático passo a passo)

Nesta etapa, vamos obter o script de inicialização armazenado no S3 e lançar nossa primeira instância EC2 com servidor web automatizado.

**Problema de negócio:**
A equipe de estabilização da Ilha Quest deseja aumentar 
a confiabilidade e disponibilidade do seu sistema de estabilização atual.

**Objetivos do aprendizado:**
Lançar duas EC2 em duas AZs dentro da mesma Região.


### Etapa 1: Baixar o Script `user-data.txt` do S3
1. No canto superior direito do Console AWS, confirme se a região está como **N. Virginia (`us-east-1`)**.
2. Na barra de pesquisa superior, digite `s3` e selecione o serviço **S3**.
3. Na guia **Buckets de uso geral**, clique no bucket com o nome iniciado por `cloud-first-steps-`.
4. Selecione o arquivo `user-data.txt` e clique em **Download** para salvar no seu computador.

#### 📄 Entendendo o Script do Laboratório:
```bash
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo yum install -y git

# Consulta aos Metadados usando IMDSv2 (IP 169.254.169.254)
export TOKEN=`curl -X PUT "[http://169.254.169.254/latest/api/token](http://169.254.169.254/latest/api/token)" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"`
export META_INST_ID=`curl [http://169.254.169.254/latest/meta-data/instance-id](http://169.254.169.254/latest/meta-data/instance-id) -H "X-aws-ec2-metadata-token: $TOKEN"`
export META_INST_TYPE=`curl [http://169.254.169.254/latest/meta-data/instance-type](http://169.254.169.254/latest/meta-data/instance-type) -H "X-aws-ec2-metadata-token: $TOKEN"`
export META_INST_AZ=`curl [http://169.254.169.254/latest/meta-data/placement/availability-zone](http://169.254.169.254/latest/meta-data/placement/availability-zone) -H "X-aws-ec2-metadata-token: $TOKEN"`

# Gera a página web e inicia o Apache na porta 80
cd /var/www/html
# (código HTML gerando os cards da página)
sudo service httpd start
```


### Etapa 2: Lançar a Instância EC2
1. Na barra de busca, digite ec2 e abra o console do EC2.

2. Clique em Executar instância (Launch instance).

3. Configurações da máquina:

- Nome: webserver01

- AMI: Mantenha Amazon Linux 2023.

- Tipo de Instância: t3.micro.

- Par de Chaves: Selecione Proceder sem uma chave.

4. Configurações de Rede (clique em Editar):

- VPC: Escolha obrigatoriamente cloud-first-steps/LabVPC.

- Sub-rede: Escolha a sub-rede vinculada à AZ us-east-1a.

5. Security Group:

- Nome: Lab-SG

- Descrição: HTTP Security Group

- Regras de Entrada: Adicione a regra do tipo HTTP (Porta 80, origem 0.0.0.0/0).

6. Injetar o Script:

- Expanda a seção Detalhes avançados (Advanced details) na parte inferior.

- Em Dados do usuário (User data), clique em Escolher arquivo e envie o `user-data.txt`.

- Clique em Executar instância (Launch instance).

### Etapa 3: Acessar a Aplicação Web
- Vá em Instâncias, selecione `webserver01` e aguarde o status mudar para **Running**.

- Copie o valor do campo Public IPv4 DNS.

- Abra uma nova guia no navegador e acesse: `http://<DNS_PUBLICO_COPIADO>`

- Confirme se a página web exibe os metadados da instância (ID, Tipo e AZ).

<img width="637" height="541" alt="Image" src="https://github.com/user-attachments/assets/9171d0bd-2dab-4604-bb94-035cb9a18b5a" />

---


## PARTE 3: DIY (Faça Você Mesmo)
Nesta etapa, você colocará em prática o conceito de **Alta Disponibilidade**
duplicando a infraestrutura em uma segunda Zona de Disponibilidade.

**Objetivo do Desafio:**
Lançar uma segunda instância EC2 (webserver02)
em uma AZ diferente da primeira, reutilizando a VPC,
o Security Group e o script de User Data.

### Passo a Passo de Execução:

1. Verificar a AZ atual: Na aba de instâncias, confira em qual AZ a webserver01 foi criada (ex: us-east-1a).

<img width="1914" height="764" alt="Image" src="https://github.com/user-attachments/assets/eb880466-baa0-4a65-9291-2a91b87a4117" />


2. Lançar a segunda máquina:
   
- Clique em Executar instância.

- Nome: webserver02

- AMI: Amazon Linux 2023 | Tipo: t3.micro

- Par de Chaves: Proceder sem uma chave.

3. Configurar Rede Multi-AZ:

- Clique em Editar nas configurações de rede.

- VPC: cloud-first-steps/LabVPC.

- Sub-rede: Selecione a sub-rede vinculada à outra AZ (ex: us-east-1b).

- Security Group: Marque a opção Selecionar grupo de segurança existente e escolha o Lab-SG.


<img width="1863" height="762" alt="Image" src="https://github.com/user-attachments/assets/9d6e64d3-02e9-4362-9c3b-1667585550c8" />


---


4. Injetar o User Data:

- Em Detalhes avançados > Dados do usuário, envie novamente o arquivo `user-data.txt`.

- Finalizar: Clique em Executar instância.


5. Validação:
- Aguarde a webserver02 ficar no estado Running.

 - Copie seu Public IPv4 DNS e abra no navegador: `http://<DNS_PUBLICO_DA_WEBSERVER02>`

- Verifique se o campo Availability Zone na página exibe a nova AZ (ex: us-east-1b).

- Copie os ID da instância

<img width="1901" height="817" alt="Image" src="https://github.com/user-attachments/assets/4383744f-3924-41db-a8ce-381b8f1efdfe" />

---

- Cole nos campos para validação

 <img width="998" height="795" alt="Image" src="https://github.com/user-attachments/assets/7fe9db22-0759-49f8-894f-efd0ea37871e" />

---
  
- Clique em Validate no painel do AWS Cloud Quest para concluir a missão!

**Erros Comuns para Evitar no Exame e nos Labs**
- Trocar a Região da AWS: Alterar o seletor para outra região fora de N. Virginia (us-east-1) quebrará a validação do laboratório.

- Criar sub-redes na mesma AZ: Sub-redes diferentes podem pertencer à mesma AZ. Sempre confira o código da AZ (ex: us-east-1a vs us-east-1b) para garantir a redundância física.

- Usar HTTPS em vez de HTTP: O laboratório libera a porta 80 (HTTP). Se tentar abrir com https://, a conexão dará tempo limite (timeout).








