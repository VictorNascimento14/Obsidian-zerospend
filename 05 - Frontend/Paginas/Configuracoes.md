---
tipo: funcionalidade
camada: frontend
area: Configuracoes
rota: /configuracoes
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, pagina]
---

# Configurações

## O que é

A área `/configuracoes` da [[Casca]]. Descrição na tela: "Empresa, moeda, alertas e membros."

## Onde está no código

`src/app/(app)/configuracoes/page.tsx`, Server Component com os metadados ("Configurações ·
ZeroSpend"), e as seções, uma embaixo da outra, em até `max-w-3xl`.

## Comportamento

- **Empresa:** nome, moeda padrão e cotação do dólar ([[OrganizationSettings]]).
- **Alertas:** antecedência e canais ([[AlertSettings]]).
- Chega depois: membros (ordem 35).

## Histórico de mudanças

- [[2026-09-29-pr-016-casca]] — criada, com título e descrição.
- [[2026-09-29-pr-035-configuracoes-empresa]] — a seção Empresa.
- [[2026-09-29-pr-036-configuracoes-alertas]] — a seção Alertas.
