---
tipo: funcionalidade
camada: frontend
area: Shell
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, casca, alertas]
---

# Notificações

## O que é

O sino do header: o que pede atenção na empresa atual, de qualquer página, e o caminho até a central de
alertas ([[AlertsCenter]]).

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/components/layout/notifications-menu.tsx` | `NotificationsMenu`: o sino, o contador e o popover |
| `src/components/alerts/use-alerts.ts` | `useAlerts()`: alertas abertos, dispensados e em revisão, os mesmos do painel e da central |
| `src/components/alerts/alert-actions.ts` | `dismissWithUndo()`: dispensar com "Desfazer" |

## Comportamento

- **Sino** entre a busca e o tema, também no celular.
  - O contador vermelho soma os alertas não dispensados e as assinaturas em revisão, e mostra "9+"
    acima de 9.
  - O nome acessível diz "Notificações: N itens pendentes".
- **Popover "Notificações"**, alinhado à direita:
  - no topo, "N pendentes";
  - cada alerta com "Dispensar";
  - a linha "N assinaturas em revisão — Confirme ou descarte na central de alertas.", que leva a
    `/alertas`;
  - embaixo, "Ver todos os alertas". Seguir um link fecha o popover.
- **Dispensar pelo sino** é o mesmo da central: a dispensa é da empresa, o aviso traz "Desfazer", e o
  popover continua aberto.
- **O número só cai quando a coisa é tratada**, porque abrir o sino não marca nada como visto.

## Estados (vazio, carregando, erro)

- **Vazio:** sino sem número, e "Nada pedindo atenção agora." com o ícone de confirmação.
- **Carregando:** o sino aparece com a casca, depois da [[GuardaDeSessao]].

## Regras de uso

- Não existe "lida/não lida". Quando existir notificação de verdade (e-mail, WhatsApp), o estado de
  leitura é do backend.
- A lista rola dentro do popover (até 60% da altura), e a largura nunca passa da tela menos 2rem.

## Histórico de mudanças

- [[2026-09-29-pr-034-notificacoes]] — criado.
