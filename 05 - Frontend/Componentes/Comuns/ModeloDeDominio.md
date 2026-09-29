---
tipo: funcionalidade
camada: frontend
area: Comuns
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, dominio]
---

# Modelo de domínio

## O que é

Os tipos do ZeroSpend, a lista de categorias e as funções que mostram dinheiro e data em português.
É a base de todas as regras (cobrança, alertas, redundância): funções puras, sem React, sem
`localStorage` e sem `new Date()` escondido ([[ADR-001-frontend-primeiro-com-dados-locais]]).

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/lib/domain/types.ts` | `Subscription`, `Organization` (com `defaultCurrency` e `brlPerUsd`), `IsoDate` e as listas `CURRENCIES`, `BILLING_CYCLES`, `SUBSCRIPTION_STATUSES`, `SUBSCRIPTION_SOURCES` (os tipos derivam delas) |
| `src/lib/domain/validation.ts` | `validateSubscriptionDraft` (erro por campo, em português), `SubscriptionDraft` |
| `src/lib/domain/categories.ts` | `CATEGORIES` (id → rótulo), `CATEGORY_IDS`, `isCategory` |
| `src/lib/domain/dates.ts` | `isIsoDate`, `toIsoDate`, `addMonths`, `addDays`, `daysBetween` |
| `src/lib/domain/format.ts` | `formatMoney`, `formatDate`, `formatDaysUntil`, `plural`, `BILLING_CYCLE_LABELS`, `STATUS_LABELS`, `SOURCE_LABELS` |
| `src/lib/domain/*.test.ts` | os testes (Vitest, fuso de São Paulo) |

## Comportamento

- **`amount` é o valor de uma cobrança**, na moeda da assinatura — o valor por mês é regra de
  cobrança, não campo.
- **Status gravado** é só `active` ("Ativa"), `review_needed` ("Em revisão") ou `cancelled`
  ("Cancelada"). "Ferramenta redundante" e "renova em N dias" são calculados a cada leitura.
- **Categorias** (lista fechada): Comunicação, Reuniões e vídeo, Produtividade, Design, CRM, Marketing,
  Desenvolvimento, Financeiro, RH, Armazenamento, Segurança e Outros.
- **Data sem hora é `YYYY-MM-DD`.** `isIsoDate` recusa data que não existe (`2026-02-30`) e formato
  torto (`2026-10-5`). `toIsoDate(instante)` devolve o dia **local** — é o "hoje" que a tela entrega às
  regras.
- **Dinheiro** só aparece por `formatMoney`: `R$ 8.450,00`, `US$ 12,00` (com espaço não separável).
- **Data** aparece por `formatDate`: `05/10/2026`, cortando a string, sem `Date`.

## Regras de uso

- Nunca `new Date("YYYY-MM-DD")` — vira o dia anterior no Brasil. Ver
  [[2026-09-29-teste-de-data-precisa-do-fuso-do-brasil]].
- Regra pura recebe `today: IsoDate` de quem chama; só a tela lê o relógio.
- Categoria nova entra na lista fechada, com rótulo — nunca texto livre na assinatura.

## Histórico de mudanças

- [[2026-09-29-pr-010-dominio]] — criado: tipos, categorias, datas sem hora e formatação.
- [[2026-09-29-pr-011-regras-de-cobranca]] — empresa com moeda padrão e cotação; `addMonths`. As
  regras de valor mensal e próxima cobrança estão em [[RegrasDeCobranca]].
- [[2026-09-29-pr-012-sementes]] — `addDays`, para as datas relativas de [[DadosDeDemonstracao]].
- [[2026-09-29-pr-013-repositorio-local]] — listas em tempo de execução e `validateSubscriptionDraft`, usada pelo [[RepositorioLocal]].
- [[2026-09-29-pr-014-alertas-de-renovacao]] — `daysBetween` e `formatDaysUntil`, para os [[AlertasDeRenovacao]].
- [[2026-09-29-pr-020-dashboard-kpis]] — `plural` (sem `Intl.PluralRules`, que trata o zero como singular).
