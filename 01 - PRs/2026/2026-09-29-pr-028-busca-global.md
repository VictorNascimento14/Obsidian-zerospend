---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 28
url: https://github.com/VictorNascimento14/ZeroSpend/pull/28
branch: feat/busca-global
tags: [pr, frontend, casca, busca]
status: aberto
---

# PR #28 — feat(casca): busca global com ⌘K

## 🎯 Contexto

Ordem 26 de [[2026-09-29-plano-da-v1-do-frontend]]. O briefing pede "busca global" no header. Ela
acha páginas, assinaturas e — para quem cuida de várias — empresas, sem tirar a mão do teclado.

## 🔧 Mudanças

- `src/components/layout/global-search.tsx` (novo): `GlobalSearch`, um botão em pílula no header
  ("Buscar… ⌘K" ou "Ctrl K"; no celular, só o ícone) e a paleta:
  - **Páginas:** as cinco áreas;
  - **Assinaturas de <empresa>:** escolher leva a `/assinaturas?busca=<nome>`;
  - **Empresas:** "Trocar para …".
- `/assinaturas` lê o `?busca=` e abre a lista já filtrada; a chave recria a lista quando o parâmetro
  muda sem sair da página.
- `npx shadcn@4.21.0 add command`: `command.tsx` e `input-group.tsx`, e a dependência `cmdk`. Os textos
  padrão do `CommandDialog` passam para português.
- `organization-switcher.tsx`: o gatilho ganha `shrink`.
- `rows.ts`: `normalize` exportado (a busca global usa o mesmo critério).

## 🕵️ Dado sensível (LGPD e sigilo)

A paleta lista nomes de fornecedor e valores da empresa — os mesmos da tabela, na mesma tela.

## 🧠 Decisões técnicas

- **O `command` depende do `dialog`, e o CLI perguntou se sobrescrevia** o nosso, traduzido. A resposta
  foi não (`printf 'n' | npx shadcn add command`): o "Fechar" continua lá.
- **Textos do primitivo em português:** o `CommandDialog` vinha com "Command Palette" e "Search for a
  command to run..." no cabeçalho só para leitor de tela. O uso passa os textos certos, e o padrão
  virou "Busca" e "Digite para buscar." — um uso futuro que esqueça não fala inglês.
- **Filtro sem acento na paleta** (`filter` do `cmdk` com o `normalize` da lista): "clinica" acha
  "Clínica Exemplo".
- **Assinatura escolhida abre a lista filtrada, não um diálogo:** ali estão as ações (editar, excluir,
  revisar), com a assinatura à vista.
- **Atalho pelo sistema:** ⌘K no Mac (lido do `navigator` — o header só existe no cliente, dentro da
  [[GuardaDeSessao]], então não diverge da hidratação); Ctrl K nos outros. A tecla abre e fecha.
- **O valor na paleta não usa `CommandShortcut`,** que é feito para atalho (letras espaçadas).

## ⚠️ Armadilhas e aprendizados

- **O `Button` do shadcn tem `shrink-0` na base.** Com o ícone de busca no header do celular, o
  seletor de empresa não encolhia, e a página vazava para o lado. `shrink` no gatilho resolve:
  medido em 360, 390, 768 e 1280px, sem vazar, com o nome cortado em reticências até 768px.
- `npx shadcn add` com `--yes` ainda pergunta antes de sobrescrever; a resposta vai pela entrada padrão.

## 🧪 Como testar

Num Chromium headless, com o console limpo:

1. O header mostra "Buscar… Ctrl K".
2. Ctrl+K, "figma" → "Figma — R$ 405,00/mês"; Enter → `/assinaturas?busca=Figma`, com "1 de 15" e a
   busca preenchida.
3. "alertas" → a página Alertas.
4. "clinica" → "Trocar para Clínica Exemplo"; Enter troca a empresa.
5. "zzz" → "Nada encontrado."
6. No celular, o ícone abre a paleta, e a página não vaza.

`npm run lint && npm run type-check && npm test && npm run build` passam.

## 🚫 O que não foi verificado

- Leitor de tela real na paleta (o `cmdk` cuida dos papéis de lista e opção).

## 📎 Documentação afetada

- [[Casca]] · [[Assinaturas]] · [[Primitivos]]
- [[2026]] (changelog)
