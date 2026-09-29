---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 24
url: https://github.com/VictorNascimento14/ZeroSpend/pull/24
branch: feat/editar-assinatura
tags: [pr, frontend, assinaturas]
status: merged
---

# PR #24 — feat(assinaturas): editar valor, ciclo, data, status e responsável

## 🎯 Contexto

Ordem 22 de [[2026-09-29-plano-da-v1-do-frontend]]. A tabela da especificação "permite editar
valores, alterar a data de renovação, marcar responsável". É o primeiro uso da coluna "Ações" que o
[[2026-09-29-pr-021-tabela-de-assinaturas]] deixou para quando houvesse o que fazer.

## 🔧 Mudanças

- **Modelo:** `Subscription.owner` (opcional), o responsável; `OWNER_MAX` (80) e a regra na
  `validateSubscriptionDraft`. O repositório grava o responsável aparado e deixa de fora o vazio.
- `subscription-actions.tsx` (novo): o menu "⋯" de cada linha, com "Editar", e o diálogo de edição.
- `SubscriptionFields`: campo "Responsável" (cadastro e edição) e, na edição, "Status".
- `readSubscriptionForm`: lê status (quando o formulário tem o campo) e responsável; a origem nunca muda.
- Tabela: coluna de ações; o responsável aparece embaixo do nome ("Resp.: Pessoa Exemplo").
- Sementes: Google Workspace e HubSpot com Admin Exemplo; Figma e Canva com Pessoa Exemplo.
- Dashboard: o painel lateral passa de 1600px para 1700px.
- 3 testes novos (110 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

**Responsável é dado pessoal** (nome de quem trabalha na empresa). Fica no `localStorage`, com o resto
dos dados da empresa (ADR-001). As sementes usam só os nomes fictícios estáveis do cofre.

## 🧠 Decisões técnicas

- **Responsável em texto livre, não em lista de membros.** Quem responde por uma ferramenta nem
  sempre tem conta no ZeroSpend — e os membros só existem na ordem 35. Até 80 caracteres: um nome, não
  um texto.
- **Mostrado embaixo do nome do software,** não numa coluna: a tabela já era larga.
- **O status aparece só na edição:** o cadastro manual nasce ativo ([[SubscriptionForm]]). A origem
  (`source`) nunca é editável — ela diz de onde a assinatura veio.
- **O diálogo edita uma cópia tirada ao abrir.** Salvar muda a assinatura enquanto o diálogo ainda faz
  a animação de saída; com a assinatura "viva", os campos recebiam valores iniciais novos depois de
  montados, e o Base UI avisava no console. Com a cópia, o aviso sumiu, e a próxima abertura renova a
  cópia.
- **Painel lateral a partir de 1700px:** medido de novo — a tabela passou de ~860 a ~900px com a coluna
  de ações e o responsável, e a 1600px era cortada.

## ⚠️ Armadilhas e aprendizados

- Mudança de coluna na tabela mexe no ponto de quebra do painel do dashboard. A medida está nos dois
  lugares (comentário do código e [[AlertsPanel]]).

## 🧪 Como testar

1. `npm test` — 110 testes: responsável vazio e longo, aparado e omitido na gravação, status e
   origem na leitura do formulário.
2. Num Chromium headless, com o console limpo:
   - "⋯ → Editar" no Slack abre preenchido ("Editar Slack", 880, 11/10/2026);
   - valor 0 → "O valor precisa ser maior que zero.";
   - R$ 990 anual, "Em revisão", responsável "Pessoa Exemplo" → "Assinatura de Slack atualizada.";
   - a linha fica com "Resp.: Pessoa Exemplo · R$ 82,50 · R$ 990,00 por ano · Anual · Em revisão";
   - apagar o responsável tira a linha "Resp.";
   - a 1600px o painel fica acima, e a 1700 e 1920px ao lado — a tabela inteira nos três.

## 🚫 O que não foi verificado

- Leitor de tela real no menu de ações.

## 📎 Documentação afetada

- [[SubscriptionForm]] · [[SubscriptionsTable]] · [[ModeloDeDominio]] · [[DadosDeDemonstracao]]
- [[AlertsPanel]] · [[glossario]]
- [[2026]] (changelog)
