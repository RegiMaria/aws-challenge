# Guia Passo a Passo - Lab DynamoDB (Prática)

## Objetivo
- Criar um banco de dados NoSQL como uma tabela do Amazon DynamoDB
- Adicionar registros com schema dinâmico (flexível) na tabela
- Consultar a tabela com Query e Scan

---
## Arquitetura

<div align="center">
  <img width="500" height="250" alt="Image" src="https://github.com/user-attachments/assets/7167d7c6-eb9a-4c21-be76-20938205a13a" />
</div>

---

### Passo 1 - Acessar o DynamoDB
Na barra de pesquisa superior do console AWS, digite `dynamodb` e clique em **DynamoDB** nos resultados, em Services.

### Passo 2 - Ir para Tables
No menu à esquerda, clique em **Tables**.

### Passo 3 - Criar a tabela
Clique em **Create table**.

### Passo 4 - Definir nome e chave primária
- **Table name:** `UserVideoHistory`
- **Partition key:** `userId`
  - ⚠️ Atenção: as chaves diferenciam maiúsculas/minúsculas. Digite exatamente `userId` (com "I" maiúsculo).
- Tipo: **String**

Clique em **Create table**.

### Passo 5 - Confirmar que a tabela está ativa
Aguarde o **Status** mudar para **Active**. Quando estiver ativa, clique no nome da tabela (`UserVideoHistory`) para abri-la.

### Passo 6 - Criar um item
Clique em **Actions** → **Create item**.

### Passo 7 - Preencher os primeiros atributos
- **userId** → Value: `12345-abcd-6789`
- **lastDateWatched** → Value: `1740086439`
  - Esse é um timestamp UNIX (data/hora em segundos).

### Passo 8 - Adicionar novo atributo (tipo String)
Clique em **Add new attribute** → escolha **String**.

### Passo 9 - Preencher o atributo videoId
- **Attribute name:** `videoId`
  - ⚠️ Atenção: "Id" com "I" maiúsculo.
- **Value:** `9875-djac-1859`

### Passo 10 - Adicionar outro atributo (tipo String)
Clique em **Add new attribute** → escolha **String** novamente.

### Passo 11 - Preencher o atributo preferredLanguage
- **Attribute name:** `preferredLanguage`
- **Value:** `en-US`

### Passo 12 - Adicionar atributo do tipo Lista
Clique em **Add new attribute** → escolha **List**.

### Passo 13 - Nomear a lista
- **Attribute name:** `supportedDeviceTypes`

### Passo 14 - Inserir o primeiro item da lista
Em **Value**, clique em **Insert a field** → escolha **String**.

### Passo 15 - Preencher o primeiro valor da lista
- Digite: `Amazon Fire TV`
- Clique novamente em **Insert a field** → escolha **String**.

### Passo 16 - Preencher o segundo valor da lista e salvar
- Digite: `Amazon Fire Tablet`
- Clique em **Create item** para salvar o registro.

---

### Passo 17 - Editar o item criado
Na seção **Items returned**, clique no valor `12345-abcd-6789` (na coluna userId) para reabrir o item para edição.

### Passo 18 - Adicionar novo atributo (tipo Number)
Clique em **Add new attribute** → escolha **Number**.

### Passo 19 - Preencher lastStopTime e salvar
- **Attribute name:** `lastStopTime`
- **Value:** `90`
  - Esse atributo guarda a duração assistida do vídeo em segundos (tipo Number). É útil para implementar a função de "retomar vídeo de onde parou".
- Clique em **Save and close**.

---

**O que é um "item" e um "atributo"**

Pense assim:

Item = uma linha/registro (como uma linha numa tabela SQL, ou um documento JSON)
Atributo = um campo desse registro (como uma coluna, mas flexível - cada item pode ter atributos diferentes)

Você está criando um item só, que representa: "o histórico de um usuário assistindo vídeos numa plataforma 
de streaming (tipo Netflix/Prime Video)".

**Pra que serve cada atributo que nós criamos**

| Atributo | Valor | O que representa na vida real |
|---|---|---|
| `userId` | `12345-abcd-6789` | O ID único desse usuário — é a **partition key**, ou seja, é assim que o DynamoDB localiza esse item depois. |
| `lastDateWatched` | `1740086439` | Quando foi a última vez que ele assistiu algo (em formato **timestamp UNIX** — segundos desde 1970). |
| `videoId` | `9875-djac-1859` | O ID do vídeo/filme/série que ele assistiu por último. |
| `preferredLanguage` | `en-US` | O idioma preferido do usuário para legendas/áudio. |
| `supportedDeviceTypes` | `["Amazon Fire TV", "Amazon Fire Tablet"]` | Quais dispositivos esse usuário usa para assistir. |

**Por que isso importa (a parte conceitual)**

