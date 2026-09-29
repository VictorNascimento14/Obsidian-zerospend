---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 21
url: https://github.com/VictorNascimento14/ZeroSpend/pull/21
branch: feat/tabela-de-assinaturas
tags: [pr, frontend, dashboard, assinaturas]
status: aberto
---

# PR #21 — feat(dashboard): tabela de assinaturas no dashboard

## 🎯 Contexto

Ordem 19 de [[2026-09-29-plano-da-v1-do-frontend]]. O briefing pede a `<SubscriptionsTable />` no
dashboard, com as colunas Software, Categoria, Valor/mês, Ciclo, Próxima cobrança, Status e Ações.

## 🔧 Mudanças

- `src/components/subscriptions/`:
  - `subscriptions-table.tsx`: a tabela, dentro de um cartão;
  - `rows.ts`: `buildRows`, que calcula cada linha e ordena;
  - `status-badge.tsx`: `StatusBadge` e `RedundantBadge`;
  - `vendor-avatar.tsx`: o monograma do fornecedor.
- `/dashboard`: KPIs e a tabela.
- 2 testes novos (104 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum novo: é o gasto da empresa, na tela dela.

## 🧠 Decisões técnicas

- **Sem coluna "Ações" por enquanto.** Editar, excluir e revisar são as ordens 22 a 24; um menu sem
  efeito seria a tela prometendo o que o código não faz (regra 7 do `CLAUDE.md`). Ela entra com o
  primeiro uso.
- **Monograma, nunca logotipo** (regra do design system). A cor sai do nome, estável entre sessões;
  teto anotado com `ponytail:` — o catálogo de fornecedores (ordem 27) dá a cor dos conhecidos.
- **"Ferramenta redundante" é uma segunda etiqueta, ao lado do status**, não um status: é sinal
  derivado ([[Redundancia]]). A especificação chama de "Alerta de duplicidade", e o glossário, de
  ferramenta redundante.
- **Valor/mês na moeda da empresa**, com o valor original embaixo quando é anual ou em outra moeda
  ("US$ 84,00 por mês", "R$ 7.800,00 por ano"): o número da coluna bate com o gasto mensal do KPI, e o
  original continua visível.
- **Próxima cobrança efetiva** ([[RegrasDeCobranca]]); dentro da janela de alerta, "em 2 dias" em tom de
  alerta. A cancelada não tem próxima cobrança ("—").
- **Ordem:** da cobrança mais próxima para a mais distante — o que o dashboard quer mostrar primeiro —,
  com as canceladas no fim, por nome.
- **Cores de etiqueta com contraste AA nos dois modos**, conferido por script antes da foto: no claro,
  texto `shade` sobre `tint-50`; no escuro, texto `tint-200` sobre `shade-300` a 40% em cima do cartão.
  Os números estão em [[linguagem-visual]].

## ⚠️ Armadilhas e aprendizados

- O escuro não pode usar `shade-300` cheio atrás de texto `tint-200` em toda família: no alerta
  (warning), dá 4,12 e reprova. A 40%, sobre o cartão, passa com folga.

## 🧪 Como testar

1. `npm test` — 104 testes (ordem das linhas, valor por mês, marca de redundância).
2. Num Chromium headless, com o console limpo:
   - a demonstração mostra 15 linhas, começando por GitHub ("01/10/2026 · em 2 dias");
   - HubSpot, Pipedrive, Figma e Canva com "Ferramenta redundante";
   - Gupy com "R$ 650,00 / R$ 7.800,00 por ano";
   - Dropbox no fim, com "—" e "Cancelada";
   - no celular, a página não rola para o lado — só a tabela, dentro do cartão.

## 🚫 O que não foi verificado

- Tabela longa (centenas de linhas): sem paginação nem virtualização; a lista completa com busca e
  filtros é a ordem 25.

## 📎 Documentação afetada

- [[SubscriptionsTable]] (novo) · [[Dashboard]] · [[linguagem-visual]]
- [[2026]] (changelog)
