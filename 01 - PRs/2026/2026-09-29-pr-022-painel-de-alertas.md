---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 22
url: https://github.com/VictorNascimento14/ZeroSpend/pull/22
branch: feat/painel-de-alertas
tags: [pr, frontend, dashboard, alertas]
status: aberto
---

# PR #22 — feat(dashboard): painel de alertas e duplicidades

## 🎯 Contexto

Ordem 20 de [[2026-09-29-plano-da-v1-do-frontend]]. O briefing pede um "painel lateral de alertas e
duplicidades" com cards de recomendação — "Duplicidade detectada: Canva e Figma…", "Renovação do Zoom
em 5 dias". Com ele, o dashboard tem tudo o que o briefing lista.

## 🔧 Mudanças

- `src/components/dashboard/alerts-panel.tsx` (novo): `AlertsPanel`. Traz as renovações dentro da
  antecedência, da mais próxima, e as duplicidades, da maior economia.
- `/dashboard`: painel e tabela lado a lado a partir de 1600px; abaixo disso, empilhados, com o painel
  antes.
- `format.ts`: `formatList` (`Intl.ListFormat` pt-BR: "Figma, Canva e Miro").
- 1 teste novo (105 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum novo.

## 🧠 Decisões técnicas

- **Texto sem artigo antes da marca:** "Zoom renova em 5 dias", "Ficar só com Figma…". Com "do/da", a
  frase presumiria o gênero de cada marca, e metade sairia errada.
- **Duplicidade diz o que fazer e quanto vale:** "HubSpot e Pipedrive estão na mesma categoria. Ficar
  só com HubSpot economiza R$ 534,60 por mês." Os números vêm de [[Redundancia]], com a mesma regra
  conservadora do KPI.
- **Lado a lado só a partir de 1600px.** Medido: a tabela pede ~860px. Com o painel a 1/3 da largura,
  ela era cortada em 1280, 1440 e 1536px. Com o painel em 22rem, cabe a partir de ~1570px.
- **Empilhado, o painel vem antes da tabela:** alerta é o que pede atenção primeiro.
- **As colunas da lista seguem o painel, não a tela** (`@container` com `@lg:` e `@4xl:`). Ao lado, 1
  coluna; em largura cheia, até 3. Ver [[2026-09-29-colunas-seguem-o-conteiner-nao-a-tela]].
- **Sem "dispensar" ainda:** é a central de alertas (ordem 31).
- **Em revisão não entra no painel:** o briefing fala em alertas e duplicidades; os itens em revisão
  aparecem no KPI e na tabela, e na central de alertas.

## ⚠️ Armadilhas e aprendizados

- A primeira versão pôs o painel a 1/3 da largura desde `xl` — a foto a 1440px mostrou a coluna Status
  cortada. A medida da largura natural da tabela decidiu o ponto de quebra.
- `lg:grid-cols-3` venceu `min-[1600px]:grid-cols-1` no mesmo elemento; mais do que brigar com a ordem
  das variantes, o número de colunas dependia do painel, não da tela.

## 🧪 Como testar

1. `npm test` — 105 testes.
2. Num Chromium headless, com o console limpo:
   - **Exemplo Tecnologia:** GitHub, Google Workspace e Zoom renovam (com valor e data), mais as
     duplicidades em CRM (economiza R$ 534,60) e Design (R$ 99,92);
   - **Clínica Exemplo:** só "Zoom renova em 2 dias";
   - **empresa vazia:** "Nada pedindo atenção agora.";
   - **larguras:** a 390px, o painel fica acima e a tabela rola dentro do cartão; a 1280, 1440 e
     1536px, o painel fica acima em 3 colunas, com a tabela inteira; a 1600 e 1920px, o painel fica ao
     lado em 1 coluna, com a tabela inteira; a página nunca vaza para o lado.

## 🚫 O que não foi verificado

- Dezenas de alertas ao mesmo tempo: a lista cresce sem limite (a central de alertas é quem pagina).

## 📎 Documentação afetada

- [[AlertsPanel]] (novo) · [[2026-09-29-colunas-seguem-o-conteiner-nao-a-tela]] (novo)
- [[Dashboard]] · [[ModeloDeDominio]]
- [[2026]] (changelog)
