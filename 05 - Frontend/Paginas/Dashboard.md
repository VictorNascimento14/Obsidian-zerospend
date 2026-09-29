---
tipo: funcionalidade
camada: frontend
area: Dashboard
rota: /dashboard
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, pagina]
---

# Dashboard

## O que é

A área `/dashboard` da [[Casca]]. Descrição na tela: "Gasto mensal, economia potencial e as renovações que vêm aí."

## Onde está no código

`src/app/(app)/dashboard/page.tsx` — Server Component com os metadados ("Dashboard · ZeroSpend").

## Comportamento

`PageHeader`, os quatro [[KpiCards]], a [[SubscriptionsTable]] e o [[AlertsPanel]] — tudo o que o
briefing lista. A partir de 1600px, tabela e painel ficam lado a lado; abaixo disso, o painel vem antes
da tabela.

## Histórico de mudanças

- [[2026-09-29-pr-016-casca]] — criada, com título e descrição.
- [[2026-09-29-pr-020-dashboard-kpis]] — os cards de KPI.
- [[2026-09-29-pr-021-tabela-de-assinaturas]] — a tabela de assinaturas.
- [[2026-09-29-pr-022-painel-de-alertas]] — o painel de alertas e duplicidades.
- [[2026-09-29-pr-023-nova-assinatura]] — botão "Nova assinatura" no topo ([[SubscriptionForm]]).
