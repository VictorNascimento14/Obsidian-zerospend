---
tipo: funcionalidade
camada: frontend
area: Assinaturas
rota: /dashboard
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, assinaturas, dashboard]
---

# Tabela de assinaturas

## O que é

A `SubscriptionsTable` do briefing: todas as assinaturas da empresa da sessão, com o que cada uma
custa por mês, quando cobra de novo e em que estado está. Aparece no [[Dashboard]].

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/components/subscriptions/subscriptions-table.tsx` | `SubscriptionsTable` (o cartão do dashboard) |
| `src/components/subscriptions/subscription-rows-table.tsx` | `SubscriptionRowsTable` — o corpo da tabela, usado no dashboard e em [[Assinaturas]] |
| `src/components/subscriptions/rows.ts` | `buildRows(assinaturas, empresa, hoje)` — valor por mês, próxima cobrança, dias até ela, redundância, ordem; `filterRows` e `sortRows` (testes em `rows.test.ts`) |
| `src/components/subscriptions/status-badge.tsx` | `StatusBadge` (Ativa, Em revisão, Cancelada) e `RedundantBadge` ("Ferramenta redundante") |
| `src/components/subscriptions/vendor-avatar.tsx` | `VendorAvatar` — monograma na cor do kit |

## Comportamento

| Coluna | O que mostra |
|---|---|
| Software | monograma ("GW" para Google Workspace) + nome; embaixo, "Resp.: …" quando há responsável |
| Categoria | o rótulo da categoria |
| Valor/mês | na moeda da empresa ([[RegrasDeCobranca]]); embaixo, o valor original quando é anual ou em outra moeda |
| Ciclo | Mensal ou Anual |
| Próxima cobrança | a efetiva, `DD/MM/AAAA`; dentro da janela de alerta, "em N dias" em tom de alerta; cancelada: "—" |
| Status | a etiqueta do status gravado e, se for o caso, "Ferramenta redundante" ([[Redundancia]]) |
| (ações) | o menu "⋯" — "Editar" e "Excluir"; em revisão, "Confirmar", "Descartar" e "Editar" ([[SubscriptionForm]]) |

- **Ordem:** da próxima cobrança para a mais distante; canceladas no fim, por nome.
- **Etiquetas:** Ativa em success, Em revisão em warning, Cancelada em cinza, Ferramenta redundante em
  danger — classes literais com o par escuro.
- **Monograma:** a cor do catálogo de fornecedores ([[LeitorDeExtrato]]) quando o fornecedor é
  conhecido; senão, uma cor estável tirada do nome — entre cerulean, raspberry, plum, success e warning.
- **Celular:** a tabela rola na horizontal dentro do cartão; a página não.

## Estados (vazio, carregando, erro)

- **Vazio** (`NoSubscriptionsYet`, o mesmo da lista em [[Assinaturas]]): "Nenhuma assinatura cadastrada
  nesta empresa. Importe do extrato do cartão ou use “Nova assinatura”." O texto vem com o link
  "Importar do extrato" para o [[Onboarding]], e a tabela não aparece. O botão "Nova assinatura" está
  no topo da página ([[SubscriptionForm]]).

## Regras de uso

- Ação nova da linha entra no menu "⋯" (`SubscriptionActions`).
- Coluna nova mexe no ponto de quebra do painel do dashboard ([[AlertsPanel]]): meça a tabela de novo.

## Histórico de mudanças

- [[2026-09-29-pr-021-tabela-de-assinaturas]] — criada, no dashboard.
- [[2026-09-29-pr-023-nova-assinatura]] — o estado vazio aponta para "Nova assinatura".
- [[2026-09-29-pr-024-editar-assinatura]] — coluna de ações (Editar) e o responsável embaixo do nome.
- [[2026-09-29-pr-025-excluir-assinatura]] — "Excluir" no menu da linha.
- [[2026-09-29-pr-026-revisar-deteccao]] — confirmar ou descartar na linha em revisão.
- [[2026-09-29-pr-027-pagina-assinaturas]] — corpo da tabela separado (`SubscriptionRowsTable`); filtrar e ordenar.
- [[2026-09-29-pr-029-leitor-de-extrato]] — monograma com a cor do catálogo.
- [[2026-09-29-pr-030-onboarding-extrato]] — o vazio aponta para o [[Onboarding]] ("Importar do extrato").
- [[2026-09-29-pr-036-configuracoes-alertas]] — o destaque "em N dias" segue a antecedência da empresa.
