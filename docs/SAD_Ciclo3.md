# SAD — Software Architecture Document
## LogiTrack — Fase 3: Cloud e Resiliência

**Autor:** Camilly Christine S. Floriano  
**Matrícula:** 2312301  
**Data:** 2026-05  
**Versão:** 3.0

---

## 1. Visão Geral do Sistema

O **LogiTrack** é um sistema distribuído de otimização de rotas e entregas que moderniza operações logísticas por meio de microsserviços em nuvem. O sistema resolve problemas críticos de atraso, rotas ineficientes e falta de rastreabilidade em tempo real.

**Usuários principais:**
- Gestores Logísticos — planejam e monitoram entregas
- Motoristas — executam as rotas otimizadas

**Estado atual na Fase 3:** Arquitetura evoluída para cloud AWS com escalabilidade horizontal, Circuit Breaker, API Gateway e comunicação híbrida síncrona/assíncrona.

---

## 2. Requisitos Não Funcionais (RNFs)

| RNF | Descrição | Solução Adotada |
|-----|-----------|-----------------|
| Desempenho | Resposta em tempo real para rastreamento | REST síncrono + ECS otimizado |
| Escalabilidade | Suportar aumento de veículos e entregas | Auto Scaling na AWS |
| Confiabilidade | Informações precisas e consistentes | PostgreSQL + Retry |
| Resiliência | Funcionar mesmo com falhas externas | Circuit Breaker + Bulkhead |
| Disponibilidade | Acessível durante toda a operação | Multi-AZ na AWS |

---

## 3. Estilo Arquitetural

O LogiTrack adota **Microsserviços com Clean Architecture/Hexagonal**, combinando:
- Isolamento da lógica de negócio via Ports and Adapters
- Comunicação híbrida (REST + RabbitMQ)
- Orquestração via API Gateway

---

## 4. Estratégia de Cloud e Implantação

### Modelo de Serviço: PaaS — AWS

| Serviço AWS | Uso no LogiTrack |
|-------------|-----------------|
| Amazon ECS | Orquestração dos microsserviços em containers |
| Amazon RDS (PostgreSQL) | Persistência de dados |
| Amazon SQS | Suporte ao Message Broker |
| Auto Scaling Groups | Escalabilidade horizontal automática |
| Application Load Balancer | Distribuição de carga entre instâncias |

### Escalabilidade
- **Horizontal:** novas instâncias de containers são provisionadas automaticamente quando CPU > 70%
- **Multi-AZ:** serviços replicados em múltiplas zonas de disponibilidade

---

## 5. Análise de Fragilidade e Mitigação

**Ponto Frágil:** Dependência da API de Mapas externa (Google Maps/OpenStreetMap). Em caso de indisponibilidade, o Route Service ficaria bloqueado.

**Mitigação:**
- Circuit Breaker monitora a taxa de falha nas chamadas à API externa
- Ao atingir o threshold, o circuito abre e retorna uma rota em cache (fallback)
- Retry com Backoff Exponencial realiza novas tentativas após intervalos crescentes
- Cache de rotas recentes no Redis para respostas offline

---

## 6. Decisões Arquiteturais (ADRs)

| ADR | Título | Status |
|-----|--------|--------|
| [ADR 0001](../adrs/0001-estrategia-nuvem.md) | Estratégia de Nuvem e Escalabilidade | Aceito |
| [ADR 0002](../adrs/0002-padrao-resiliencia.md) | Padrões de Resiliência | Aceito |
| [ADR 0003](../adrs/0003-modelo-comunicacao.md) | Modelo de Comunicação | Aceito |

---

## 7. Parecer Técnico Final

O LogiTrack representa a solução arquitetural mais adequada para o cliente logístico por combinar modernidade tecnológica com pragmatismo operacional. A adoção de microsserviços com Clean Architecture garante manutenibilidade e evolução independente dos serviços. A implantação em PaaS na AWS elimina a sobrecarga de gerenciamento de infraestrutura, permitindo foco total na lógica de negócio. Os padrões de resiliência (Circuit Breaker, Bulkhead, Retry) garantem que falhas pontuais não se propaguem em cascata, mantendo a disponibilidade mesmo em cenários adversos. A comunicação híbrida otimiza tanto a performance das operações críticas quanto a resiliência dos processos secundários. O resultado é um sistema escalável, resiliente e preparado para crescer junto com o negócio do cliente.

---

## 8. Referências

- Pressman, R. S. (2021). *Engenharia de Software*. McGraw Hill.
- Richards, M., & Ford, N. (2020). *Fundamentals of Software Architecture*. O'Reilly.
- C4 Model: https://c4model.com
- AWS ECS: https://docs.aws.amazon.com/ecs/
- RabbitMQ: https://www.rabbitmq.com/documentation.html
