---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 29
url: https://github.com/VictorNascimento14/ZeroSpend/pull/29
branch: feat/leitor-de-extrato
tags: [pr, frontend, importacao]
status: merged
---

# PR #29 — feat(importacao): leitor de extrato CSV e reconhecimento de fornecedores

## 🎯 Contexto

Ordem 27 de [[2026-09-29-plano-da-v1-do-frontend]]. O fluxo 1 da
[[2026-09-29-especificacao-do-mvp]] começa pelo "upload de CSV" do extrato do cartão (campos mínimos
Data, Descrição, Valor), e a especificação diz que a v1 reconhece fornecedores por um catálogo local
(a IA de extração vem com o backend). Este PR é a leitura; a tela de upload é a ordem 28.

## 🔧 Mudanças

- `src/lib/import/csv.ts`: `parseStatement` (o extrato), `parseDate` e `parseAmount`.
- `src/lib/import/vendors.ts`: `KNOWN_VENDORS` (30 fornecedores, com categoria e cor), `recognizeVendor`
  e `knownVendorByName`.
- `src/lib/import/recognize.ts`: `readStatement` (compras por fornecedor, não reconhecidas e créditos),
  `toDraft` (detecção → assinatura em revisão) e `isAlreadyTracked`.
- `src/lib/domain/text.ts` (novo): `normalizeText` e `toWords` — a normalização que estava na tabela
  e agora serve à importação também.
- `VendorAvatar`: o monograma usa a cor do catálogo quando o fornecedor é conhecido.
- 29 testes novos (143 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

**Extrato de cartão é dado sensível** — tem compras que não são software, às vezes nome de portador. O
arquivo é lido **só no navegador**; nada é enviado a lugar nenhum. Da leitura, só as assinaturas
reconhecidas viram registro (e só quando a pessoa confirmar a importação, na ordem 28); as linhas
não reconhecidas não são guardadas. Os testes usam só descrições fictícias ou nomes públicos de
produto.

## 🧠 Decisões técnicas

- **Feito para o extrato brasileiro:**
  - separador `;` ou `,` (o que mais aparece no cabeçalho), aspas com `""`, BOM e `\r\n`;
  - cabeçalho por começo de palavra, sem acento ("Data da compra", "Histórico", "Valor (R$)");
  - datas `DD/MM/AAAA`, `DD/MM/AA` e ISO, sem passar por `Date`;
  - valores "R$ 1.234,56", "(85,00)", "-85,00" e "1234.56". O decimal é o último separador, e "1.234"
    sem centavos é milhar.
- **Linha ruim não derruba o arquivo:** vira um problema com o número da linha e o motivo ("Data que não
  dá para ler."). Só a falta das colunas mínimas recusa o arquivo inteiro.
- **O sinal da compra é o da maioria.** Cada banco escreve de um jeito (compra positiva ou negativa); o
  sinal contrário é pagamento da fatura ou estorno, e fica de fora.
- **Catálogo por palavra inteira** (`toWords`): "ZOOM.US 888…" casa "zoom"; "ZOOMCAR" não. O mais
  específico vem antes, e "google" sozinho não é padrão ("GOOGLE PLAY" não é assinatura).
- **Detecção = cobrança mais recente, mensal, em real,** com a próxima cobrança um mês depois. Um
  extrato não mostra o ciclo, e o valor do cartão já está em reais, mesmo quando o serviço cobra em
  dólar. Entra **em revisão** (fluxo 2 da especificação) — a pessoa confirma ou descarta
  ([[2026-09-29-pr-026-revisar-deteccao]]).
- **O não reconhecido volta numa lista,** para a tela mostrar — ninguém deve achar que a compra sumiu.
- **O catálogo dá a cor do monograma** dos fornecedores conhecidos (era o teto `ponytail:` do
  [[2026-09-29-pr-021-tabela-de-assinaturas]]).

## ⚠️ Armadilhas e aprendizados

- Linha em branco no meio do CSV não é problema, é espaço: é pulada sem aviso.
- "1.234" é milhar no Brasil e decimal nos EUA. Sem vírgula e sem dois dígitos depois do ponto, fica
  milhar.

## 🧪 Como testar

`npm test` — 143 testes:

- os dois formatos de arquivo, os sinônimos de cabeçalho e as linhas ruins com o motivo;
- oito jeitos de escrever valor e as datas;
- seis descrições reais de extrato reconhecidas, e "ZOOMCAR", "GOOGLE PLAY" e "PADARIA" não;
- a integridade do catálogo;
- agrupamento, créditos e sinal negativo;
- a detecção vira rascunho válido;
- 500 linhas lidas bem abaixo de 1 s (a especificação pede menos de 10 s).

## 🚫 O que não foi verificado

- Extratos reais de cada banco brasileiro: a leitura cobre os formatos comuns, e a tela (ordem 28)
  mostra o que não deu para ler.
- PDF: a especificação cita, mas a v1 só lê CSV (a leitura de PDF é demonstração, como manda a ADR-001).

## 📎 Documentação afetada

- [[LeitorDeExtrato]] (novo) · [[SubscriptionsTable]] · [[ModeloDeDominio]] · [[glossario]]
- [[2026]] (changelog)
