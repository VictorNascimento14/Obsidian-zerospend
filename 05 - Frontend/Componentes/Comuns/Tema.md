---
tipo: funcionalidade
camada: frontend
area: Comuns
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, design-system]
---

# Tema

## O que é

A fundação visual do app: os tokens do kit do Figma no Tailwind v4, os tokens semânticos do shadcn
apontando para eles, a fonte Inter e o modo escuro. A decisão está na
[[ADR-002-design-system-do-figma-com-shadcn]]; os valores e os contrastes, em [[linguagem-visual]].

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/app/globals.css` | `@theme`: paleta do kit (a padrão zerada), cantos, sombras e tipografia · `@theme inline`: a ponte dos tokens semânticos para as classes (`bg-card`, `text-muted-foreground`…) · `:root` e `.dark`: o valor de cada token semântico em cada modo |
| `src/app/layout.tsx` | Inter (`--font-inter`) e `ThemeProvider` do `next-themes` |
| `src/lib/utils.ts` | `cn()`: junta classes e resolve conflito; conhece `text-h1`…`text-h4` |
| `components.json` | configuração do `shadcn` (estilo `base-nova`, ícones `lucide`) |

## Comportamento

- O modo segue o sistema (`defaultTheme="system"`); a classe `dark` entra no `<html>` antes da
  hidratação, e o `next-themes` guarda a escolha no `localStorage`.
- Classe de cor fora do kit (`bg-slate-50`) não gera CSS nenhum: o elemento fica sem a cor.
- `text-h1`…`text-h4` aplicam tamanho, altura de linha, tracking e peso de uma vez.

## Regras de uso

- Superfície, texto e borda usam token semântico (`bg-card`, `text-foreground`, `border-border`). Cor
  crua do kit fica para estado, sempre com o par `dark:`.
- `ghost` sobre o fundo da página não mostra hover (no claro, `muted` e `background` são o mesmo
  `grey-50`): em cima do fundo, use `outline`; `ghost` vive em superfície branca (cartão, menu).
- Ao juntar classes num componente, use `cn()` — ele sabe que `text-h3` é tamanho, não cor.

## Histórico de mudanças

- [[2026-09-29-pr-003-tema]] — criado: kit no Tailwind, tokens semânticos, Inter e modo escuro.
