---
tipo: funcionalidade
camada: frontend
area: Alertas
rota: /alertas
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, alertas]
---

# AlertsCenter

## O que é

A central de alertas da empresa: o que pede atenção, com o jeito de tratar cada coisa. Fica na página
[[Alertas]].

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/components/alerts/alerts-center.tsx` | `AlertsCenter`: as seções, dispensar e voltar a mostrar |
| `src/components/alerts/alert-item.tsx` | `AlertItem` (ícone, título, descrição e ação) e `describeAlert` (o texto de um alerta), usados também pelo [[AlertsPanel]] |
| `src/components/alerts/use-alerts.ts` | `useAlerts()`: alertas abertos, dispensados e em revisão, o mesmo no painel, na central e no sino |
| `src/components/alerts/alert-actions.ts` | `dismissWithUndo()`: dispensar com "Desfazer" |
| `src/lib/domain/alerts.ts` | `currentAlerts` e as chaves ([[AlertasDeRenovacao]]) |

## Comportamento

- **Renovações nos próximos 7 dias**, da mais próxima: "GitHub renova em 2 dias — US$ 84,00 em
  01/10/2026.", com "Dispensar".
- **Ferramentas redundantes**, da maior economia: "Duplicidade em CRM — HubSpot e Pipedrive estão na
  mesma categoria…", com "Dispensar".
- **Dispensar:**
  - grava a dispensa na empresa ([[RepositorioLocal]]), e o alerta sai daqui e do painel do dashboard;
  - o aviso "Alerta dispensado." traz "Desfazer";
  - o alerta volta sozinho quando a situação muda: a cobrança seguinte, ou outra ferramenta no grupo.
- **Em revisão:** as assinaturas `review_needed`, com valor e origem, e os botões "Confirmar" (vira
  ativa) e "Descartar" (sai da lista, com "Desfazer"). É o mesmo comportamento da tabela.
- **"N alertas dispensados"** (fechado): os alertas atuais que foram dispensados, com "Voltar a
  mostrar".
- Os botões repetidos têm nome acessível completo ("Dispensar: GitHub renova em 2 dias", "Confirmar
  ChatGPT Team").

## Estados (vazio, carregando, erro)

- **Vazio**, por seção, com o ícone de confirmação em verde: "Nenhuma renovação nos próximos 7 dias.",
  "Nenhuma ferramenta redundante." e "Nada em revisão.".
- **Carregando:** o esqueleto da [[GuardaDeSessao]].

## Regras de uso

- Dispensa é da empresa, não da pessoa.
- "Em revisão" não se dispensa: se confirma ou se descarta, porque a cobrança conta até lá.
- Tipo novo de alerta entra em `currentAlerts`, com chave de situação, e ganha texto em
  `describeAlert`.

## Histórico de mudanças

- [[2026-09-29-pr-033-central-de-alertas]] — criado.
- [[2026-09-29-pr-034-notificacoes]] — `useAlerts` e `dismissWithUndo` compartilhados com o sino ([[Notificacoes]]).
