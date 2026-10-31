# 05 - Thinking Serverless at Scale

 ## 1. Visão Geral e Objetivos do Curso
 
Este curso é voltado para a criação de arquiteturas serverless capazes de crescer de forma confiável — seja por
crescimento orgânico contínuo ou por picos repentinos de uso (como cenários de fusões e aquisições ou eventos de alta demanda).

## Objetivos de Aprendizagem:
- Métricas de Escala: Comparar arquiteturas baseadas em AWS Lambda com arquiteturas de nuvem tradicionais em termos de o que e como medir para suportar o tráfego.

- Otimização de Desempenho: Aplicar boas práticas para otimizar o tempo de execução de funções Lambda (Cold e Warm Starts).

- Persistência de Estado: Comparar opções de armazenamento para manter o estado da aplicação e dos recursos.

- Controle de Acesso e Regulação: Identificar como e onde aplicar limites de uso (API throttling, chaves de API, controles de concorrência no Lambda e códigos de status HTTP).

- Estratégias de Teste: Dominar testes de unidade, mocking, testes de integração e testes de carga/escala para aplicações serverless.

- Gestão de Custos: Implementar conscientização e otimização contínua de custos e desempenho.

## 2. Arquiteturas Tradicionais (EC2) vs. Arquiteturas Serverless na Escala

Enquanto em arquiteturas tradicionais a escalabilidade exige o provisionamento de instâncias
e balanceadores de carga, no modelo serverless o foco muda para a análise de limites e comportamento de eventos.
