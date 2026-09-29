---
tipo: funcionalidade
camada: frontend
area: Assinaturas
rota: /dashboard, /assinaturas
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, assinaturas]
---

# Formulário de assinatura

## O que é

O cadastro manual de uma assinatura ("Nova assinatura"), a edição ("⋯ → Editar" na tabela) e os campos
que os dois compartilham.

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/components/subscriptions/new-subscription-button.tsx` | `NewSubscriptionButton`: botão + diálogo de cadastro |
| `src/components/subscriptions/subscription-actions.tsx` | `SubscriptionActions`: o menu "⋯" da linha, o diálogo de edição e a confirmação de exclusão |
| `src/components/subscriptions/subscription-fields.tsx` | `SubscriptionFields`: os campos, com erro por campo |
| `src/components/subscriptions/subscription-form.ts` | `readSubscriptionForm(form, { status, source })` (testes em `subscription-form.test.ts`) |

## Comportamento

- **Onde aparece:** no topo do [[Dashboard]] e de [[Assinaturas]].
- **Campos:**
  - software;
  - categoria (lista fechada);
  - valor, com a dica "De uma cobrança: no plano anual, o valor do ano.";
  - moeda (real ou dólar; começa na moeda da empresa);
  - ciclo (começa em mensal);
  - próxima cobrança;
  - responsável (opcional, até 80 caracteres);
  - status — só na edição.
- **Cadastrar:** valida no [[RepositorioLocal]]. Com erro, cada mensagem vai para o seu campo. Com
  sucesso, fecha, avisa "Assinatura de Miro cadastrada." e a tabela e os KPIs se atualizam sozinhos.
- **O que nasce:** status `active` e origem `manual`.
- **Editar:** o mesmo formulário, preenchido, mais o status; salva com "Assinatura de Slack
  atualizada." A origem nunca muda. O diálogo trabalha sobre uma cópia da assinatura tirada ao abrir.
- **Excluir:** "⋯ → Excluir" pede confirmação ("Ela sai da tabela, do gasto mensal e dos alertas.
  Logo depois, dá para desfazer."). Depois, o aviso "Assinatura de Slack excluída." traz
  "Desfazer", que devolve a assinatura com o mesmo id.
- **Revisar (linha em revisão):** o menu vira "Confirmar" (status ativa: "Assinatura de ChatGPT Team
  confirmada."), "Descartar" (sai da lista, com "Desfazer"; sem diálogo — é triagem) e "Editar". O
  "Excluir" não aparece ali: descartar já é excluir.

## Estados (vazio, carregando, erro)

- **Erro de campo:** a mensagem embaixo do campo; o campo com borda de erro.
- **Falha do armazenamento** (cota cheia): aviso de erro, sem fechar o diálogo.

## Regras de uso

- Os mesmos `SubscriptionFields` servem para a edição — campo novo entra nos dois de uma vez.

## Histórico de mudanças

- [[2026-09-29-pr-023-nova-assinatura]] — criado: cadastro manual.
- [[2026-09-29-pr-024-editar-assinatura]] — edição, status e responsável.
- [[2026-09-29-pr-025-excluir-assinatura]] — excluir com confirmação e desfazer.
- [[2026-09-29-pr-026-revisar-deteccao]] — confirmar ou descartar o que está em revisão.
