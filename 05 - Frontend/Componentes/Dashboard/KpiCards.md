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

`src/components/dashboard/kpi-cards.tsx`: `KpiCards` e `Kpi`, o card de um indicador, que a página
[[Integracoes]] também usa. O cálculo é `computeKpis` em
`src/lib/domain/kpis.ts` (testes em `kpis.test.ts` e `seed.test.ts`).

## Comportamento

| Card | Número | Legenda |
|---|---|---|
| Gasto mensal | `totalMonthlySpend`, na moeda padrão ([[RegrasDeCobranca]]) | "Com o dólar a R$ 5,40, a cotação da empresa" quando houve conversão; senão "Mensais e anuais, por mês" |
| Economia potencial | [[Redundancia]] | "Por mês, cortando N ferramentas redundantes" ou "Nenhuma ferramenta redundante" |
| Assinaturas ativas | só `active` | "E N em revisão" ou "Nenhuma em revisão" |
| Renovações em N dias (a antecedência da empresa, 7 por padrão) | [[AlertasDeRenovacao]] | "Próxima: GitHub, em 2 dias" ou "Nenhuma nos próximos N dias" |

- Cancelada não entra em nenhum card.
- Ícones na cor do kit, com o par escuro: cerulean (gasto), success (economia), plum (ativas) e
  warning (renovações). O `Kpi` recebe o tom pelo nome da família (`cerulean`, `raspberry`, `plum`,
  `success`, `warning`), e não pelo papel no dashboard.
- Grade: 1 coluna no celular, 2 a partir de `sm`, 4 a partir de `xl`.

## Estados (vazio, carregando, erro)

- **Vazio** (empresa sem assinaturas): tudo zero, com as legendas "Nenhuma…".
- **Carregando:** não acontece aqui — a [[GuardaDeSessao]] só mostra a página com os dados lidos.

## Histórico de mudanças

- [[2026-09-29-pr-020-dashboard-kpis]] — criado.
- [[2026-09-29-pr-032-integracoes]] — `Kpi` exportado, com o tom pelo nome da cor do kit (e `raspberry`).
- [[2026-09-29-pr-036-configuracoes-alertas]] — "Renovações em N dias" com a antecedência da empresa.
