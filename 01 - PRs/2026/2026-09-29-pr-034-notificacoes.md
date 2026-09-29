---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 34
url: https://github.com/VictorNascimento14/ZeroSpend/pull/34
branch: feat/notificacoes
tags: [pr, frontend, casca, alertas]
status: aberto
---

# PR #34 — feat(casca): sino de notificações no header

## 🎯 Contexto

Ordem 32 de [[2026-09-29-plano-da-v1-do-frontend]]. O briefing pede, no header, "seletor de empresa,
busca global, notificações, avatar", e só faltava o sino. A central de alertas
([[2026-09-29-pr-033-central-de-alertas]]) já dizia o que pede atenção. O sino leva isso a qualquer
página.

## 🔧 Mudanças

- **Sino no header**, entre a busca e o tema ([[Notificacoes]]):
  - o contador soma os alertas não dispensados e as assinaturas em revisão ("9+" acima de 9);
  - o nome acessível traz o número ("Notificações: 7 itens pendentes").
- **O popover "Notificações"** mostra:
  - cada alerta com "Dispensar", que tem "Desfazer" no aviso;
  - uma linha "N assinaturas em revisão", que leva à central;
  - "Ver todos os alertas". Os links fecham o popover.
- **`Popover`**, primitivo novo do shadcn, gerado sem mudança ([[Primitivos]]).
- **Compartilhado entre painel, central e sino:**
  - `useAlerts()` diz o que pede atenção: alertas abertos, dispensados e em revisão;
  - `dismissWithUndo()` dispensa com "Desfazer".

  Painel e central passaram a usar os dois, sem mudar de comportamento.

## 🕵️ Dado sensível (LGPD e sigilo)

Nada novo é gravado. O sino lê os mesmos alertas e dispensas da central.

## 🧠 Decisões técnicas

- **Sem "lida/não lida".**
  - O número é o que falta tratar, e só cai quando a coisa é tratada: dispensada, confirmada ou
    descartada.
  - Guardar "visto" pediria mais um estado por pessoa, e um contador que zera ao abrir esconderia o que
    continua pendente.
  - O visual de notificação não lida do kit fica para quando houver notificação de verdade (e-mail,
    WhatsApp), que é do backend.
- **Em revisão entra na conta, mas não na lista.** A assinatura em revisão se resolve confirmando ou
  descartando, e isso é trabalho da central. O sino mostra uma linha com o total e o caminho.
- **Cores do contador:** `danger-shade-100` com texto branco no claro (5,37:1) e `danger-tint-100` com
  `grey-900` no escuro (11,15:1). O `destructive` do tema não serve aqui: no escuro ele é claro, e o
  texto branco some.
- **O popover controla o próprio `open`** para fechar ao seguir um link. O header não desmonta entre as
  páginas, então sem isso o popover ficaria aberto sobre a página nova.
- **Largura `min(24rem, 100vw - 2rem)`**, com a lista rolando até 60% da altura. Assim, a 390px o
  popover ocupa de 5 a 363px, sem vazar.

## ⚠️ Armadilhas e aprendizados

- Nenhuma nova. O `shadcn add popover` não tocou nos primitivos já alterados.

## 🧪 Como testar

`npm test` (154 testes), lint, type-check e build passam.

No navegador (Chromium headless, 1440px claro e escuro e 390px, com o console limpo):

1. Na conta de demonstração, o sino mostra 7 (3 renovações, 2 duplicidades e 2 em revisão).
2. Dispensar "GitHub renova em 2 dias" pelo sino: o popover continua aberto, o sino passa a 6, e o
   aviso traz "Desfazer".
3. Esc fecha, e o foco volta ao sino.
4. "2 assinaturas em revisão" e "Ver todos os alertas" levam a `/alertas` e fecham o popover.
5. Conta nova: sino sem número, e o popover diz "Nada pedindo atenção agora.".
6. A 390px, o popover fica de 5 a 363px, e a página não vaza.

## 🚫 O que não foi verificado

- Leitor de tela real no popover. O título é o `Popover.Title` do Base UI, que dá nome ao diálogo.

## 📎 Documentação afetada

- [[Notificacoes]] (nova) · [[Casca]] · [[Primitivos]] · [[AlertsCenter]] · [[AlertsPanel]]
- [[2026]] (changelog)
