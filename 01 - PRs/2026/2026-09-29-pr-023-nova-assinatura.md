---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 23
url: https://github.com/VictorNascimento14/ZeroSpend/pull/23
branch: feat/nova-assinatura
tags: [pr, frontend, assinaturas]
status: merged
---

# PR #23 — feat(assinaturas): cadastro manual de assinatura

## 🎯 Contexto

Ordem 21 de [[2026-09-29-plano-da-v1-do-frontend]]. A especificação tem três origens para uma
assinatura — varredura de e-mail, extrato CSV e **cadastro manual** —, e o manual é o primeiro que a v1
consegue fazer de verdade. É também a primeira escrita pela tela no [[RepositorioLocal]].

## 🔧 Mudanças

- `src/components/subscriptions/`:
  - `new-subscription-button.tsx`: o botão "Nova assinatura" com o diálogo de cadastro;
  - `subscription-fields.tsx`: os campos (software, categoria, valor, moeda, ciclo, próxima cobrança),
    que a edição vai reaproveitar;
  - `subscription-form.ts`: `readSubscriptionForm`, que converte o `FormData` num rascunho.
- `PageHeader` ganha `actions`; o botão aparece no dashboard e em `/assinaturas`.
- Tabela vazia: "Use "Nova assinatura" para cadastrar a primeira."
- 2 testes novos (107 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

A partir daqui, a pessoa grava dado da empresa pela tela — valor e fornecedor de uma assinatura. Fica
no `localStorage` do navegador (ADR-001).

## 🧠 Decisões técnicas

- **Campos nativos:** `type="number"` (o navegador entrega o valor com ponto, qualquer que seja o
  idioma) e `type="date"` (entrega `YYYY-MM-DD`, o formato do domínio). Sem analisador de número ou de
  data caseiro — a escada ponytail para no degrau 4 (recurso nativo da plataforma).
- **Quem valida é o repositório:** o formulário só converte. O `noValidate` deixa a mensagem em
  português, da `validateSubscriptionDraft`, cair no campo certo (`aria-invalid` e
  `aria-describedby`).
- **Nasce ativa e manual:** quem cadastra está confirmando a assinatura, e o status não aparece no
  cadastro. A edição (ordem 22) pode mudar.
- **Moeda começa na moeda padrão da empresa**, e o ciclo em mensal; categoria e data começam vazias —
  escolha consciente, não palpite.
- **A dica do valor diz o que é:** "De uma cobrança: no plano anual, o valor do ano" — a regra 3 do
  `CLAUDE.md` (`amount` é uma cobrança), na língua de quem usa.
- **O Select do Base UI entra no formulário pelo `name`** (campo escondido) e mostra o rótulo pelo
  `items`.

## ⚠️ Armadilhas e aprendizados

- Nenhuma nova.

## 🧪 Como testar

1. `npm test` — 107 testes (`readSubscriptionForm` gera rascunho válido; vazio aponta os campos).
2. Num Chromium headless, com o console limpo:
   - "Nova assinatura" vazio → erros em software, categoria, valor e próxima cobrança;
   - cadastrar Miro (Design, US$ 48,50, mensal, 20/10/2026) → "Assinatura de Miro cadastrada.";
   - a linha aparece com "Ferramenta redundante";
   - o gasto vai de R$ 7.444,75 a R$ 7.706,65 e a economia de R$ 634,52 a R$ 896,42 — o grupo Design
     passa a manter o Figma e cortar Miro e Canva;
   - o registro gravado tem só os campos do modelo, com `source: "manual"`;
   - no celular, o diálogo cabe inteiro.

## 🚫 O que não foi verificado

- Leitor de tela real nos Selects do Base UI.

## 📎 Documentação afetada

- [[SubscriptionForm]] (novo) · [[SubscriptionsTable]] · [[Casca]] · [[Dashboard]] · [[Assinaturas]]
- [[2026]] (changelog)
