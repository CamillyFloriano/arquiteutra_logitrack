# ADR 0003 — Modelo de Comunicação

**Status:** Aceito  
**Data:** 2026-05  
**Autor:** Camilly Christine S. Floriano

## Contexto

O LogiTrack possui serviços que precisam se comunicar de formas diferentes. O Tracking Service precisa responder em tempo real (baixa latência), enquanto o processamento de notificações e atualizações de rota pode ocorrer de forma assíncrona. Adotar um modelo único de comunicação (apenas síncrono ou apenas assíncrono) comprometeria either a performance ou a consistência do sistema.

## Decisão

Adotar um **modelo híbrido de comunicação**:
- **Síncrono (REST/HTTP)** para operações que exigem resposta imediata (consulta de rotas, rastreamento ao vivo)
- **Assíncrono (RabbitMQ/AMQP)** para operações que toleram processamento posterior (notificações, atualizações de status de entrega)

## Alternativas Rejeitadas

| Alternativa | Motivo da Rejeição |
|-------------|-------------------|
| 100% Síncrono | Cria acoplamento forte entre serviços e risco de cascata de falhas |
| 100% Assíncrono | Inadequado para operações que exigem resposta imediata ao usuário |
| gRPC | Curva de aprendizado elevada e menor suporte a ferramentas de monitoramento |

## Justificativa

Conforme Richards & Ford (2020), a escolha entre comunicação síncrona e assíncrona deve ser guiada pelo requisito de negócio de cada operação. Operações onde o usuário aguarda uma resposta imediata (ex: "qual a rota mais rápida agora?") devem ser síncronas. Já operações onde o resultado pode ser processado em background (ex: "notifique o cliente quando o pedido sair para entrega") devem ser assíncronas para não bloquear o fluxo principal. O uso do RabbitMQ como Message Broker garante entrega confiável de mensagens mesmo que o serviço consumidor esteja temporariamente indisponível, aumentando a resiliência do sistema conforme demonstrado na atividade extra-classe do Ciclo 3.

## Trade-offs

- **Vantagem:** Melhor performance — operações críticas não são bloqueadas por processos secundários
- **Vantagem:** Maior resiliência — falha no Notification Service não impacta o Route Service
- **Desvantagem:** Consistência eventual — o cliente pode não ser notificado em tempo real
- **Desvantagem:** Maior complexidade operacional — necessidade de monitorar filas e mensagens perdidas

## Impacto nos RNFs

- **Desempenho:** ✅ Atendido — operações críticas respondem em tempo real
- **Resiliência:** ✅ Atendido — falhas isoladas não propagam cascata
- **Escalabilidade:** ✅ Atendido — consumidores da fila escalam independentemente

## Referências

- Richards, M., & Ford, N. (2020). *Fundamentals of Software Architecture*. O'Reilly.
- Pressman, R. S. (2021). *Engenharia de Software*. McGraw Hill.
- RabbitMQ Documentation: https://www.rabbitmq.com/documentation.html
