---
tipo: funcionalidade
camada: frontend
area: Comuns
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, design-system]
---

# Primitivos

## O que é

Os componentes de base do shadcn/ui (estilo `base-nova`, sobre Base UI) com que as telas são montadas.
São **gerados** pelo CLI e vivem em `src/components/ui/`; o visual vem dos tokens do [[Tema]].

## Onde está no código

`src/components/ui/<nome>.tsx`, um arquivo por primitivo. Para adicionar:
`npx shadcn@4.21.0 add <nome>`.

| Primitivo | Peças | Variantes |
|---|---|---|
| `Button` | — | `default` (primária), `outline`, `secondary`, `ghost`, `destructive`, `link` · tamanhos `xs`, `sm`, `default`, `lg` e `icon`, `icon-xs`, `icon-sm`, `icon-lg` |
| `Badge` | — | `default`, `secondary`, `destructive`, `outline`, `ghost`, `link` (pílula) |
| `Card` | `CardHeader`, `CardTitle`, `CardDescription`, `CardAction`, `CardContent`, `CardFooter` | `size`: `default`, `sm` |
| `Avatar` | `AvatarImage`, `AvatarFallback`, `AvatarGroup`, `AvatarGroupCount`, `AvatarBadge` | — |
| `Separator` | — | `orientation`: `horizontal`, `vertical` |
| `Skeleton` | — | — |

## Comportamento

- Ícone dentro do botão ganha `data-icon="inline-start"` ou `"inline-end"`, que ajusta o respiro.
- `Badge` aceita `render` (Base UI) para trocar o elemento sem perder o estilo.
- Foco visível: anel de 3px na cor `ring` (cerulean) em todo primitivo interativo.

## Regras de uso

- **Primitivo é fundação.** Ajuste de tela vai por `className` ou variante; mudar o arquivo gerado é PR
  próprio, que diz quem muda junto. Mudanças feitas até aqui:
  - `Card` ganhou `shadow-sm` (a sombra Small do kit), como pede o briefing — vale para todo cartão.
- O `outline` pinta com `background` (o fundo da página, `grey-50`): dentro de cartão ou modal, ele
  aparece levemente cinza. É o comportamento gerado, não defeito.
- Botão só de ícone (`size="icon…"`) leva `aria-label`.
- Status de assinatura não usa as variantes do `Badge` direto: vai por mapa de classes literais com a
  cor do kit (regra do `CLAUDE.md` do repositório). O formato é <A DEFINIR> no PR da tabela.

## Histórico de mudanças

- [[2026-09-29-pr-006-primitivos-base]] — `Button`, `Badge`, `Card`, `Avatar`, `Separator` e
  `Skeleton`; `Card` com `shadow-sm`.
