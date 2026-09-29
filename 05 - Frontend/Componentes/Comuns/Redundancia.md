---
tipo: funcionalidade
camada: frontend
area: Comuns
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, dominio, redundancia]
---

# Redundância e economia potencial

## O que é

A regra que acha ferramentas que fazem a mesma coisa e estima quanto a empresa economizaria por mês
consolidando cada grupo. Alimenta a tag "Ferramenta redundante" da tabela, o KPI de economia
potencial e os cards de duplicidade do painel.

## Onde está no código

`src/lib/domain/redundancy.ts` — `findRedundancies`, `potentialMonthlySavings`, `redundantIds`,
`RedundancyGroup`. Testes em `redundancy.test.ts` (e o da demonstração em `seed.test.ts`).

## Comportamento

- **Grupo redundante:** duas ou mais assinaturas **não canceladas** na mesma categoria (em revisão
  conta). A categoria "Outros" nunca forma grupo.
- **Dentro do grupo**, as assinaturas vêm da mais cara para a mais barata pelo custo mensal na moeda
  padrão ([[RegrasDeCobranca]]); empate se desfaz pelo nome.
- **Economia do grupo:** mantém a mais cara e soma o custo mensal das outras.
- **Economia potencial da empresa:** a soma das economias dos grupos. A lista de grupos vem da maior
  economia para a menor.
- **Tag da tabela:** toda assinatura de um grupo (inclusive a mais cara) é "Ferramenta redundante".
- É sinal derivado: calculado a cada leitura, nunca gravado.

Na demonstração da Exemplo Tecnologia: CRM (HubSpot fica, Pipedrive economiza R$ 534,60) e Design
(Figma fica, Canva economiza R$ 99,92) — economia potencial de **R$ 634,52**.

## Regras de uso

- O número é estimativa: a tela chama de "economia potencial", nunca de economia garantida.
- A "inatividade" da especificação não existe na v1 (sem dado de uso).

## Histórico de mudanças

- [[2026-09-29-pr-015-redundancia]] — criado.
