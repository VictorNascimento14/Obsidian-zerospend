---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 25
url: https://github.com/VictorNascimento14/ZeroSpend/pull/25
branch: feat/excluir-assinatura
tags: [pr, frontend, assinaturas]
status: aberto
---

# PR #25 — feat(assinaturas): excluir com confirmação e desfazer

## 🎯 Contexto

Ordem 23 de [[2026-09-29-plano-da-v1-do-frontend]]. A tabela da especificação "permite… deletar uma
assinatura". Excluir é destrutivo; a confirmação e o "desfazer" evitam a perda por um clique errado.

## 🔧 Mudanças

- `repository.ts`: `restoreSubscription(assinatura)` — o "desfazer": volta com o mesmo id.
- `subscription-actions.tsx`: "Excluir" no menu da linha (destrutivo, depois de um separador) e o
  `DeleteSubscriptionDialog` (confirmação).
- 1 teste novo (111 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

Excluir remove o registro do `localStorage`. O "desfazer" guarda a assinatura só na memória da aba,
enquanto o aviso está na tela; recarregar a página perde a chance de desfazer.

## 🧠 Decisões técnicas

- **A confirmação diz o efeito real:** "Ela sai da tabela, do gasto mensal e dos alertas. Logo depois,
  dá para desfazer." — nada que o código não faça (regra 7 do `CLAUDE.md`).
- **"Desfazer" devolve o mesmo id:** a assinatura volta idêntica, não como uma cópia nova. O
  repositório valida de novo e recusa se o id já voltou (dois cliques) ou se a empresa sumiu.
- **Excluir apaga de verdade;** "cancelar a assinatura" é outra coisa — é o status "Cancelada", pela
  edição. A exclusão é para o que não devia estar ali (duplicata, cadastro errado).
- **O botão "Desfazer" usa o estilo do Sonner** (invertido: escuro no claro, claro no escuro),
  legível nos dois modos — sem mexer no primitivo.

## ⚠️ Armadilhas e aprendizados

- Nenhuma nova.

## 🧪 Como testar

1. `npm test` — 111 testes (desfazer com o mesmo id, uma vez só; empresa sumida recusada).
2. Num Chromium headless, com o console limpo:
   - "⋯ → Excluir" no Slack mostra a confirmação, e "Cancelar" não muda nada;
   - "Excluir" tira a linha, o gasto cai de R$ 7.444,75 para R$ 6.564,75, e o aviso "Assinatura de
     Slack excluída." aparece com "Desfazer";
   - "Desfazer" traz o Slack de volta com o mesmo id, e o gasto volta a R$ 7.444,75.

## 🚫 O que não foi verificado

- Desfazer depois de recarregar a página: não existe (o aviso some com a aba).

## 📎 Documentação afetada

- [[SubscriptionForm]] · [[SubscriptionsTable]] · [[RepositorioLocal]]
- [[2026]] (changelog)
