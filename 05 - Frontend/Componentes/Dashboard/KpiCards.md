---
tipo: funcionalidade
camada: frontend
area: Dashboard
rota: /dashboard
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, dashboard]
---

# Cards de KPI

## O que é

Os quatro indicadores do topo do [[Dashboard]], da empresa da sessão.

## Onde está no código

`src/components/dashboard/kpi-cards.tsx` (`KpiCards`); o cálculo é `computeKpis` em
`src/lib/domain/kpis.ts` (testes em `kpis.test.ts` e `seed.test.ts`).

## Comportamento

| Card | Número | Legenda |
|---|---|---|
| Gasto mensal | `totalMonthlySpend`, na moeda padrão ([[RegrasDeCobranca]]) | "Com o dólar a R$ 5,40, a cotação da empresa" quando houve conversão; senão "Mensais e anuais, por mês" |
| Economia potencial | [[Redundancia]] | "Por mês, cortando N ferramentas redundantes" ou "Nenhuma ferramenta redundante" |
| Assinaturas ativas | só `active` | "E N em revisão" ou "Nenhuma em revisão" |
| Renovações em 7 dias | [[AlertasDeRenovacao]] | "Próxima: GitHub, em 2 dias" ou "Nenhuma nos próximos 7 dias" |

- Cancelada não entra em nenhum card.
- Ícones na cor de estado do kit: cerulean (gasto), success (economia), plum (ativas), warning
  (renovações), com o par escuro.
- Grade: 1 coluna no celular, 2 a partir de `sm`, 4 a partir de `xl`.

## Estados (vazio, carregando, erro)

- **Vazio** (empresa sem assinaturas): tudo zero, com as legendas "Nenhuma…".
- **Carregando:** não acontece aqui — a [[GuardaDeSessao]] só mostra a página com os dados lidos.

## Histórico de mudanças

- [[2026-09-29-pr-020-dashboard-kpis]] — criado.
