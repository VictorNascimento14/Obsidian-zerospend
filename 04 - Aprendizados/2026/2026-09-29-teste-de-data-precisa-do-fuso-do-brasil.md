---
tipo: aprendizado
data: 2026-09-29
contexto: PR #10 (domínio)
tags: [aprendizado, dominio, testes]
autor: VictorNascimento14
---

# Teste de data precisa do fuso do Brasil

## O sintoma

Nenhum — e esse é o problema. `new Date("2026-10-05")` é meia-noite **UTC**; no Brasil (UTC−3), isso
é dia 4 às 21h. Um código que faz isso mostra a próxima cobrança um dia antes na tela de quem usa. O CI
do GitHub roda em **UTC**, onde o mesmo código cai no dia certo: o teste passa e o bug chega à
produção.

## A causa

Data sem hora (`YYYY-MM-DD`) é interpretada pelo `Date` como UTC, e os getters (`getDate()`) leem no
fuso local. Os dois só coincidem em UTC.

## A correção

Duas camadas, no [[2026-09-29-pr-010-dominio]]:

1. **O código não passa data sem hora por `Date`.** Os helpers de `src/lib/domain/` tratam a string
   (`formatDate` corta, `isIsoDate` valida por `Date.UTC`), e o "hoje" vem de `toIsoDate(new Date())`,
   que lê o dia local.
2. **Os testes rodam em São Paulo**, fixado no `vitest.config.mts` (`process.env.TZ`), em qualquer
   máquina e no CI. Dois testes de guarda conferem o fuso e reproduzem a armadilha. Sem a fixação, em
   UTC, os dois falham — conferido.

## Como evitar

- Data sem hora é string até o fim; aritmética de datas usa `Date.UTC` e getters `getUTC*`.
- Não tire a linha do `TZ` do `vitest.config.mts` — os testes de guarda existem para acusar isso.
