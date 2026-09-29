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
| `Input` | — | `type` nativo; `aria-invalid` pinta o erro |
| `Label` | — | — |
| `Select` | `SelectTrigger`, `SelectValue`, `SelectContent`, `SelectItem`, `SelectGroup`, `SelectLabel`, `SelectSeparator`, `SelectScrollUpButton`, `SelectScrollDownButton` | `SelectTrigger size`: `default`, `sm` |
| `Textarea` | — | `aria-invalid` pinta o erro |
| `Switch` | — | `size`: `default`, `sm` |
| `Checkbox` | — | — |
| `Dialog` | `DialogTrigger`, `DialogContent`, `DialogHeader`, `DialogTitle`, `DialogDescription`, `DialogFooter`, `DialogClose` | `DialogContent showCloseButton` (X no canto) · `DialogFooter showCloseButton` (botão "Fechar") |
| `AlertDialog` | `AlertDialogTrigger`, `AlertDialogContent`, `AlertDialogHeader`, `AlertDialogTitle`, `AlertDialogDescription`, `AlertDialogFooter`, `AlertDialogAction`, `AlertDialogCancel`, `AlertDialogMedia` | `AlertDialogAction variant` (as do `Button`) |
| `DropdownMenu` | `DropdownMenuTrigger`, `DropdownMenuContent`, `DropdownMenuGroup`, `DropdownMenuLabel`, `DropdownMenuItem`, `DropdownMenuSeparator`, `DropdownMenuCheckboxItem`, `DropdownMenuRadioGroup`, `DropdownMenuRadioItem`, `DropdownMenuShortcut`, `DropdownMenuSub…` | `DropdownMenuItem variant`: `default`, `destructive` |
| `Sheet` | `SheetTrigger`, `SheetContent`, `SheetHeader`, `SheetTitle`, `SheetDescription`, `SheetFooter`, `SheetClose` | `SheetContent side`: `top`, `right`, `bottom`, `left` |
| `Tooltip` | `TooltipTrigger`, `TooltipContent`, `TooltipProvider` (no layout) | `TooltipContent side` |
| `Table` | `TableHeader`, `TableBody`, `TableFooter`, `TableRow`, `TableHead`, `TableCell`, `TableCaption` | — (rola na horizontal no celular) |
| `Toaster` (Sonner) | montado no layout; o aviso sai por `toast()` / `toast.success()` de `sonner` | — |

## Comportamento

- Ícone dentro do botão ganha `data-icon="inline-start"` ou `"inline-end"`, que ajusta o respiro.
- `Badge` aceita `render` (Base UI) para trocar o elemento sem perder o estilo.
- Foco visível: anel de 3px na cor `ring` (cerulean) em todo primitivo interativo.
- Erro de campo: `aria-invalid` no campo pinta borda e anel em `destructive`; a mensagem vai embaixo,
  em `text-destructive`.
- Gatilho de sobreposição recebe o componente por `render`, não por `asChild`:
  `<DialogTrigger render={<Button variant="outline" />}>Abrir</DialogTrigger>`.
- `Select` recebe a lista em `items` (`{ label, value }`): sem isso, o `SelectValue` mostra o valor
  cru (`monthly`) em vez do rótulo ("Mensal").

## Regras de uso

- **Primitivo é fundação.** Ajuste de tela vai por `className` ou variante; mudar o arquivo gerado é PR
  próprio, que diz quem muda junto. Mudanças feitas até aqui:
  - `Card` ganhou `shadow-sm` (a sombra Small do kit), como pede o briefing — vale para todo cartão.
  - `Dialog` e `Sheet`: o texto "Close" (leitor de tela e botão de rodapé) virou "Fechar".
  - `Toaster`: `fontFamily: "inherit"`, porque o CSS injetado pelo Sonner, fora de camada, trocava a
    fonte do toast pela do sistema.
- O `outline` pinta com `background` (o fundo da página, `grey-50`): dentro de cartão ou modal, ele
  aparece levemente cinza. É o comportamento gerado, não defeito.
- Botão só de ícone (`size="icon…"`) leva `aria-label`.
- Todo campo tem `Label` ligado pelo `htmlFor`. O chevron do select fica como o `base-nova` desenha
  (só o ícone), não no segmento `grey-50` do kit.
- **`DropdownMenuLabel` só dentro de `DropdownMenuGroup`.** Fora dele, a página inteira quebra com
  `Base UI: MenuGroupContext is missing. Menu group parts must be used within <Menu.Group>`.
- O título do toast usa o tamanho do Sonner (13px), fora da escala do kit.
- Na tabela, número e data levam `tabular-nums`, e valor alinha à direita (`text-right`).
- Status de assinatura não usa as variantes do `Badge` direto: vai por mapa de classes literais com a
  cor do kit (regra do `CLAUDE.md` do repositório). O formato é <A DEFINIR> no PR da tabela.

## Histórico de mudanças

- [[2026-09-29-pr-006-primitivos-base]] — `Button`, `Badge`, `Card`, `Avatar`, `Separator` e
  `Skeleton`; `Card` com `shadow-sm`.
- [[2026-09-29-pr-007-primitivos-formulario]] — `Input`, `Label`, `Select`, `Textarea`, `Switch` e
  `Checkbox`.
- [[2026-09-29-pr-008-primitivos-sobreposicao]] — `Dialog`, `AlertDialog`, `DropdownMenu`, `Sheet`,
  `Tooltip` e toasts; textos de fechar em português.
- [[2026-09-29-pr-009-tabela]] — `Table`.
