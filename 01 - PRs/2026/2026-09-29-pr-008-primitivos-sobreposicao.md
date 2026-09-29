---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 8
url: https://github.com/VictorNascimento14/ZeroSpend/pull/8
branch: ui/primitivos-sobreposicao
tags: [pr, frontend, design-system]
status: merged
---

# PR #8 — ui(primitivos): adicionar Dialog, AlertDialog, DropdownMenu, Sheet, Tooltip e toasts

## 🎯 Contexto

Ordem 6 de [[2026-09-29-plano-da-v1-do-frontend]]. Terceiro lote de primitivos: o que abre por cima
da tela — o cadastro em modal, a confirmação de exclusão, o menu de ações da linha, o menu do celular,
a dica e o aviso de "excluída, desfazer".

## 🔧 Mudanças

- `npx shadcn@4.21.0 add dialog alert-dialog dropdown-menu sheet tooltip sonner`: seis arquivos em
  `src/components/ui/` e a dependência `sonner`.
- `src/app/layout.tsx`: `TooltipProvider` em volta das páginas (o CLI pede) e o `Toaster` montado uma
  vez, com a região de avisos rotulada "Notificações".
- Arquivos gerados alterados:
  - `dialog.tsx` e `sheet.tsx`: o texto "Close" (do leitor de tela, e o do botão de rodapé do
    `Dialog`) virou "Fechar";
  - `sonner.tsx`: `fontFamily: "inherit"` no estilo do `Toaster`.

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum.

## 🧠 Decisões técnicas

- **Texto em inglês nos primitivos é defeito, não detalhe.** A interface é `pt-BR` (`<html lang>`), e
  um leitor de tela lê "Close" com fonética portuguesa. A troca é no arquivo gerado, no PR que o
  introduz.
- **Fonte do toast herdada, não fixada.** O Sonner injeta um CSS próprio, fora de camada, com
  `font-family: ui-sans-serif, system-ui…`, que vence qualquer utilitário do Tailwind. O `style`
  inline do `Toaster` ganha a disputa; `inherit` faz o toast usar a fonte da página sem conhecer o nome
  da variável da Inter.
- **Rótulo da região de avisos pela prop do Sonner (`containerAriaLabel`)**, no layout, sem editar o
  arquivo. O `closeButtonAriaLabel` ficou de fora: o botão de fechar do toast está desligado.
- `Toaster` montado já neste PR: sem ele, `toast()` não mostra nada, e o primeiro uso (excluir com
  desfazer) vem na ordem 23.

## ⚠️ Armadilhas e aprendizados

- **`DropdownMenuLabel` fora de `DropdownMenuGroup` derruba a página inteira**, com
  `Base UI: MenuGroupContext is missing. Menu group parts must be used within <Menu.Group>`. O rótulo
  é um `Menu.GroupLabel` do Base UI. Registrado nas regras de [[Primitivos]].
- Gatilho do Base UI recebe o componente por `render` (`<DialogTrigger render={<Button />}>`), não por
  `asChild`.

## 🧪 Como testar

Vitrine temporária (fora do commit), percorrida num Chromium headless nos dois modos, com o console
limpo: abrir o diálogo, o alerta de exclusão, o menu de ações e a folha lateral (e fechar cada um com
Esc), passar o mouse na dica e disparar um toast.

- O diálogo é branco (`popover`) sobre fundo desfocado, com "Fechar" no rodapé e no canto.
- O menu tem o item destrutivo em `destructive`.
- A dica é invertida (`foreground`), e o foco visível volta ao gatilho depois do Esc.
- O toast sai em Inter, com a região lida como "Notificações alt+T".

Lint, type-check e build passam.

## 🚫 O que não foi verificado

- O título do toast usa o tamanho do Sonner (13px), fora da escala do kit (12 · 14 · 16).
- Armadilha de foco dentro do diálogo e leitura por leitor de tela real: são do Base UI; a primeira
  tela com modal confere.

## 📎 Documentação afetada

- [[Primitivos]]
- [[2026]] (changelog)
