---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 1
url: https://github.com/VictorNascimento14/ZeroSpend/pull/1
branch: chore/scaffolding
tags: [pr, infra, scaffolding]
status: merged
---

# PR #1 — chore: scaffolding Next.js 16 + TypeScript + Tailwind v4 + ESLint

## 🎯 Contexto

Ordem 1 de [[2026-09-29-plano-da-v1-do-frontend]]. É a base de todo o resto: sem um projeto que
builda, não há onde instalar o tema do Figma nem as telas.

## 🔧 Mudanças

- Projeto do `create-next-app@16.3.7` com App Router, `src/`, TypeScript, Tailwind v4 e ESLint, na
  opção `--empty` (sem a página de exemplo).
- `package.json`: scripts `dev`, `build`, `start`, `lint` (`eslint --max-warnings=0`) e `type-check`
  (`next typegen && tsc --noEmit`).
- `src/app/layout.tsx`: `lang="pt-BR"` e metadados do ZeroSpend.
- `src/app/page.tsx`: página mínima, só para o build ter o que empacotar.

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum.

## 🧠 Decisões técnicas

- `type-check` roda `next typegen` antes do `tsc`: o Next 16 declara `LayoutProps`/`PageProps` como
  tipos globais gerados em `.next/types`, e num checkout limpo (o CI) o `tsc` sozinho não os acha.
- `--max-warnings=0` no lint: aviso que ninguém lê vira erro que alguém conserta.
- npm, não pnpm: o CI usa `npm ci` com o cache do `setup-node`, como no Alivium.
- Sem React Compiler e sem `typedRoutes` — nenhum dos dois foi pedido, e cada um é uma peça a mais
  para depurar.
- O `AGENTS.md` do scaffold ficou de fora (`--no-agents-md`): as regras moram no `CLAUDE.md`. O
  `.gitignore` do bootstrap já cobria o do scaffold, mais `.claude/`.

## ⚠️ Armadilhas e aprendizados

- O `create-next-app@16.3.7` instala React `19.2.8`, não o `19.3.0` que o `npm view react` mostra — o
  Next fixa a versão com que foi testado. Não "atualize" o React à parte.

## 🧪 Como testar

Ver [[runbook-rodar-local]]. Lint, type-check e build passam.

## 🚫 O que não foi verificado

Nada de interface ainda — a página é um texto.

## 📎 Documentação afetada

- [[runbook-rodar-local]]
- [[2026]] (changelog)
