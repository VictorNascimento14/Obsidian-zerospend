---
tipo: funcionalidade
camada: frontend
area: Assinaturas
rota: /assinaturas
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, pagina]
---

# Assinaturas

## O que é

A área `/assinaturas` da [[Casca]]. Descrição na tela: "Todas as assinaturas de software da empresa."

## Onde está no código

`src/app/(app)/assinaturas/page.tsx` — Server Component com os metadados ("Assinaturas · ZeroSpend").

## Comportamento

- **Topo:** `PageHeader` com "Nova assinatura" ([[SubscriptionForm]]).
- **Lista completa** (`SubscriptionsList`, em `src/components/subscriptions/subscriptions-list.tsx`):
  - busca em pílula (software, categoria ou responsável), sem acento nem maiúscula;
  - status: todos, ativas, em revisão, canceladas ou "Ferramentas redundantes";
  - categoria: todas ou uma;
  - ordem: próxima cobrança, maior valor por mês ou nome (A–Z).
- **Contagem:** "15 assinaturas"; com filtro, "2 de 15 assinaturas".
- **Tabela:** a mesma do dashboard ([[SubscriptionsTable]]), com as ações de cada linha.
- **Filtros** ficam na tela (não na URL): sair da página zera.

## Estados (vazio, carregando, erro)

- **Filtro sem resultado:** "Nenhuma assinatura com esses filtros." com "Limpar filtros".
- **Empresa sem assinaturas:** "Nenhuma assinatura cadastrada nesta empresa. Use “Nova assinatura”…".

## Histórico de mudanças

- [[2026-09-29-pr-016-casca]] — criada, com título e descrição.
- [[2026-09-29-pr-023-nova-assinatura]] — botão "Nova assinatura" no topo ([[SubscriptionForm]]).
- [[2026-09-29-pr-027-pagina-assinaturas]] — lista completa com busca, filtros e ordenação.
