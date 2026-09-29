---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 6
url: https://github.com/VictorNascimento14/ZeroSpend/pull/6
branch: ui/primitivos-base
tags: [pr, frontend, design-system]
status: aberto
---

# PR #6 — ui(primitivos): adicionar Button, Badge, Card, Avatar, Separator e Skeleton

## 🎯 Contexto

Ordem 4 de [[2026-09-29-plano-da-v1-do-frontend]]. A numeração do GitHub está dois à frente da ordem,
porque o #4 e o #5 entraram fora do plano. Primeiro lote de primitivos: o que a casca e o dashboard
usam em toda tela.

## 🔧 Mudanças

- `npx shadcn@4.21.0 add button badge card avatar separator skeleton`: seis arquivos em
  `src/components/ui/`, sem dependência nova (todas vieram no [[2026-09-29-pr-003-tema]]).
- `card.tsx`: `shadow-sm` na classe base.

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum.

## 🧠 Decisões técnicas

- **A sombra do cartão vai no primitivo, não em cada uso.** O briefing pede "cards brancos com bordas
  finas, sombras sutis"; o `Card` do `base-nova` vem só com o anel de 1px. Como todo cartão do app deve
  sair igual, a classe entra uma vez no primitivo — é a exceção registrada à regra de não editar arquivo
  gerado.
- **O `outline` fica como gerado.** Ele pinta com `background`, que aqui é o fundo da página
  (`grey-50`), e dentro de cartão aparece levemente cinza. Trocar para transparente seria editar outro
  primitivo sem pedido — anotado em [[Primitivos]].
- Nenhuma variante de status no `Badge`: a cor de status vem por mapa de classes literais, no PR da
  tabela.

## ⚠️ Armadilhas e aprendizados

- Os primitivos importam `cn` do pacote — o motivo do [[2026-09-29-pr-005-titulos-do-kit]].
- O `Button` não tem `"use client"` e funciona dentro de Server Component: o módulo do Base UI
  (`@base-ui/react/button`) já declara `'use client'`. `Avatar` e `Separator` declaram no próprio
  arquivo.

## 🧪 Como testar

1. Vitrine temporária (fora do commit) com todos os primitivos e variantes, fotografada num Chromium
   headless nos dois modos, com o console limpo:
   - primária `cerulean` com texto branco no claro, e `cerulean-tint-200` com texto `grey-900` no
     escuro;
   - destrutivo legível nos dois modos;
   - esqueleto visível dentro do cartão;
   - cartão com e sem `shadow-sm`, comparados lado a lado.
2. `grep` nos seis arquivos: nenhuma cor da paleta padrão, nenhuma sombra ou tamanho fora do kit.
3. Lint, type-check e build passam.

## 🚫 O que não foi verificado

- Hover e foco foram conferidos pelas contas de contraste do [[Tema]], não por foto.
- Nenhuma tela usa os primitivos ainda: a página continua provisória.

## 📎 Documentação afetada

- [[Primitivos]] (novo)
- [[ADR-002-design-system-do-figma-com-shadcn]]
- [[2026]] (changelog)
