---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 20
url: https://github.com/VictorNascimento14/ZeroSpend/pull/20
branch: feat/dashboard-kpis
tags: [pr, frontend, dashboard]
status: aberto
---

# PR #20 — feat(dashboard): cards de KPI com gasto mensal e economia

## 🎯 Contexto

Ordem 18 de [[2026-09-29-plano-da-v1-do-frontend]]. O briefing abre o dashboard com quatro cards —
gasto total mensal, economia potencial estimada, assinaturas ativas e alertas urgentes de renovação.
As regras já existiam ([[RegrasDeCobranca]], [[Redundancia]], [[AlertasDeRenovacao]]); aqui elas
aparecem.

## 🔧 Mudanças

- `src/lib/domain/kpis.ts` (novo): `computeKpis(assinaturas, empresa, hoje)` junta os quatro números
  e o que as legendas precisam.
- `src/components/dashboard/kpi-cards.tsx` (novo): os quatro cards, com ícone na cor de estado do kit.
- `store.ts`: `useOrganizationData()`, com a empresa da sessão e as assinaturas dela.
- `format.ts`: `plural(n, singular, plural)`.
- `/dashboard`: `PageHeader` + `KpiCards`.
- Testes: 102 no total.

## 🕵️ Dado sensível (LGPD e sigilo)

O gasto da empresa aparece na tela — é o produto. Nada sai do navegador.

## 🧠 Decisões técnicas

- **Cálculo puro, componente só desenha.** `computeKpis` é testado com a demonstração; o card não
  calcula nada.
- **A cotação aparece quando pesa.** Se alguma assinatura em outra moeda entrou no gasto, a legenda diz
  "Com o dólar a R$ 5,40, a cotação da empresa" — a promessa do [[2026-09-29-pr-018-criar-conta]] de
  mostrar a cotação usada.
- **"Assinaturas ativas" conta só as ativas**; as em revisão aparecem na legenda ("E 2 em revisão"). A
  cancelada não aparece em nada.
- **"Renovações em 7 dias"** diz a janela no título (a antecedência vira configurável na ordem 34) e
  nomeia a próxima na legenda.
- **Ícone na cor de estado do kit** (cerulean, success, plum, warning), com o par `dark:`, por mapa de
  classes literais.
- **`plural` sem `Intl.PluralRules`:** no pt-BR, ele trata o zero como singular ("0 ferramenta").
- **"Hoje" vem da tela:** `toIsoDate(new Date())` no componente, que só renderiza no cliente (dentro
  da [[GuardaDeSessao]]).

## ⚠️ Armadilhas e aprendizados

- Nenhuma nova.

## 🧪 Como testar

1. `npm test` — 102 testes; os KPIs da demonstração estão em `seed.test.ts`.
2. Num Chromium headless, com o console limpo:
   - **Exemplo Tecnologia:** R$ 7.444,75 ("Com o dólar a R$ 5,40…"), R$ 634,52 ("cortando 2
     ferramentas redundantes"), 12 ativas ("E 2 em revisão") e 3 renovações ("Próxima: GitHub, em 2
     dias");
   - **Clínica Exemplo:** R$ 537,80, sem redundância, 4 ativas e 1 renovação (Zoom);
   - **empresa nova:** tudo zero, com "Nenhuma…" nas legendas.

## 🚫 O que não foi verificado

- Página aberta de um dia para o outro: o "hoje" só muda na próxima renderização.

## 📎 Documentação afetada

- [[KpiCards]] (novo) · [[Dashboard]] · [[ModeloDeDominio]]
- [[2026]] (changelog)
