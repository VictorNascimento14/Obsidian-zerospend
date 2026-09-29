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

`src/lib/domain/alerts.ts` — `renewalAlerts`, `RenewalAlert`, `DEFAULT_RENEWAL_LEAD_DAYS`. A frase
da distância é `formatDaysUntil` (`format.ts`). Testes em `alerts.test.ts`.

## Comportamento

- Entra no alerta toda assinatura **não cancelada** cuja **próxima cobrança efetiva**
  ([[RegrasDeCobranca]]) cai **de hoje até hoje + antecedência**, com as duas pontas incluídas.
- Antecedência padrão: **7 dias** (a da especificação). Em revisão também avisa.
- Cada alerta traz a assinatura, a data da cobrança (`chargeDate`) e os dias até ela (`daysUntil`, 0 =
  hoje). A lista vem da mais próxima para a mais distante.
- A tela fala a distância com `formatDaysUntil`: "hoje", "amanhã", "em 5 dias".
- É sinal derivado: calculado a cada leitura, nunca gravado.

Na demonstração da Exemplo Tecnologia, com a antecedência padrão: GitHub (em 2 dias), Google
Workspace (em 3) e Zoom (em 5).

## Regras de uso

- `today` vem da tela (`toIsoDate(new Date())`).
- Nenhuma tela promete aviso por e-mail ou WhatsApp: a v1 não envia nada.
- Dispensar um alerta (central de alertas, ordem 31) é outra regra, sobre esta lista.

## Histórico de mudanças

- [[2026-09-29-pr-014-alertas-de-renovacao]] — criado.
