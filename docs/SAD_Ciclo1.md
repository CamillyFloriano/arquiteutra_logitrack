# Documento de Aruitetura de Software (SAD) 
## Sistema para LogiTrack - Sistema de Otimização de Rotas e Entregas

---

# CICLO 1: Visão e Requisitos (Fase 1)

## 1.1 Resumo do Cenário de Negócio
O sistema LogiTrack tem como objetivo modernizar a operação de uma empresa de logística por meio da otimização inteligênte de rotas de entrega. 
Atualmente, muitas empresas enfrentam problemas como atraso, rotas ineficientes e falta de monitoramento em tempo real. O sistema utilizará 
geolocalização para calcular rotas mais eficientes, monitorar veículos em tempo real e fornecer previsões de chegada. Os principais usuários do 
sistema são gestores logísticos e motoristas, que utilizarão a plataforma para planejar e acompanhar entregas.

---

## 1.2 Atributos de Qualidade (RNFs) Priorizados

**1. Desmpenho:**
O sistema precisa calcular rotas e processar dados de localização rapidamente para garantir resposta em tempo real.

**2. Escalabilidade:**
O sistema deve suportar aumento no número de veículos, entregas e usuários sem comprometer o desempenho.

**3. Confiabilidade:**
As informações sobre rotas e entregas precisam ser precisas e consistentes para evitar erros logísticos.

**4. Resiliência:**
O sistema deve continuar funcionando mesmo diate de falhas em serviços externos ou problemas de comunicação.

**5. Disponibilidade:**
O sistema precisa estar acessível durante roda a operação logística para garantir o acompanhamento das entregas.

---

## 1.3 Diagrama de Contexto (C4 Nível 1)
![Diagrama LogiTrack](..docs/diagrama_logitrack.png)
O sistema LogiTrack interage com gestores logísticos, motoriatas e sistemas externos como APIs de mapas e sistemas de estoque.

## 1.4 Classificação da Estratégia

**Classificacao:** Balanceada.

**Justificativa:** A estratégia balanceada busca equilibrar inovação tecnológica e estabilidade. O sistema utiliza tecnologias modernas
para otimização de rotas e processamento de dados em tempo real, mas também se apoia em soluções consolidadas para garantir confiabilidade
e manutenção do sistema, reduzindo riscos no desenvolvimento.

---

# CICLO 2: Blueprint e Decisões (Fase 2)

## 2.1 Diagrama de Containers (C4 Nível 2)

---

## 2.2 Estilo Arquitetural Escolhido

---

## 2.3 Architecture Decision Record (ADR) Principal

---

# CICLO 3: Cloud e Resiliência (Fase 3)

## 3.1 Estratégia de Cloud e Implantação

---

## 3.2 Análise de Fragilidade e Mitigação

---

## 3.3 Parecer Técnico Final
