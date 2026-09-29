---
tipo: funcionalidade
camada: frontend
area: Comuns
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, dominio]
---

# Regras de cobrança

## O que é

Como o ZeroSpend chega ao valor por mês de cada assinatura, ao gasto mensal da empresa na moeda
padrão dela e à próxima data de cobrança. Funções puras, testadas, sobre o [[ModeloDeDominio]].

## Onde está no código

`src/lib/domain/billing.ts` (testes em `billing.test.ts`) e `addMonths` em `src/lib/domain/dates.ts`.

## Comportamento

| Função | Regra |
|---|---|
| `monthlyAmount(assinatura)` | Mensal: o próprio `amount`. Anual: `amount ÷ 12`. Na moeda da assinatura. |
| `convertAmount(valor, de, para, brlPerUsd)` | Dólar → real multiplica pela cotação; real → dólar divide; mesma moeda não muda. |
| `monthlyAmountIn(assinatura, empresa)` | O valor por mês, convertido para a moeda padrão da empresa. |
| `totalMonthlySpend(assinaturas, empresa)` | Soma do valor por mês na moeda padrão. **Cancelada não conta; em revisão conta.** |
| `nextChargeDate(assinatura, hoje)` | A data gravada, se for hoje ou depois. Se já passou, avança de ciclo em ciclo a partir dela (mensal: 1 mês; anual: 12) até a primeira a partir de hoje. |
| `addMonths(data, meses)` | Soma meses; o dia que não existe no destino encosta no último (31/01 + 1 = 28/02). |

Exemplos (cotação de R$ 5,00): anual de US$ 120 → US$ 10/mês → R$ 50/mês. Mensal gravada para
05/09/2026, hoje 29/09/2026 → próxima cobrança 05/10/2026. Âncora 31/01/2026, hoje 05/03/2026 →
31/03/2026 (não 28/03).

## Regras de uso

- Arredonde só na hora de mostrar (`formatMoney`); as regras trabalham com o valor cheio.
- A data gravada nunca é reescrita pela regra: a próxima cobrança efetiva é calculada a cada leitura.
- `today` vem de quem chama (`toIsoDate(new Date())` na tela).
- Tetos anotados no código (`ponytail:`): dinheiro em `number`, e só duas moedas na conversão.
- `DEFAULT_BRL_PER_USD` (5,40) é a cotação com que uma empresa nova começa — valor inicial, não
  cotação do dia.

## Histórico de mudanças

- [[2026-09-29-pr-011-regras-de-cobranca]] — criado.
- [[2026-09-29-pr-018-criar-conta]] — `DEFAULT_BRL_PER_USD`.
