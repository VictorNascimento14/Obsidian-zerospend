---
tipo: indice
ultima_atualizacao: 2026-09-29
tags: [indice, design-system]
camada: frontend
---

# Linguagem visual — tokens do design system

Fonte: arquivo do Figma **SaaS Design System & UI Kit (Community)**
(`figma.com/design/9IibZkPOOthj8jno1M6tVs`). Transcrito em 2026-09-29 a partir das páginas *Color*,
*Typography*, *Spacing*, *Elevation*, *Corners*, *Icons*, *Buttons*, *Badges*, *Tooltips*, *Input*,
*Date Picker*, *Cards* e *Modals* do arquivo. A decisão de como isso entra no código está em
[[ADR-002-design-system-do-figma-com-shadcn]].

> **Mood pedido no briefing:** clean, moderno, B2B profissional — fundo suave, cartões brancos com
> borda fina, texto escuro de alto contraste, cor primária azul nos CTAs, cantos arredondados, sombra
> sutil, modo escuro.

## Cores

O kit nomeia cada família e dá **quatro tints** (mais claros) e **três shades** (mais escuros) em torno
da cor base. No código o nome é o mesmo do kit: `cerulean-tint-50`, `cerulean`, `cerulean-shade-100`.

### Marca

| Token | Tint 50 | Tint 100 | Tint 200 | Tint 300 | **Base** | Shade 100 | Shade 200 | Shade 300 |
|---|---|---|---|---|---|---|---|---|
| `cerulean` (Primary) | `#E5F0FD` | `#BFD9FA` | `#80B4F4` | `#408EEF` | **`#0068E9`** | `#0053BA` | `#003E8C` | `#002A5D` |
| `raspberry` (Secondary) | `#FCE3EE` | `#F9BFD8` | `#F380B1` | `#EC408A` | **`#E60063`** | `#BB0051` | `#90003E` | `#65002C` |
| `plum` (Tertiary) | `#EFE9F0` | `#D6C8DA` | `#BEA8C4` | `#A889AF` | **`#7C4E87`** | `#633E6C` | `#4A2F51` | `#321F36` |

### Sistema

| Token | Tint 50 | Tint 100 | Tint 200 | Tint 300 | **Base** | Shade 100 | Shade 200 | Shade 300 |
|---|---|---|---|---|---|---|---|---|
| `success` | `#DCF9ED` | `#A8F1D2` | `#6EDFAF` | `#33CC8C` | **`#00BF6F`** | `#009959` | `#007343` | `#004C2C` |
| `warning` | `#FFF0DF` | `#FFE1BF` | `#FFC480` | `#FFA640` | **`#FF8800`** | `#DA7400` | `#B66100` | `#914D00` |
| `danger` | `#FFE7EA` | `#FFB7BF` | `#FF8894` | `#FF4C5E` | **`#FF1028`** | `#D50D21` | `#AA0B1B` | `#800814` |

### Escala de cinza

| `grey-50` | `grey-100` | `grey-200` | `grey-300` | `grey-400` | `grey-500` | `grey-600` | `grey-700` | `grey-800` | `grey-900` |
|---|---|---|---|---|---|---|---|---|---|
| `#F3F3F4` | `#E2E2E5` | `#C5C5CB` | `#A9A8B1` | `#8C8A96` | `#6F6D7C` | `#525062` | `#3E3C4A` | `#292831` | `#151419` |

### Contraste — o que pode ser texto

Medido contra branco (WCAG AA pede 4,5:1 para texto comum):

| Cor | Contraste | Uso permitido |
|---|---|---|
| `cerulean` `#0068E9` | 5,05 | texto, link, fundo de botão com texto branco |
| `grey-500` `#6F6D7C` | 5,05 | **mínimo** para texto secundário informativo |
| `grey-400` `#8C8A96` | 3,39 | só placeholder e desabilitado — nunca informação |
| `success` `#00BF6F` | 2,42 | só preenchimento/ícone; texto usa `success-shade-200` (5,94) |
| `warning` `#FF8800` | 2,39 | só preenchimento/ícone; texto usa `warning-shade-300` (6,43) — `shade-200` dá 4,47 e reprova |
| `danger` `#FF1028` | 3,92 | só preenchimento/ícone; texto e botão destrutivo usam `danger-shade-100` (5,37) |

