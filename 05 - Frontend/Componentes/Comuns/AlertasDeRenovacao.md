---
tipo: funcionalidade
camada: frontend
area: Comuns
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, dominio, alertas]
---

# Alertas de renovação

## O que é

A regra que decide quais assinaturas merecem aviso porque vão cobrar em breve. Alimenta o KPI de
renovações urgentes, o painel de alertas do dashboard e a central de alertas.

## Onde está no código

`src/lib/domain/alerts.ts` — `renewalAlerts`, `RenewalAlert`, `DEFAULT_RENEWAL_LEAD_DAYS`, e
`currentAlerts` com o tipo `Alert` (todos os alertas da empresa, com chave). A frase
da distância é `formatDaysUntil` (`format.ts`). Testes em `alerts.test.ts`.

## Comportamento

- Entra no alerta toda assinatura **não cancelada** cuja **próxima cobrança efetiva**
  ([[RegrasDeCobranca]]) cai **de hoje até hoje + antecedência**, com as duas pontas incluídas.
- Antecedência: **a da empresa** (`organization.renewalLeadDays`), escolhida em [[AlertSettings]] entre
  3, 7, 15, 30 e 60 dias (`RENEWAL_LEAD_OPTIONS`). O padrão é **7 dias** (a da especificação,
  `defaultAlertSettings()`). Em revisão também avisa.
- Cada alerta traz a assinatura, a data da cobrança (`chargeDate`) e os dias até ela (`daysUntil`, 0 =
  hoje). A lista vem da mais próxima para a mais distante.
- A tela fala a distância com `formatDaysUntil`: "hoje", "amanhã", "em 5 dias".
- É sinal derivado: calculado a cada leitura, nunca gravado.

Na demonstração da Exemplo Tecnologia, com a antecedência padrão: GitHub (em 2 dias), Google
Workspace (em 3) e Zoom (em 5).

**Todos os alertas da empresa** (`currentAlerts(assinaturas, empresa, hoje)`, com a antecedência da
empresa): as renovações e depois as duplicidades
([[Redundancia]]), cada uma com uma **chave de situação**:

- renovação: `renovacao:<id da assinatura>:<data da cobrança>`, e a cobrança seguinte é outra
  situação;
- duplicidade: `redundancia:<categoria>:<ids do grupo, ordenados>`, e o grupo mudar é outra situação.

A chave é o que a dispensa grava ([[AlertsCenter]]): o alerta dispensado volta quando a situação
muda.

## Regras de uso

- `today` vem da tela (`toIsoDate(new Date())`).
- Nenhuma tela promete aviso por e-mail ou WhatsApp: a v1 não envia nada.
- Dispensar não mexe nesta regra: a tela filtra `currentAlerts` pelas chaves dispensadas da empresa.

## Histórico de mudanças

- [[2026-09-29-pr-014-alertas-de-renovacao]] — criado.
- [[2026-09-29-pr-033-central-de-alertas]] — `currentAlerts`, com as chaves de situação.
- [[2026-09-29-pr-036-configuracoes-alertas]] — a antecedência passa a ser da empresa.
