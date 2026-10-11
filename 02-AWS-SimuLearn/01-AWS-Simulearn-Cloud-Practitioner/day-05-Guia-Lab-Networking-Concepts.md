# AWS SimuLearn: Networking Concepts - Guia de Execução

## Objetivo Geral
Configurar componentes de rede de uma VPC (route tables, internet gateway e security groups) para permitir conectividade segura à internet para o Web Server, mantendo o DB Server isolado, exceto para o tráfego necessário do Web Server na porta 3306.

---

## Parte 1 - Prática (Practice)

### Passo 1-2: Acessar o console EC2
1. Confirme que a região está definida como **N. Virginia (us-east-1)**.
2. Na busca de serviços, digite `ec2` e acesse o console do **EC2**.

### Passo 3-4: Testar conectividade inicial
1. Em **Instances**, selecione o **Web Server**.
2. Na aba **Details**, copie o **Public IPv4 address**.
3. Cole o IP em uma nova aba do navegador → deve ocorrer **timeout** (ainda sem rota para a internet).
4. Volte para o console EC2.

### Passo 5-6: Explorar rede da instância
1. Com o Web Server selecionado, abra a aba **Networking**.
2. Revise os endereços **Public e Private IPv4**.
3. Clique no **Subnet ID** para abrir o console da VPC.

### Passo 7-8: Localizar a route table da subnet pública
1. Em **Subnets**, selecione **WebServerSubnet**.
2. Na aba **Route table**, clique no link que contém **RouteTable2**.
3. Na aba **Routes**, observe as rotas existentes:
   - Rota local (tráfego interno da VPC)
   - Rota apontando para um **NAT Gateway** (será substituída)
4. Clique em **Edit routes**.

### Passo 9: Remover rota do NAT Gateway
1. Clique em **Remove** para excluir a rota que aponta para o NAT Gateway.

### Passo 10-11: Adicionar rota para o Internet Gateway
1. Clique em **Add route**.
2. **Destination:** `0.0.0.0/0`
3. **Target:** `Internet Gateway` → selecione o `igw-xxxxxxx`.
4. Clique em **Save changes**.
5. Confirme na aba **Routes** que a nova rota aponta para o Internet Gateway.

> 💡 `0.0.0.0/0` = "todos os endereços IP" (todo o tráfego de internet).

### Passo 12-13: Acessar o Security Group do Web Server
1. Volte para a página **Instances** no EC2.
2. Selecione o **Web Server** → aba **Security**.
3. Clique no grupo **WebServerSecurityGroup**.
4. Na aba **Inbound rules**, clique em **Edit inbound rules**.

### Passo 14-16: Liberar HTTP (porta 80)
1. Clique em **Add rule**.
2. **Type:** `HTTP` (⚠️ não escolha HTTPS).
3. **Source:** `Anywhere-IPv4` (0.0.0.0/0).
4. Clique em **Save rules**.

> 💡 Security Groups são *stateful*: o tráfego de resposta é liberado automaticamente, mesmo sem regra explícita de saída.

### Passo 17-19: Revisar/ajustar regras de saída (Outbound)
1. Clique na aba **Outbound rules**.
2. Observe que já existem regras liberando portas **80, 443 e 3306**.
3. Clique em **Edit outbound rules** → **Add rule**.
4. **Type:** `All traffic` | **Destination:** `0.0.0.0/0`.
5. Clique em **Save rules**.

### Passo 20-22: Validar conectividade
1. Volte para **Instances** → selecione o **Web Server** → aba **Networking**.
2. Copie novamente o **Public IPv4 address**.
3. Acesse em uma nova aba usando **HTTP** (ex.: `http://11.22.33.44`).
4. No diagrama exibido, verifique:
   - ✅ Conexão **Internet → Web Server**: estabelecida.
   - ❌ Conexão **Web Server → DB Server**: falhou (será corrigido no DIY).

### Passo 23: Fim da Prática
Prática concluída - siga para o DIY.

---

## Parte 2 - DIY (Do It Yourself)

### Objetivo
Alterar as regras do Security Group para permitir tráfego na **porta 3306** até o DB Server.

### Contexto da validação
- Web Server está na subnet `10.10.0.0/24` (Security Group: `WebServerSecurityGroup`).
- DB Server está na subnet `10.10.2.0/24` (Security Group: `DbServerSecurityGroup`).
- É necessário permitir conexão **TCP na porta 3306 (MySQL/Aurora)** do Web Server para o DB Server.

### Passo a passo

1. No console **EC2**, vá em **Instances** e selecione o **DB Server**.
2. Clique na aba **Security** e depois no link do **DbServerSecurityGroup**.
3. Na aba **Inbound rules**, clique em **Edit inbound rules**.
4. Clique em **Add rule** e configure:
   - **Type:** `MYSQL/Aurora` (preenche automaticamente a porta `3306`)
   - **Protocol:** `TCP` (preenchido automaticamente)
   - **Source:** no campo de busca, digite `sg-` ou o nome `WebServerSecurityGroup` e selecione-o na lista.
     - ⚠️ Não use "Anywhere" nem um IP - a origem deve ser o **Security Group do Web Server**, para permitir tráfego apenas dele.
5. Clique em **Save rules**.
6. Volte à página do diagrama (obtida via IP público do Web Server) e recarregue.
7. Verifique se a conexão **Web Server → DB Server** agora aparece como **Connected**.

### Resultado esperado
- ✅ Internet → Web Server: Connected
- ✅ Web Server → DB Server: Connected (porta 3306)
- ❌ DB Server permanece inacessível diretamente pela internet (subnet privada sem rota para o Internet Gateway)

---

## Resumo da Arquitetura Final

| Componente | Configuração |
|---|---|
| VPC | Isola a rede do Web Server e do DB Server |
| Subnet pública | WebServerSubnet - rota `0.0.0.0/0 → Internet Gateway` |
| Subnet privada | DbServerSubnet - sem rota para a internet |
| Internet Gateway | Anexado à VPC, permite tráfego de entrada/saída da subnet pública |
| WebServerSecurityGroup | Inbound: HTTP (80) de `Anywhere-IPv4` |
| DbServerSecurityGroup | Inbound: MySQL/Aurora (3306) apenas de `WebServerSecurityGroup` |


## Architeture

<img width="986" height="687" alt="Image" src="https://github.com/user-attachments/assets/cd8a8fdb-4ea5-43e3-8299-182b1a2e4283" />



