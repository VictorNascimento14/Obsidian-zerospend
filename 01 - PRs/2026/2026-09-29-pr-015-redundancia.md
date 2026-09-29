---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 15
url: https://github.com/VictorNascimento14/ZeroSpend/pull/15
branch: feat/redundancia
tags: [pr, frontend, dominio, redundancia]
status: aberto
---

# PR #15 — feat(dominio): ferramentas redundantes e economia estimada

## 🎯 Contexto

Ordem 13 de [[2026-09-29-plano-da-v1-do-frontend]], a última regra de domínio antes das telas. A
especificação marca "Ferramenta Redundante" quando há mais de uma assinatura na mesma categoria, e o
dashboard mostra a "economia potencial". Esta é a regra por trás da tag, do KPI e dos cards de
duplicidade do painel.

## 🔧 Mudanças

- `src/lib/domain/redundancy.ts` (novo):
  - `findRedundancies(assinaturas, empresa)`, que devolve os grupos da mesma categoria;
  - `potentialMonthlySavings(grupos)`;
  - `redundantIds(grupos)`.
- 7 testes novos (67 no total), um deles sobre a demonstração.

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum.

## 🧠 Decisões técnicas

- **Economia conservadora: mantém a mais cara.** A especificação fala em "economia estimada (soma dos
  alertas de duplicidade)", mas não diz qual ferramenta fica. A mais cara do grupo é, em geral, a
  principal da equipe; somar as outras dá um número que o produto consegue sustentar. Manter a mais
  barata prometeria mais (na demonstração, R$ 1.855,00 contra R$ 634,52) com menos base.
- **Custo comparado no mensal equivalente e na moeda padrão** ([[RegrasDeCobranca]]): um anual em real
  e um mensal em dólar se comparam pelo que custam por mês na moeda da empresa.
- **"Outros" não gera redundância** — junta ferramentas que não se substituem (na demonstração,
  ChatGPT Team e Adobe Acrobat Pro).
- **Cancelada fica fora; em revisão entra** (regra 5 do `CLAUDE.md` do repositório).
- **Ordem estável:** grupos pela economia (maior primeiro); dentro do grupo, da mais cara para a mais
  barata, com empate desfeito pelo nome.
- **A "inatividade" da especificação ficou de fora:** não há dado de uso na v1 (depende de integração
  com os fornecedores).

## ⚠️ Armadilhas e aprendizados

- A regra da especificação é grosseira de propósito: duas ferramentas de "Desenvolvimento" (GitHub e
  Vercel) seriam marcadas como redundantes sem ser. A demonstração evita isso com uma ferramenta por
  categoria ([[DadosDeDemonstracao]]). Com uso real, a tag pede um "não é redundante" por par —
  <A DEFINIR> se entra na v1.

## 🧪 Como testar

`npm test` — 67 testes:

- agrupamento;
- cancelada e "Outros" fora, em revisão dentro;
- economia mantendo a mais cara, com anual e dólar;
- ordem e desempate;
- soma e ids.

Na demonstração da Exemplo Tecnologia, os grupos são CRM (economiza o Pipedrive, R$ 534,60) e Design
(economiza o Canva, R$ 99,92), com economia potencial de R$ 634,52; a Clínica Exemplo não tem
redundância.

## 🚫 O que não foi verificado

- Nenhuma tela usa a regra ainda (KPI na ordem 18, tag na 19, painel na 20).

## 📎 Documentação afetada

- [[Redundancia]] (novo)
- [[glossario]] · [[DadosDeDemonstracao]]
- [[ADR-001-frontend-primeiro-com-dados-locais]]
- [[2026]] (changelog)
