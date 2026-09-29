---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 11
url: https://github.com/VictorNascimento14/ZeroSpend/pull/11
branch: feat/regras-de-cobranca
tags: [pr, frontend, dominio]
status: merged
---

# PR #11 — feat(dominio): valor mensal, conversão de moeda e próxima cobrança efetiva

## 🎯 Contexto

Ordem 9 de [[2026-09-29-plano-da-v1-do-frontend]]. O KPI "gasto total mensal" da especificação é
"convertido para a moeda padrão da empresa", e a tabela mostra "valor/mês" e "próxima cobrança".
Estas são as regras por trás dos três números.

## 🔧 Mudanças

- `types.ts`: `Organization` ganha `defaultCurrency` e `brlPerUsd` (quantos reais vale um dólar).
- `dates.ts`: `addMonths`, que encosta no último dia do mês quando o dia não existe.
- `billing.ts` (novo): `monthlyAmount`, `convertAmount`, `monthlyAmountIn`, `totalMonthlySpend` e
  `nextChargeDate`.
- 12 testes novos (32 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum. Os testes usam a empresa fictícia "Exemplo Tecnologia Ltda" e valores fictícios.

## 🧠 Decisões técnicas

- **Cancelada não entra no gasto; "em revisão" entra.** O `CLAUDE.md` do repositório só exclui a
  cancelada, e o que está em revisão é, na maioria das vezes, cobrança real que ainda não foi
  confirmada.
- **Arredondamento só na tela.** As regras somam o valor cheio (1.000 ÷ 12 = 83,333…), e o
  `formatMoney` arredonda. Somar valores já arredondados faria o total divergir da soma das linhas.
- **A cotação é da empresa**, informada por ela (v1 não busca cotação), e a escrita garante que é
  maior que zero.
- **A próxima cobrança efetiva é derivada, não gravada.** Se a data gravada já passou, a regra avança
  de ciclo em ciclo **a partir da data gravada** (o dia-âncora): 31/01 → 28/02 → 31/03. Somar um mês
  de cada vez à data anterior faria o dia escorregar para 28 para sempre.
- Nomes em inglês, como o resto do domínio: `nextChargeDate`, e não `proximaCobranca`.

## ⚠️ Armadilhas e aprendizados

- Os tetos deliberados ficam anotados no código com `ponytail:`. Dinheiro é `number` (ponto
  flutuante), o que basta para exibir centavos. Com backend, vira `numeric` ou centavos inteiros. A
  conversão também só conhece duas moedas: com uma terceira, a cotação vira tabela por moeda, e o
  código atual converteria errado em silêncio.

## 🧪 Como testar

`npm test`: 32 testes, entre eles:

- anual ÷ 12;
- conversão nos dois sentidos;
- total com moeda mista e cancelada fora;
- 31/01 + 1 mês = 28/02, e 29/02/2028 + 12 meses = 28/02/2029;
- a mensal vencida avança até a primeira cobrança a partir de hoje, e a data-âncora 31/01 volta a 31/03.

## 🚫 O que não foi verificado

- Nenhuma tela usa as regras ainda (os KPIs são a ordem 18).

## 📎 Documentação afetada

- [[RegrasDeCobranca]] (novo)
- [[ModeloDeDominio]]
- [[glossario]]
- [[ADR-001-frontend-primeiro-com-dados-locais]]
- [[2026]] (changelog)
