# ADR 0002 — Padrões de Resiliência

**Status:** Aceito  
**Data:** 2026-05  
**Autor:** Camilly Christine S. Floriano

## Contexto

Em uma arquitetura de microsserviços, falhas em um serviço podem se propagar em cascata para outros serviços dependentes — o chamado "efeito dominó". No LogiTrack, se o serviço de mapas externos (Google Maps) ficar lento ou indisponível, isso poderia travar o Route Service e consequentemente o API Gateway, derrubando todo o sistema. É necessário implementar padrões de resiliência para isolar falhas.

## Decisão

Adotar os seguintes padrões de resiliência:
- **Circuit Breaker** para interromper chamadas a serviços instáveis
- **API Gateway** como ponto único de entrada com rate limiting
- **Retry com Backoff Exponencial** para tentativas automáticas em falhas transitórias
- **Bulkhead** para isolar pools de recursos por serviço

## Alternativas Rejeitadas

| Alternativa | Motivo da Rejeição |
|-------------|-------------------|
| Timeout simples | Não impede acúmulo de requisições em fila durante indisponibilidade |
| Sem padrão de resiliência | Risco crítico de cascata de falhas em produção |
| Service Mesh (Istio) | Complexidade excessiva para o tamanho atual do projeto |

## Justificativa

Conforme Pressman (2021), sistemas distribuídos devem ser projetados para falhar graciosamente. O padrão Circuit Breaker, descrito por Martin Fowler, monitora as chamadas entre serviços e, ao detectar uma taxa de falha acima do threshold configurado, abre o circuito e retorna uma resposta de fallback imediatamente — evitando sobrecarga no serviço com problema. No LogiTrack, isso foi aplicado na comunicação entre o Route Service e a API de Mapas externa, conforme documentado na atividade de microsserviços do Ciclo 3.

## Trade-offs

- **Vantagem:** Isolamento de falhas — um serviço instável não derruba os demais
- **Vantagem:** Resposta rápida ao usuário via fallback em vez de timeout longo
- **Desvantagem:** Complexidade adicional na configuração dos thresholds do Circuit Breaker
- **Desvantagem:** Respostas de fallback podem ser dados desatualizados ou parciais

## Impacto nos RNFs

- **Resiliência:** ✅ Atendido via Circuit Breaker e Bulkhead
- **Disponibilidade:** ✅ Atendido via API Gateway com rate limiting
- **Confiabilidade:** ✅ Atendido via Retry com Backoff Exponencial

## Referências

- Pressman, R. S. (2021). *Engenharia de Software*. McGraw Hill.
- Fowler, M. Circuit Breaker Pattern. https://martinfowler.com/bliki/CircuitBreaker.html
- Richards, M., & Ford, N. (2020). *Fundamentals of Software Architecture*. O'Reilly.
