---
tipo: runbook
camada: frontend
escopo: rodar o app na máquina
ultima_atualizacao: 2026-09-29
tags: [runbook, frontend]
tempo_estimado: 2 min
---

# Runbook — rodar o ZeroSpend local

## Quando usar

Primeira vez numa máquina, ou depois de trocar de branch com `package.json` diferente.

## Passos

1. Node 20.9 ou mais novo (o CI usa o 22).
2. `cd "$ZEROSPEND_REPO"`
3. `npm install`
4. `npm run dev` — sobe em `http://localhost:3000`.

Checks: `npm run lint && npm run type-check && npm run build` — os mesmos que o CI roda em todo PR
([[runbook-ci]]).

## Como saber que deu certo

A página abre sem erro no console. Criado em [[2026-09-29-pr-001-scaffolding]].
