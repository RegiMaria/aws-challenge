# 🐕 Radar - Banco de Quizzes

<p align="center">
  <img src="https://github.com/user-attachments/assets/3b361f39-7a62-441e-9a0a-0ee6c6c9f0d6" width="250" alt="Radar Pet" />
</p>

- **Espécie:** Cachorro (*Dobermann Marrom*)
- **Domínio AWS:** Conceitos de Nuvem (*Cloud Concepts*)
- **Tópico Principal:** Infraestrutura Global da AWS (Regiões, AZs e Edge Locations)
- **Total de Quizzes:** 1 (Expandido para 5 para estudo prático)
- **Status:** Domesticado ✅

---

### Quiz 1: Conceito de Região AWS
**Enunciado:** Um arquiteto de soluções precisa escolher o local geográfico ideal para hospedar os dados de um cliente cumprindo leis estritas de soberania de dados. O que define uma **Região AWS**?

- [ ] **A)** Um único centro de dados físico isolado localizado numa capital.
- [x] **B)** Uma área geográfica física no mundo que contém múltiplos centros de dados agrupados em Zonas de Disponibilidade isoladas.
- [ ] **C)** Uma coleção de servidores virtuais compartilhados entre diferentes fornecedores de nuvem.
- [ ] **D)** Um ponto de presença de rede projetado exclusivamente para fazer cache de vídeos estáticos.

> **Por que esta é a resposta correta?**
> * Uma **Região AWS** é uma localização geográfica distinta no mundo composta por múltiplas Zonas de Disponibilidade (*AZs*) isoladas e separadas por distância física razoável, conectadas por redes redundantes de altíssima velocidade e ultra-baixa latência.
> * *Incorretas:* Uma Região nunca é apenas um único centro de dados; o isolamento entre AZs garante tolerância a falhas na mesma região.
>
> 📖 **Documentação:** [Infraestrutura Global - Regiões e Zonas de Disponibilidade](https://aws.amazon.com/pt/about-aws/global-infrastructure/regions_az/)

---

### Quiz 2: Zonas de Disponibilidade (Availability Zones - AZs)
**Enunciado:** Para garantir alta disponibilidade e tolerância a falhas para uma aplicação crítica, o desenvolvedor deve implantar instâncias Amazon EC2 em pelo menos duas Zonas de Disponibilidade diferentes dentro da mesma região. O que é uma Zona de Disponibilidade (AZ)?

- [x] **A)** Um ou mais centros de dados físicos discretos com energia, arrefecimento e segurança física independentes.
- [ ] **B)** Um servidor lógico que compartilha a mesma fonte de energia e infraestrutura com outras AZs.
- [ ] **C)** Um local de rede isolado fora da rede global da AWS para testes de desastre.
- [ ] **D)** Uma conexão via cabo submarino exclusiva entre dois continentes.

> **Por que esta é a resposta correta?**
> * Cada **AZ** é composta por um ou mais centros de dados físicos individuais, com fontes de alimentação, redes e sistemas de arrefecimento totalmente independentes. Se uma AZ sofrer uma falha física (como corte de energia), as outras AZs na mesma região permanecem operacionais.
>
> 📖 **Documentação:** [Visão geral das Zonas de Disponibilidade na AWS](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)

---

### Quiz 3: Locais de Borda (Edge Locations) e Cache de Conteúdo
**Enunciado:** Uma empresa de mídia deseja entregar vídeos e arquivos estáticos para utilizadores em todo o mundo com a menor latência possível, utilizando o serviço de CDN Amazon CloudFront. Qual componente da infraestrutura global da AWS é responsável por armazenar esse conteúdo em cache perto dos utilizadores finais?

- [ ] **A)** Regiões Locais (*AWS Local Zones*)
- [ ] **B)** Zonas de Disponibilidade Primárias
- [x] **C)** Locais de Borda (*Edge Locations*)
- [ ] **D)** Dispositivos AWS Outposts

> **Por que esta é a resposta correta?**
> * Os **Locais de Borda (Edge Locations)** são pontos de presença localizados nas principais cidades e centros populacionais ao redor do mundo. Eles não hospedam servidores de aplicação genéricos, mas mantêm dados em cache para serviços como o Amazon CloudFront e Route 53, reduzindo drasticamente o tempo de resposta (*latência*).
>
> 📖 **Documentação:** [Locais de Borda e CloudFront Key Concepts](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)

---

### Quiz 4: Escolha da Região Ideal
**Enunciado:** Ao criar um novo serviço na AWS, quais fatores devem ser avaliados prioritariamente para determinar em qual Região AWS os recursos devem ser implantados? (Selecione DOIS)

- [x] **A)** Proximidade geográfica dos utilizadores finais para reduzir latência.
- [x] **B)** Requisitos regulatórios e leis de conformidade de residência de dados.
- [ ] **C)** A cor do painel de administração configurada no AWS Management Console.
- [ ] **D)** O número total de computadores pessoais conectados à rede Wi-Fi da empresa.

> **Por que estas são as respostas corretas?**
> * **Latência e Experiência do Utilizador:** Implantar recursos na região mais próxima dos utilizadores melhora a velocidade de resposta.
> * **Conformidade Legal:** Diversas legislações exigem que dados financeiros ou pessoais permaneçam estritamente dentro das fronteiras geográficas do país de origem.
>
> 📖 **Documentação:** [Como escolher a Região AWS correta para sua carga de trabalho](https://aws.amazon.com/pt/blogs/aws-brasil/como-escolher-a-regiao-aws-correta-para-sua-carga-de-trabalho/)

---

### Quiz 5: AWS Local Zones vs. Regiões Padrão
**Enunciado:** Uma empresa de jogos online precisa de latência de um único dígito em milissegundos para jogadores localizados numa área metropolitana específica que fica distante de qualquer Região AWS existente. Qual extensão da infraestrutura AWS estende os serviços de computação e armazenamento para mais perto dessa cidade?

- [ ] **A)** AWS Wavelength
- [x] **B)** AWS Local Zones
- [ ] **C)** AWS Snowball Edge
- [ ] **D)** AWS Direct Connect

> **Por que esta é a resposta correta?**
> * As **AWS Local Zones** colocam serviços de computação, armazenamento e banco de dados muito mais próximos de grandes centros urbanos e industriais onde não existe uma Região AWS completa, atendendo a casos de uso que exigem latência ultra-baixa (como streaming de jogos, mídia e simulações).
>
> 📖 **Documentação:** [O que são as AWS Local Zones?](https://aws.amazon.com/pt/about-aws/global-infrastructure/localzones/)
