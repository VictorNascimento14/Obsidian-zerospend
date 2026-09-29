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

Por enquanto, só o `PageHeader`. O conteúdo chega em: A lista completa com busca, filtros e ordenação (ordem 25), mais cadastrar, editar, excluir e revisar (ordens 21 a 24).

## Histórico de mudanças

- [[2026-09-29-pr-016-casca]] — criada, com título e descrição.
- [[2026-09-29-pr-023-nova-assinatura]] — botão "Nova assinatura" no topo ([[SubscriptionForm]]).
