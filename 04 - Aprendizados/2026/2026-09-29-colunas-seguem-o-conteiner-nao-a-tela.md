---
tipo: aprendizado
data: 2026-09-29
contexto: PR #22 (painel de alertas)
tags: [aprendizado, frontend, layout]
autor: VictorNascimento14
---

# Colunas de um componente seguem o contêiner, não a tela

## O sintoma

O painel de alertas do dashboard tem uma lista de cards. Em largura cheia, ela devia ter 3 colunas;
ao lado da tabela (painel de 22rem), 1. Com `lg:grid-cols-3 min-[1600px]:grid-cols-1`, a 1920px o
painel estava ao lado e a lista continuava em 3 colunas espremidas.

## A causa

Duas coisas. A imediata: no mesmo elemento, `lg:grid-cols-3` venceu `min-[1600px]:grid-cols-1` (a
ordem das variantes no CSS gerado não era a esperada). A de fundo: o número de colunas não depende da
tela, e sim da largura que o **painel** tem — que muda com o layout da página, não com o `lg`.

## A correção

Consulta de contêiner, nativa no Tailwind v4: `@container` no conteúdo do cartão e
`@lg:grid-cols-2 @4xl:grid-cols-3` na lista. Ver [[2026-09-29-pr-022-painel-de-alertas]].

## Como evitar

- Componente que pode aparecer em larguras diferentes (lado, cheio, celular) decide as colunas pelo
  próprio contêiner (`@container` + `@sm:`, `@lg:`…), não pela tela.
- Antes de pôr duas colunas lado a lado, meça a largura natural do conteúdo mais largo
  (`scrollWidth` do contêiner da tabela): ela decide o ponto de quebra.
