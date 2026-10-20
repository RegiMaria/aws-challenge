# Thinking Serverless: Arquiteturas Orientadas a Eventos

Este módulo apresenta a evolução arquitetural em direção aos microserviços e arquiteturas
orientadas a eventos (Event-Driven Architectures - EDA), destacando a importância das
conexões assíncronas e o papel do AWS Lambda junto a outros serviços gerenciados.

1. Abstração de Infraestrutura e Evolução para Microserviços

* *Conceito de Serverless:* Abstrai a infraestrutura de computação ao ponto de eliminar qualquer
responsabilidade sobre o gerenciamento dos servidores onde o código roda.

* *Sem custo ocioso:* Quando a aplicação não está em execução, não há custos com servidores parados.

* *Flexibilidade e Agilidade:* Reflete uma evolução na indústria em direção a microserviços desacoplados, 
permitindo lançar novas funcionalidades e escalar com muito mais agilidade.   

2. O que são Arquiteturas Orientadas a Eventos (EDA)?

Em uma arquitetura orientada a eventos, os microserviços utilizam eventos para se
comunicarem e invocarem uns aos outros de forma totalmente desacoplada e agnóstica de tecnologia.

* *O que é um evento?* É uma simples mudança ou atualização de estado
(ex: um item adicionado ao carrinho de compras de um e-commerce).

* *Desacoplamento:* O estado e o código são separados; a integração entre
os componentes é feita tipicamente via mensageria assíncrona.

* *Padrões Consagrados:* Padrões de integração orientados a eventos foram documentados em
2003 no livro "Enterprise Integration Patterns". No entanto, antigamente eram caros de
construir e gerar acoplamento (lock-in). Com a nuvem Serverless, esses padrões podem
ser implementados com custos e sobrecarga operacional mínimos.