Pares de badge *outline* (texto sobre o tint 50 da mesma família): `cerulean-shade-100` 6,18 ·
`success-shade-200` 5,33 · `warning-shade-300` 5,75 · `danger-shade-100` 4,57 · `plum-shade-100` 7,22 ·
`raspberry-shade-100` 5,37. `grey-500` sobre o fundo `grey-50` dá 4,56 — passa, sem folga.

No escuro (sobre `grey-800` `#292831`): `grey-300` 6,19 · `cerulean-tint-200` 6,76 (o `tint-300` dá 4,39
e reprova) · `success-tint-200` 8,90 · `warning-tint-200` 9,33 · `danger-tint-200` 6,38.

## Tipografia

Fonte **Inter**. Altura de linha do corpo: **145%**.

| Estilo | Tamanho | Peso | Tracking |
|---|---|---|---|
| Header 1 | 48px (3rem) | 800 | −1,5% |
| Header 2 | 40px (2,5rem) | 700 | −1,25% |
| Header 3 | 32px (2rem) | 700 | −0,25% |
| Header 4 | 24px (1,5rem) | 700 | −0,25% |
| Body 1 | 16px (1rem) | 400 | — |
| Body 2 | 14px (0,875rem) | 400 | — |
| Body 3 | 12px (0,75rem) | 400 | — |
| Text Link 1/2/3 | 16 / 14 / 12px | 500 (Medium), cor primária | — |

No código: `text-h1`…`text-h4` aplicam tamanho, peso, tracking e altura de linha (1,2 a 1,3 — a
altura dos títulos não está no kit) numa classe só; o corpo é `text-base`, `text-sm` e `text-xs`, a
145%.

## Espaçamento, cantos e elevação

- **Espaçamento:** 4 · 8 · 12 · 16 · 24 · 32 · 40 · 48 · 64 · 80 px — coincide com a escala padrão do
  Tailwind (`1`, `2`, `3`, `4`, `6`, `8`, `10`, `12`, `16`, `20`).
- **Cantos:** 4 · 8 · 12 · 16 px. Botão e campo usam **8px**; cartão **12px** (o `rounded-xl` do
  briefing); badge e busca são **pílula**. No código: `rounded-sm` e `rounded-md` 4px (o kit não tem
  6px), `rounded-lg` 8px, `rounded-xl` 12px, `rounded-2xl` 16px; pílula é `rounded-full` ou
  `rounded-4xl`.
- **Elevação** (sombra preta):

| Nível | Camadas |
|---|---|
| X-Small | `0 0 4px` a 15% |
| Small | `0 1px 6px` a 15% |
| Medium | `1.5px 2px 8px` a 10% + `-1px 0 8px` a 10% |
| Large | `2.25px 3px 10px` a 10% + `-2px -1px 12px` a 10% |

No código: `shadow-xs`, `shadow-sm`, `shadow-md` e `shadow-lg`. As outras sombras do Tailwind
(`shadow-2xs`, `shadow-xl`, `shadow-2xl`) não existem.

## Ícones

Traço de contorno, cantos arredondados, sem preenchimento — o mesmo desenho do **Lucide**, que é a
biblioteca pedida no briefing (`lucide-react`).

## Componentes do kit

- **Botões:** preenchido, claro (tint) e contorno, em cada cor de marca e no neutro escuro; ícone à
  esquerda ou à direita; estado desabilitado em cinza.
- **Badges:** pílula, versões *fill* e *outline* (fundo tint 50 + borda e texto da cor) para as seis
  famílias; contador redondo.
