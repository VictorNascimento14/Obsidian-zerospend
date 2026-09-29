---
tipo: funcionalidade
camada: frontend
area: Alertas
rota: /alertas
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, pagina]
---

# Alertas

## O que é

A área `/alertas` da [[Casca]]: a central de alertas da empresa. Descrição na tela: "Renovações dentro
da antecedência, ferramentas redundantes e assinaturas em revisão."

## Onde está no código

`src/app/(app)/alertas/page.tsx`, Server Component com os metadados ("Alertas · ZeroSpend"), e
[[AlertsCenter]].

## Comportamento

Renovações e duplicidades com "Dispensar", assinaturas em revisão com "Confirmar" e "Descartar", e os
dispensados com "Voltar a mostrar". Ver [[AlertsCenter]]. Chega-se aqui pela sidebar e pelo "Ver
todos" do [[AlertsPanel]].

## Histórico de mudanças

- [[2026-09-29-pr-016-casca]] — criada, com título e descrição.
- [[2026-09-29-pr-033-central-de-alertas]] — a central de alertas.
