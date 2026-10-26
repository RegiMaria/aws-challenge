# 05 - Failure Management Across Your Application

Em arquiteturas orientadas a eventos e desacopladas, existem múltiplos componentes interagindo de forma assíncrona.
Gerenciar falhas de forma resiliente, desde o código até a infraestrutura, é indispensável para evitar gargalos,
perda de dados ou comportamento imprevisível no sistema.

## 1. Gestão de Falhas no Código e nas Funções

**A. Monitoramento com CloudWatch**

- CloudWatch & CloudWatch Logs: Todo stdout ou erro gerado na função Lambda é automaticamente capturado e agregado pelo Amazon CloudWatch Logs.

- Métricas Customizadas: Utilize a biblioteca de logs nativa do seu runtime para estruturar logs e gerar métricas customizadas para alarmes no CloudWatch.
