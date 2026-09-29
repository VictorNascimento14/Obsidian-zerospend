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

Por enquanto, só o `PageHeader`. O conteúdo chega em: Os KPIs (ordem 18), a `SubscriptionsTable` (ordem 19) e o painel de alertas e duplicidades (ordem 20).

## Histórico de mudanças

- [[2026-09-29-pr-016-casca]] — criada, com título e descrição.