- **Tooltips:** *fill* (fundo da cor, texto branco) e *light* (fundo tint 50, borda e texto da cor).
- **Input:** rótulo acima; borda `grey-300`; foco com borda primária; erro com borda `danger` e mensagem
  pequena abaixo com ícone; select com o chevron num segmento `grey-50`; busca em pílula com a lupa à
  direita.
- **Cards:** notificação (lida em branco, não lida em `cerulean-tint-50` com borda primária, "há 4 h"),
  evento com "Dispensar", **upload de arquivo** (área tracejada "arraste ou escolha", estado de sucesso
  em verde e de erro em vermelho).
- **Modais:** fundo branco, fechar no canto, título em negrito, descrição curta, ação primária à
  direita com seta.
- **Date picker:** grade mensal com fim de semana na cor primária e dia selecionado em destaque escuro.

## Modo escuro

O kit **não** tem modo escuro; o briefing pede. É derivado da escala de cinza do próprio kit — ver
[[ADR-002-design-system-do-figma-com-shadcn]] — e nenhuma cor fora do kit entra para isso. Os valores
estão na tabela de tokens semânticos, abaixo.

## Tokens semânticos — o que a tela usa

Os tokens do shadcn, com o valor do kit em cada modo (`src/app/globals.css`; ver [[Tema]]). A tela
usa estes para superfície, texto e borda; cor crua do kit fica para estado. Criado em
[[2026-09-29-pr-003-tema]].

| Token | Claro | Escuro | Onde aparece |
|---|---|---|---|
| `background` | `grey-50` | `grey-900` | fundo da página; botão `outline` |
| `foreground` | `grey-900` | `grey-50` | texto principal |
| `card` · `popover` | `white` | `grey-800` | cartão · menu, modal, folha e toast |
| `primary` | `cerulean` | `cerulean-tint-200` | botão principal, link, seleção |
| `primary-foreground` | `white` | `grey-900` | texto sobre `primary` |
| `secondary` · `accent` | `grey-100` | `grey-700` | botão secundário · item de menu em foco |
| `muted` | `grey-50` | `grey-700` | fundo discreto: cabeçalho de tabela, esqueleto, hover do `ghost` |
| `muted-foreground` | `grey-500` | `grey-300` | texto secundário |
| `destructive` | `danger-shade-200` | `danger-tint-100` | ação destrutiva (texto, e fundo da mesma cor a 10% ou 20%) |
| `border` | `grey-100` | `grey-700` | borda de cartão, divisória |
| `input` | `grey-300` | `grey-600` | borda de campo |
| `ring` | `cerulean` | `cerulean-tint-200` | anel de foco |

Contraste dos pares de texto (o AA pede 4,5):

| Par | Claro | Escuro |
|---|---|---|
| `foreground` sobre `background` · `card` | 16,53 · 18,32 | 16,53 · 13,13 |
| `muted-foreground` sobre `background` · `card` · `muted` | 4,56 · 5,05 · 4,56 | 7,79 · 6,19 · 4,58 |
| `primary-foreground` sobre `primary` | 5,05 | 8,51 |
| `primary` (link) sobre `background` · `card` | 4,56 · 5,05 | 8,51 · 6,76 |
| `secondary-foreground` sobre `secondary` | 14,17 | 9,71 |
| `destructive` sobre `background` · `card` | 6,83 · 7,57 | 11,15 · 8,86 |
| `destructive` sobre o próprio fundo translúcido, na página · no cartão | 5,72 · 6,32 | 7,04 · 5,47 |

> ⚠️ **Borda de campo abaixo de 3:1.** `input` contra o branco dá 2,35 (no escuro, 1,86 contra o
> cartão); o WCAG 1.4.11 pede 3:1 para o contorno de componente. É o desenho do kit: o campo também se
> identifica pelo rótulo acima e pelo foco em `cerulean` (5,05). Se uma auditoria pedir, a troca é
> `--input` para `grey-400` (3,39), num lugar só.

> **`ghost` sobre o fundo da página não mostra hover:** no claro, `muted` e `background` são o mesmo
> `grey-50`. Em cima do fundo, use `outline`.
