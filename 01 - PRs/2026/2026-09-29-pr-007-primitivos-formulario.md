---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 7
url: https://github.com/VictorNascimento14/ZeroSpend/pull/7
branch: ui/primitivos-formulario
tags: [pr, frontend, design-system]
status: aberto
---

# PR #7 — ui(primitivos): adicionar Input, Label, Select, Textarea, Switch e Checkbox

## 🎯 Contexto

Ordem 5 de [[2026-09-29-plano-da-v1-do-frontend]]. Segundo lote de primitivos: os campos que o
cadastro e a edição de assinatura, o entrar e as configurações vão usar.

## 🔧 Mudanças

- `npx shadcn@4.21.0 add input label select textarea switch checkbox`: seis arquivos em
  `src/components/ui/`, sem dependência nova e sem mudança nos arquivos gerados.

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum — ainda não há formulário que grave dado.

## 🧠 Decisões técnicas

- Nenhum primitivo alterado. O kit desenha o chevron do select num segmento `grey-50` à direita; o
  `base-nova` usa só o ícone. A diferença é de acabamento e não justifica editar o arquivo gerado —
  anotado em [[Primitivos]].
- O placeholder sai em `muted-foreground` (`grey-500`, 5,05 contra o branco), acima do `grey-400` que o
  kit permite para placeholder.

## ⚠️ Armadilhas e aprendizados

- O `Select` do Base UI mostra o **valor** cru no `SelectValue` (`monthly`), a menos que a lista de
  opções vá em `items` (`{ label, value }`) no `Select`. Com `items`, aparece "Mensal".

## 🧪 Como testar

Vitrine temporária (fora do commit), fotografada num Chromium headless nos dois modos, com o console
limpo:

1. Campo em foco: borda e anel em `cerulean` (no escuro, `cerulean-tint-200`).
2. Campo com `aria-invalid`: borda e anel em `destructive`, e a mensagem embaixo em `text-destructive`.
3. `Select` aberto: a opção marcada aparece sobre o gatilho, e a lista usa `popover` com a sombra
   Medium do kit.
4. Campo desabilitado, `Textarea`, `Switch` ligado e desligado, `Checkbox` marcado e desmarcado.

Lint, type-check e build passam.

## 🚫 O que não foi verificado

- Navegação por teclado dentro do `Select` aberto (setas, Enter, Esc): é do Base UI, e a primeira tela
  que usar o select confere.
- A borda do campo segue abaixo de 3:1 (ver [[linguagem-visual]]).

## 📎 Documentação afetada

- [[Primitivos]]
- [[2026]] (changelog)
