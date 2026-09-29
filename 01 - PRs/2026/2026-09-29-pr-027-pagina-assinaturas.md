---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 27
url: https://github.com/VictorNascimento14/ZeroSpend/pull/27
branch: feat/pagina-assinaturas
tags: [pr, frontend, assinaturas]
status: merged
---

# PR #27 — feat(assinaturas): lista completa com busca, filtros e ordenação

## 🎯 Contexto

Ordem 25 de [[2026-09-29-plano-da-v1-do-frontend]]. A [[visao-de-produto]] promete, em Assinaturas,
"lista completa com busca e filtros". O dashboard mostra tudo, mas na ordem das cobranças; aqui dá para
achar e comparar.

## 🔧 Mudanças

- `src/components/subscriptions/subscriptions-list.tsx` (novo): `SubscriptionsList`, com a busca, os
  filtros de status e de categoria, a ordem e a contagem ("2 de 15 assinaturas").
- `subscription-rows-table.tsx` (novo): `SubscriptionRowsTable`, o corpo da tabela que o dashboard e a
  página usam.
- `rows.ts`: `filterRows`, `sortRows` e os tipos `RowFilters`, `StatusFilter` e `SortKey`.
- `/assinaturas`: `PageHeader` com "Nova assinatura" e a lista.
- 3 testes novos (114 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

A busca procura também pelo responsável (nome de pessoa). Fica tudo no navegador.

## 🧠 Decisões técnicas

- **Busca sem acento nem maiúscula** (`normalize("NFD")` sem diacríticos): "clinica" acha "Clínica". Os
  campos buscados são software, categoria e responsável.
- **"Ferramentas redundantes" é uma opção do filtro de status**, e não um controle à parte: é um sinal
  derivado ([[Redundancia]]), mas quem filtra pensa nele como um estado.
- **Ordem por próxima cobrança (a do dashboard), maior valor por mês ou nome;** empate por nome.
- **Filtros no estado do componente, não na URL:** o plano não pediu link para uma lista filtrada.
  Voltar à página zera os filtros.
- **Um corpo de tabela só:** `SubscriptionRowsTable` sai do cartão do dashboard; as colunas e ações
  continuam iguais nos dois lugares.
- **Busca em pílula com a lupa à direita**, como o campo de busca do kit ([[linguagem-visual]]).

## ⚠️ Armadilhas e aprendizados

- O placeholder cabe ou não cabe conforme a largura: a primeira versão cortava ("…ou respo"). Foi
  medido no navegador (largura do texto contra a do campo) em 390 e 1440px.

## 🧪 Como testar

1. `npm test` — 114 testes (busca sem acento; status, redundância e categoria; ordem por valor e por
   nome).
2. Num Chromium headless, com o console limpo:
   - o dashboard continua com as 15 linhas;
   - em `/assinaturas`, "15 assinaturas";
   - a busca "pessoa" traz Figma e Canva ("2 de 15");
   - "Ferramentas redundantes" traz HubSpot, Figma, Pipedrive e Canva;
   - CRM traz HubSpot e Pipedrive;
   - "Maior valor por mês" começa por HubSpot, Google Workspace, RD Station Marketing e Slack;
   - uma busca sem resultado mostra "Limpar filtros", que volta tudo;
   - no celular, os filtros empilham e a página não vaza.

## 🚫 O que não foi verificado

- Centenas de assinaturas: sem paginação (a v1 pensa em PMEs, de dezenas).

## 📎 Documentação afetada

- [[Assinaturas]] · [[SubscriptionsTable]]
- [[2026]] (changelog)
