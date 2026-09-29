---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 5
url: https://github.com/VictorNascimento14/ZeroSpend/pull/5
branch: fix/titulos-do-kit
tags: [pr, frontend, design-system]
status: merged
---

# PR #5 — fix(tema): pôr os títulos do kit nos tamanhos padrão do Tailwind

## 🎯 Contexto

Fora da ordem de [[2026-09-29-plano-da-v1-do-frontend]]: apareceu ao adicionar os primitivos base
(ordem 4). O [[2026-09-29-pr-003-tema]] criou `text-h1`…`text-h4` e ensinou o `cn` de
`src/lib/utils.ts` a tratá-los como tamanho de fonte. Mas os primitivos do registro `base-nova`
importam `cn` **direto do pacote** (`import { cn } from "cn"`), não do `@/lib/utils`, e a
configuração nunca chega neles: `<CardTitle className="text-h4 text-foreground">` perderia o título.

## 🔧 Mudanças

- `src/app/globals.css`: os títulos do kit ocupam os tamanhos padrão do Tailwind, com o peso, o
  tracking e a altura de linha do kit — H1 `text-5xl` (48px/800), H2 `text-4xl` (40px/700), H3
  `text-3xl` (32px/700), H4 `text-2xl` (24px/700). `text-h1`…`text-h4` deixam de existir.
- `src/lib/utils.ts`: volta a ser só `export { cn } from "cn"`, como o `shadcn init` gera.
- `src/app/page.tsx`: `text-h3` → `text-3xl`.

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum.

## 🧠 Decisões técnicas

- **Nome padrão em vez de remendar os primitivos.** A alternativa era trocar o import de cada
  primitivo gerado (agora e a cada `shadcn add`) e travar isso com lint. Com os tamanhos padrão, o
  `cn` do pacote já sabe que `text-3xl` é tamanho, e nenhum arquivo gerado muda.
- **O preço:** o nome da classe não diz mais "H3". A correspondência está no comentário do CSS, em
  [[linguagem-visual]] e em [[Tema]].
- `text-lg` e `text-xl` (18 e 20px) continuam existindo, fora da escala do kit. Zerar os tamanhos,
  como as cores, apagaria em silêncio o tamanho de um primitivo futuro que os use.

## ⚠️ Armadilhas e aprendizados

- **Configuração do `cn` não alcança o que vem do registro.** Os primitivos do `base-nova` importam
  `cn` do pacote. Token com nome fora do padrão numa família de valores fixos (tamanho de fonte,
  sombra, canto) é mal classificado pelo `cn`: vira cor, ou fica sem conflito com a classe que devia
  substituir. Cor não sofre disso — para o `cn`, `text-*` desconhecido é cor, e é mesmo.
- Classe `font-*` explícita vence o peso do título: `<CardTitle className="text-2xl">` sai com o
  `font-medium` do `CardTitle`. Para H4 dentro de um `CardTitle`, some `font-bold`.

## 🧪 Como testar

1. `npm run dev` e abra `http://localhost:3000`: o título continua em 32px/700 (conferido num
   Chromium headless nos dois modos, idêntico ao do [[2026-09-29-pr-003-tema]]).
2. Tema compilado: `text-3xl` e `text-5xl` saem com tamanho, altura, tracking e peso do kit;
   `text-h3` não gera CSS. `cn("text-3xl", "text-foreground")` mantém as duas classes.

## 🚫 O que não foi verificado

Nada além disso — só a página provisória usa título por enquanto.

## 📎 Documentação afetada

- [[Tema]]
- [[linguagem-visual]]
- [[2026]] (changelog)
