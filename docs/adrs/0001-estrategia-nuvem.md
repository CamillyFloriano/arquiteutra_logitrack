# ADR 0001 — Estratégia de Nuvem e Escalabilidade

**Status:** Aceito  
**Data:** 2026-05  
**Autor:** Camilly Christine S. Floriano

## Contexto

O LogiTrack precisa processar dados de geolocalização em tempo real de múltiplos veículos simultaneamente, além de calcular rotas e enviar notificações. Em picos de operação, como datas comemorativas ou grandes entregas, o volume de requisições pode aumentar drasticamente. A arquitetura precisa escalar sem comprometer o desempenho e a disponibilidade.

## Decisão

Adotar o modelo **PaaS (Platform as a Service)** na **AWS**, utilizando os seguintes serviços:
- **Amazon ECS (Elastic Container Service)** para orquestração dos microsserviços
- **Amazon RDS (PostgreSQL)** para persistência de dados
- **Amazon SQS** como suporte ao Message Broker
- **Auto Scaling Groups** para escalabilidade horizontal automática

## Alternativas Rejeitadas

| Alternativa | Motivo da Rejeição |
|-------------|-------------------|
| IaaS (EC2 puro) | Exige gerenciamento manual de infraestrutura, aumentando complexidade operacional |
| Serverless (Lambda) | Latência de cold start incompatível com rastreamento em tempo real |
| On-premise | Alto custo de infraestrutura e baixa elasticidade |

## Justificativa

Conforme Richards & Ford (2020), a escalabilidade horizontal é preferível à vertical em arquiteturas de microsserviços, pois permite adicionar instâncias independentes por serviço sem afetar os demais. O PaaS reduz a sobrecarga operacional ao abstrair o gerenciamento do sistema operacional e da infraestrutura base, permitindo que a equipe foque na lógica de negócio. O Auto Scaling da AWS garante que novos containers sejam provisionados automaticamente quando o uso de CPU ultrapassar 70%.

## Trade-offs

- **Vantagem:** Escalabilidade horizontal automática por serviço
- **Vantagem:** Alta disponibilidade com múltiplas zonas de disponibilidade (AZs)
- **Desvantagem:** Custo variável — pode aumentar em picos de tráfego
- **Desvantagem:** Dependência do provedor cloud (vendor lock-in)

## Impacto nos RNFs

- **Escalabilidade:** ✅ Atendido via Auto Scaling
- **Disponibilidade:** ✅ Atendido via múltiplas AZs
- **Desempenho:** ✅ Atendido via containers leves e ECS

## Referências

- Richards, M., & Ford, N. (2020). *Fundamentals of Software Architecture*. O'Reilly.
- Pressman, R. S. (2021). *Engenharia de Software*. McGraw Hill.
- AWS ECS Documentation: https://docs.aws.amazon.com/ecs/
