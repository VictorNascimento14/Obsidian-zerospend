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

O cadastro manual de uma assinatura ("Nova assinatura") e os campos que o cadastro e a edição
compartilham.

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/components/subscriptions/new-subscription-button.tsx` | `NewSubscriptionButton`: botão + diálogo de cadastro |
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
  - próxima cobrança.
- **Cadastrar:** valida no [[RepositorioLocal]]. Com erro, cada mensagem vai para o seu campo. Com
  sucesso, fecha, avisa "Assinatura de Miro cadastrada." e a tabela e os KPIs se atualizam sozinhos.
- **O que nasce:** status `active` e origem `manual`.

## Estados (vazio, carregando, erro)

- **Erro de campo:** a mensagem embaixo do campo; o campo com borda de erro.
- **Falha do armazenamento** (cota cheia): aviso de erro, sem fechar o diálogo.

## Regras de uso

- Os mesmos `SubscriptionFields` servem para a edição — campo novo entra nos dois de uma vez.

## Histórico de mudanças

- [[2026-09-29-pr-023-nova-assinatura]] — criado: cadastro manual.
