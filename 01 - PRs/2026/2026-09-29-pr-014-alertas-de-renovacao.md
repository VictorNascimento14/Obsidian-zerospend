---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 14
url: https://github.com/VictorNascimento14/ZeroSpend/pull/14
branch: feat/alertas-de-renovacao
tags: [pr, frontend, dominio, alertas]
status: aberto
---

# PR #14 — feat(dominio): alertas de renovação por antecedência

## 🎯 Contexto

Ordem 12 de [[2026-09-29-plano-da-v1-do-frontend]]. A promessa do produto é avisar antes de cada
renovação. A especificação tem um cron que procura `next_billing_date - 7 dias == hoje` e manda
e-mail; na v1 (sem backend), o aviso é na tela — o KPI "alertas urgentes de renovação", o painel do
dashboard e a central de alertas. Esta é a regra que os três vão usar.

## 🔧 Mudanças

- `src/lib/domain/alerts.ts` (novo): `renewalAlerts(assinaturas, hoje, antecedência)`,
  `RenewalAlert` e `DEFAULT_RENEWAL_LEAD_DAYS` (7).
- `dates.ts`: `daysBetween`. `format.ts`: `formatDaysUntil` ("hoje", "amanhã", "em 5 dias").
- `seed.ts`: o ChatGPT Team (em revisão) passa de 6 para 10 dias.
- 7 testes novos (60 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum.

## 🧠 Decisões técnicas

- **Janela, não dia exato.** O cron da especificação dispara uma vez, no dia `cobrança − 7`. Na tela,
  o alerta fica visível de hoje até a antecedência, com as duas pontas incluídas: quem abre o app no
  dia 5 também precisa ver o que renova no dia 6.
- **Pela próxima cobrança efetiva**, não pela data gravada: uma mensal com data vencida avança para a
  próxima cobrança real ([[RegrasDeCobranca]]).
- **Cancelada não avisa; em revisão avisa.** A cobrança do que está em revisão é real até alguém
  descartar ([[2026-09-29-pr-013-repositorio-local]] manda cancelada ficar fora de tudo).
- **Antecedência por parâmetro**, com 7 de padrão: a configuração por empresa é a ordem 34.
- **A demonstração fica com os três alertas do briefing** (GitHub, Google Workspace e Zoom). Com o
  ChatGPT Team a 6 dias, seriam quatro, e o comentário das sementes dizia três.
- O teste dos alertas da demonstração mora em `seed.test.ts`: o domínio não importa a camada de dados.

## ⚠️ Armadilhas e aprendizados

- A regra não envia nada — a v1 não tem quem envie. Pela regra 7 do `CLAUDE.md` (texto que afirma um
  efeito precisa do código que o produz), nenhuma tela pode dizer "vamos te avisar por e-mail".

## 🧪 Como testar

`npm test` — 60 testes, entre eles:

- janela inclusiva (hoje e o sétimo dia avisam, o oitavo não);
- ordem da mais próxima;
- cancelada fora e em revisão dentro;
- data vencida avançada;
- antecedência de 3 dias;
- os três alertas da demonstração.

## 🚫 O que não foi verificado

- Nenhuma tela mostra os alertas ainda (KPI na ordem 18, painel na 20, central na 31).

## 📎 Documentação afetada

- [[AlertasDeRenovacao]] (novo)
- [[ModeloDeDominio]] · [[DadosDeDemonstracao]] · [[glossario]]
- [[ADR-001-frontend-primeiro-com-dados-locais]]
- [[2026]] (changelog)
