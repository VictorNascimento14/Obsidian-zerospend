---
tipo: funcionalidade
camada: frontend
area: Dashboard
rota: /dashboard
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, dashboard, alertas]
---

# Painel de alertas e duplicidades

## O que é

O "painel lateral de alertas e duplicidades" do briefing: cards de recomendação com o que pede
atenção na empresa da sessão.

## Onde está no código

`src/components/dashboard/alerts-panel.tsx` (`AlertsPanel`), no `/dashboard`. Cada alerta é um
`AlertItem`, com o texto de `describeAlert` ([[AlertsCenter]]).

## Comportamento

- **Renovações** ([[AlertasDeRenovacao]]), da mais próxima: "GitHub renova em 2 dias — US$ 84,00 em
  01/10/2026." Ícone de sino, em tom de alerta.
- **Duplicidades** ([[Redundancia]]), da maior economia: "Duplicidade em CRM — HubSpot e Pipedrive estão
  na mesma categoria. Ficar só com HubSpot economiza R$ 534,60 por mês." Ícone de cópia, em tom de
  perigo.
- **Texto sem artigo antes da marca** ("Zoom renova…"): "do/da" presumiria o gênero de cada uma.
- **Só o que não foi dispensado.** Dispensar é na central de alertas, e o link "Ver todos" (nome
  acessível "Ver todos os alertas") leva até ela.
- **Lugar na página:** ao lado da tabela a partir de 1700px (painel de 22rem); abaixo disso, antes da
  tabela, em largura cheia. O ponto de quebra vem da largura medida da tabela (~900px).
- **Colunas da lista pela largura do painel** (consulta de contêiner): 1 coluna ao lado ou no celular,
  2 a partir de `@lg`, 3 a partir de `@4xl`.

## Estados (vazio, carregando, erro)

- **Vazio:** "Nada pedindo atenção agora." com o ícone de confirmação em verde.

## Regras de uso

- Recomendação nova (em revisão, inatividade) entra aqui só se o briefing ou a especificação pedirem;
  a lista completa é a central de alertas ([[AlertsCenter]]).

## Histórico de mudanças

- [[2026-09-29-pr-022-painel-de-alertas]] — criado.
- [[2026-09-29-pr-024-editar-assinatura]] — lado a lado a partir de 1700px (a tabela cresceu).
- [[2026-09-29-pr-033-central-de-alertas]] — sem os dispensados, com "Ver todos"; texto em `AlertItem`/`describeAlert`.
- [[2026-09-29-pr-034-notificacoes]] — lê os alertas por `useAlerts()`, o mesmo do sino e da central.
