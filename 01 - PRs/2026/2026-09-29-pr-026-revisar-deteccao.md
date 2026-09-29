---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 26
url: https://github.com/VictorNascimento14/ZeroSpend/pull/26
branch: feat/revisar-deteccao
tags: [pr, frontend, assinaturas]
status: merged
---

# PR #26 — feat(assinaturas): confirmar ou descartar assinatura em revisão

## 🎯 Contexto

Ordem 24 de [[2026-09-29-plano-da-v1-do-frontend]]. Pelo fluxo 2 da
[[2026-09-29-especificacao-do-mvp]], o que o sistema detecta (extrato, e-mail) entra com o status
`review_needed`. Alguém precisa dizer se aquilo é uma assinatura de verdade — este PR dá os dois
botões.

## 🔧 Mudanças

- `subscription-actions.tsx`: para linha **em revisão**, o menu "⋯" passa a ser "Confirmar",
  "Descartar" e "Editar". Linhas ativas e canceladas seguem com "Editar" e "Excluir".
- "Confirmar" muda o status para ativa; "Descartar" remove, com "Desfazer" no aviso.

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum novo.

## 🧠 Decisões técnicas

- **"Descartar" ocupa o lugar do "Excluir" na linha em revisão:** para o que foi detectado por engano
  (uma compra avulsa, por exemplo), descartar é excluir — ter os dois no mesmo menu confundiria.
- **Descartar não pede confirmação;** o "Desfazer" do aviso cobre o engano. Revisão é triagem, e um
  diálogo a cada item deixaria a triagem lenta. A exclusão de uma assinatura confirmada continua pedindo
  confirmação ([[2026-09-29-pr-025-excluir-assinatura]]).
- **Nada novo no repositório:** confirmar é `updateSubscription` com status ativo; descartar e desfazer
  são `removeSubscription` e `restoreSubscription`.
- **Sem memória do que foi descartado.** Um extrato importado de novo pode detectar o mesmo item — <A
  DEFINIR> no PR da importação (ordem 28), que é quem sabe comparar o que chega com o que existe.

## ⚠️ Armadilhas e aprendizados

- Nenhuma nova.

## 🧪 Como testar

Num Chromium headless, com o console limpo:

1. O KPI começa em "12 · E 2 em revisão".
2. ChatGPT Team (em revisão): o menu tem Confirmar, Descartar e Editar. "Confirmar" dá "Assinatura de
   ChatGPT Team confirmada.", a linha fica "Ativa", e o KPI vai a "13 · E 1 em revisão".
3. Adobe Acrobat Pro: "Descartar" tira a linha ("Nenhuma em revisão"), e "Desfazer" devolve em
   revisão.
4. Slack (ativa): o menu segue com Editar e Excluir.

`npm run lint && npm run type-check && npm test && npm run build` passam.

## 🚫 O que não foi verificado

- Revisar em lote (confirmar várias de uma vez): não pedido.

## 📎 Documentação afetada

- [[SubscriptionForm]] · [[SubscriptionsTable]]
- [[2026]] (changelog)
