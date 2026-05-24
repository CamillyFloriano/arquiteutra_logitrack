# LogiTrack - Sistema de Otimização de Rotas e Entregas

> Sistema distribuído para otimização inteligente de rotas, monitoramento de veículos em tempo real e previsão de entregas.

## Visão Executiva

O **LogiTrack** moderniza operações logísticas por meio de microsserviços em nuvem, geolocalização em tempo real e comunicação assíncrona. O sistema resolve problemas críticos de atraso, rotas ineficientes e falta de rastreabilidade, atendendo gestores logísticos e motoristas. Na Fase 3, a arquitetura evolui para cloud com escalabilidade horizontal, Circuit Breaker e API Gateway.

## Diagrama C4 - Containers

```mermaid
C4Container
    title LogiTrack — Diagrama de Containers (C4 Nível 2)

    Person(gestor, "Gestor Logístico", "Planeja e monitora entregas")
    Person(motorista, "Motorista", "Executa as rotas")

    System_Boundary(logitrack, "LogiTrack") {
        Container(gateway, "API Gateway", "Node.js", "Ponto único de entrada, roteamento e autenticação")
        Container(rota, "Route Service", "Python/FastAPI", "Calcula e otimiza rotas de entrega")
        Container(rastreio, "Tracking Service", "Node.js", "Monitora veículos em tempo real via GPS")
        Container(notif, "Notification Service", "Python", "Envia alertas e previsões de chegada")
        Container(broker, "Message Broker", "RabbitMQ", "Comunicação assíncrona entre serviços")
        ContainerDb(db, "Database", "PostgreSQL", "Armazena rotas, entregas e histórico")
    }

    System_Ext(maps, "API de Mapas", "Google Maps / OpenStreetMap")
    System_Ext(estoque, "Sistema de Estoque", "ERP externo")

    Rel(gestor, gateway, "Usa", "HTTPS")
    Rel(motorista, gateway, "Usa", "HTTPS")
    Rel(gateway, rota, "Roteia", "REST")
    Rel(gateway, rastreio, "Roteia", "REST")
    Rel(rota, broker, "Publica eventos", "AMQP")
    Rel(broker, notif, "Consome eventos", "AMQP")
    Rel(rota, maps, "Consulta rotas", "HTTPS")
    Rel(rota, db, "Lê/Escreve", "SQL")
    Rel(rastreio, db, "Lê/Escreve", "SQL")
    Rel(gateway, estoque, "Consulta estoque", "REST")
```

## ADRs — Decisões Arquiteturais

| ADR | Título | Status |
|-----|--------|--------|
| [ADR 0001](docs/adrs/0001-estrategia-nuvem.md) | Estratégia de Nuvem e Escalabilidade | Aceito |
| [ADR 0002](docs/adrs/0002-padrao-resiliencia.md) | Padrões de Resiliência | Aceito |
| [ADR 0003](docs/adrs/0003-modelo-comunicacao.md) | Modelo de Comunicação | Aceito |

## SAD — Documento de Arquitetura

O documento completo está em [docs/sad/sad-fase3.md](docs/sad/sad-fase3.md).

## Como Executar Localmente

### Pré-requisitos
- Docker e Docker Compose instalados
- Node.js 18+
- Python 3.11+

### Passos

```bash
# Clone o repositório
git clone https://github.com/CamillyFloriano/arquiteutra_logitrack.git
cd arquiteutra_logitrack

# Suba os serviços
docker-compose up -d

# Acesse a API Gateway
http://localhost:8080
```

## Tecnologias

- **API Gateway:** Node.js
- **Microsserviços:** Python/FastAPI, Node.js
- **Mensageria:** RabbitMQ
- **Banco de Dados:** PostgreSQL
- **Cloud:** AWS (ECS + RDS + SQS)
- **Resiliência:** Circuit Breaker (Resilience4j)

## Referências

- Pressman, R. S. (2021). *Engenharia de Software*. McGraw Hill.
- Richards, M., & Ford, N. (2020). *Fundamentals of Software Architecture*. O'Reilly.
- C4 Model: https://c4model.com
- ADR GitHub: https://adr.github.io
