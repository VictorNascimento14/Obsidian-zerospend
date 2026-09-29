---
tipo: adr
numero: 2
data: 2026-09-29
status: aceito
tags: [adr, frontend, design-system]
autor: VictorNascimento14
---

# ADR-002 — Design system do Figma sobre shadcn/ui e Tailwind v4

## Contexto

O briefing pede Next.js App Router, TypeScript, Tailwind, shadcn/ui, ícones `lucide-react`, modo
escuro com o prefixo `dark:` e o arquivo do Figma **SaaS Design System & UI Kit (Community)** como
**referência visual exata**. O texto do briefing descreve o clima com nomes do Tailwind ("fundo
`slate-50`", "primária indigo/blue", `rounded-xl`), mas o arquivo do Figma tem tokens próprios: a
primária é **Cerulean `#0068E9`**, os cinzas são uma escala própria (`#F3F3F4` → `#151419`) e há
famílias de sucesso, alerta e perigo com tints e shades. O arquivo foi lido pelo embed público em
2026-09-29 (a API exige login); a transcrição está em [[linguagem-visual]].

## Decisão

1. **Stack:** Next.js 16 (App Router, `src/`), React 19, TypeScript estrito, Tailwind CSS v4,
   shadcn/ui no preset `base-nova` (primitivos Base UI — o mesmo preset do Processual), `lucide-react`,
   `next-themes` para o modo escuro, `sonner` para toasts.
2. **O Figma vence o nome aproximado do briefing.** Onde o briefing diz `indigo`/`slate`, vale o token
   do kit — é o que o próprio briefing manda ao chamar o arquivo de "referência visual exata".
3. **Só existem as cores do kit.** A paleta padrão do Tailwind é zerada (`--color-*: initial`) e o tema
   declara as famílias com o nome do kit: `cerulean`, `raspberry`, `plum`, `success`, `warning`,
   `danger` (cada uma com `tint-50…300`, base e `shade-100…300`) e `grey-50…900`. Classe como
   `bg-slate-50` deixa de existir — e o build prova isso.
4. **Os tokens semânticos do shadcn apontam para o kit:** `primary` → `cerulean`; `destructive` →
   `danger-shade-200` (era `danger-shade-100`; ver Atualizações); `border` → `grey-100`; `input` → `grey-300`; `ring` → `cerulean`; `muted` →
   `grey-50`; `muted-foreground` → `grey-500`. O `secondary` do shadcn continua **neutro**; o
   "Secondary" do kit (Raspberry) é usado pelo nome `raspberry`, para não confundir os dois.
5. **Tipografia Inter** com a escala do kit (H1–H4 e corpo a 145%); **cantos** 8px em botão e campo,
   12px em cartão, pílula em badge e busca; **sombras** do kit (X-Small a Large).
6. **Modo escuro derivado da escala de cinza do kit** (fundo `grey-900`, superfície `grey-800`, borda
   `grey-700`, texto `grey-50`, texto secundário `grey-300`, primária em texto `cerulean-tint-200`). Não
   entra cor de fora do kit.
7. **Contraste AA é regra, não sugestão:** texto de alerta usa `warning-shade-300`, de perigo
   `danger-shade-100`, de sucesso `success-shade-200`; texto secundário no mínimo `grey-500`. As contas
   estão em [[linguagem-visual]].

## Consequências

- ✅ Cada classe do código tem correspondente direto no Figma (`cerulean-tint-50` = *Primary Tint 50*).
- ✅ Um `bg-slate-*` ou `text-indigo-*` esquecido não compila em cor nenhuma — o `grep` no CSS do build
  acusa.
- ⚠️ Os componentes do shadcn são **gerados** e vivem no repo (`src/components/ui/`). Ajuste de tela se
  faz por `className`/variante, não editando o primitivo; mudança no primitivo é PR próprio.
- ⚠️ O modo escuro não tem referência no Figma: foi desenhado aqui, com os mesmos tokens.

## Atualizações

- **2026-09-29, [[2026-09-29-pr-003-tema]]:** o `destructive` do modo claro passou de
  `danger-shade-100` para `danger-shade-200`. O botão e a badge destrutivos do `base-nova` pintam o
  fundo com a própria cor a 10%, e sobre o fundo da página (`grey-50`) o `shade-100` dá 4,10:1 —
  reprova o AA da decisão 7. Com o `shade-200`: 5,72 na página e 6,32 no cartão. No escuro, o
  `destructive` é `danger-tint-100`. A tabela completa dos tokens semânticos está em
  [[linguagem-visual]].

## Implementado em

- [[2026-09-29-pr-003-tema]] — paleta do kit, tokens semânticos, Inter e modo escuro.

TODO: acrescentar os PRs dos primitivos conforme forem mergeados.