Isso é uma simulação de um caso de uso real: uma empresa de streaming quer guardar, num único lugar,
tudo que precisa saber sobre "o que esse usuário estava fazendo" - pra função de "continuar assistindo",
recomendação de idioma, e saber em quais aparelhos ele costuma usar o app.

**O ponto-chave do lab:** em SQL, você precisaria de uma tabela users, outra videos, outra devices,
com relacionamentos (JOINs) entre elas. Aqui no DynamoDB, você guardou tudo dentro de um item só, 
com tipos de dados diferentes (String, Number, List) misturados livremente - é isso que **"schema dinâmico"**
quer dizer: nada te obrigou a definir essas colunas antes, e o próximo item que você criar pode ter atributos completamente diferentes.

No Passo 19 você vai até adicionar mais um atributo (lastStopTime) depois que o item já existia - isso seria bem mais trabalhoso em uma
abela SQL tradicional, que exigiria alterar a estrutura da tabela inteira (ALTER TABLE).

### Passo 20 - Executar uma Query (consulta)
1. Clique para expandir **Scan or query items**.
2. Escolha **Query**.
3. Em **userId (Partition key)**, digite: `12345-abcd-6789`
4. Em **lastDateWatched (Sort Key)**, escolha **Greater than**.
5. No campo ao lado, digite: `1740086438`
6. Clique em **Run**.

### Passo 21 - Revisar o resultado da Query
Em **Items returned**, você verá o registro que corresponde aos critérios da consulta.

### Passo 22 - Testar Query com partition key inexistente
1. Altere o campo **userId** para: `abd5-zxcg-12385`
2. Clique em **Run**.
3. Observe: nenhum resultado será retornado, pois não existe registro com essa partition key.

### Passo 23 - Executar um Scan (varredura completa)
1. Em **Scan or query items**, escolha **Scan**.
2. Clique em **Run**.
3. Em **Items returned**, você verá **todos** os itens da tabela, não apenas os que combinam com uma chave específica.

---

## 🎉 Conclusão
Você criou uma tabela DynamoDB, inseriu um item com atributos de tipos variados (String, Number, List) - demonstrando o **schema dinâmico** do NoSQL - e praticou as duas formas principais de busca:

| Operação | Como funciona | Quando usar |
|---|---|---|
| **Query** | Busca exata pela partition key (e opcionalmente refina pela sort key) | Quando você sabe a chave do item que procura - mais rápido e eficiente |
| **Scan** | Varre a tabela inteira, item por item | Quando você não tem a chave, mas tem outro critério - menos eficiente em tabelas grandes |

---

# Seção DIY (Faça Você Mesmo)

## Objetivos
1. Criar um **novo item** na tabela `UserVideoHistory`, com um `userId` único (diferente do que você já usou).
2. Criar, nesse novo item, um atributo do tipo **Number** chamado `rating`.

## Como o lab valida

O servidor de teste vai verificar se o atributo `rating` existe no item criado e se ele é do tipo **Number**.

<div align="center">
  <img width="500" height="250" alt="Image" src="https://github.com/user-attachments/assets/02c8b573-9eec-4a68-ab7b-0c9ab110105f" />
</div>

⚠️ **Atenção:**
- `UserVideoHistory` (nome da tabela) e `rating` (nome do atributo) diferenciam maiúsculas de minúsculas - digite exatamente assim.
- O valor de `userId` pode ser qualquer texto, desde que seja diferente do item que você já criou (senão você estaria editando o mesmo item, não criando um novo).

## Passo a passo sugerido

### Passo 1 - Abrir a tabela
No console do DynamoDB, vá em **Tables** → clique em `UserVideoHistory`.

### Passo 2 - Criar um novo item
Clique em **Actions** → **Create item**.

### Passo 3 - Definir o userId (chave única)
- **userId** → Value: digite qualquer texto novo, por exemplo: `55555-teste-0001`
  - Precisa ser diferente de `12345-abcd-6789`, que já existe na tabela.

### Passo 4 - Adicionar o atributo rating
1. Clique em **Add new attribute**.
2. Escolha **Number** (não String - o teste exige que seja Number).
3. **Attribute name:** `rating`
   - ⚠️ Exatamente em minúsculas, como especificado.
4. **Value:** digite um número, por exemplo: `5`

### Passo 5 - Salvar o item
Clique em **Create item**.

### Passo 6 - Confirmar
Faça um **Scan** (como no Passo 23 da prática) e confirme que o novo item aparece na lista, com `userId` diferente e o atributo `rating` do tipo Number visível.

## Por que isso importa
Esse exercício reforça o conceito central do schema dinâmico: você criou dois itens na **mesma tabela**, mas cada um com um conjunto diferente de atributos - 
o primeiro item não tem `rating`, e esse novo item tem. Em SQL isso exigiria uma coluna `rating` definida na tabela inteira (mesmo que ficasse `NULL` 
pra todo mundo que não tem nota); no DynamoDB, o atributo simplesmente existe só onde você decidiu colocar.
