---
tipo: funcionalidade
camada: frontend
area: Comuns
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, importacao]
---

# Leitor de extrato

## O que é

A leitura do extrato do cartão corporativo (CSV) e o reconhecimento das assinaturas nele — o começo do
fluxo 1 da [[2026-09-29-especificacao-do-mvp]]. Tudo no navegador; nada sai da máquina.

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/lib/import/csv.ts` | `parseStatement(texto)`, `parseDate`, `parseAmount` |
| `src/lib/import/vendors.ts` | `KNOWN_VENDORS` (o catálogo), `recognizeVendor(descrição)`, `knownVendorByName(nome)` |
| `src/lib/import/recognize.ts` | `readStatement(linhas)`, `toDraft(detecção)`, `isAlreadyTracked(detecção, assinaturas)` |
| `src/lib/domain/text.ts` | `normalizeText`, `toWords` |

## Comportamento

1. **Ler** (`parseStatement`):
   - aceita separador `;` ou `,`, aspas, BOM e `\r\n`;
   - acha as colunas Data, Descrição e Valor pelo começo do nome, sem acento;
   - datas `DD/MM/AAAA`, `DD/MM/AA` ou ISO;
   - valores em pt-BR ("R$ 1.234,56", "(85,00)") ou com ponto decimal.

   Linha que não dá para ler vira problema, com o número da linha e o motivo. Sem as colunas mínimas:
   "Não achei as colunas Data, Descrição e Valor no cabeçalho do arquivo."
2. **Reconhecer** (`readStatement`):
   - o sinal da compra é o da maioria das linhas, e o contrário (pagamento, estorno) é crédito e fica
     de fora;
   - cada compra passa pelo catálogo, que casa palavra inteira;
   - cada fornecedor reconhecido vira uma detecção, com a cobrança mais recente e o número de cobranças;
   - o resto volta como "não reconhecidas".
3. **Virar assinatura** (`toDraft`): o nome e a categoria do catálogo, o valor da cobrança mais
   recente, em real, mensal, com a próxima cobrança um mês depois; status `review_needed`, origem
   `csv_upload`.
4. **Já cadastrada?** (`isAlreadyTracked`): mesmo nome, entre as não canceladas da empresa.

**Catálogo:** 30 fornecedores de SaaS (Google Workspace, Microsoft 365, Slack, Zoom, Figma, Canva,
HubSpot, Pipedrive, RD Station, GitHub, Conta Azul, Gupy, ChatGPT Team…), cada um com categoria e a cor
do monograma ([[SubscriptionsTable]]).

## Regras de uso

- Fornecedor novo entra no catálogo com padrão já em palavras (`toWords`) — o teste confere.
- Padrão genérico demais (só "google", só "amazon") vira falso positivo: prefira o nome do produto.
- As linhas não reconhecidas não são guardadas (extrato é dado sensível).

## Histórico de mudanças

- [[2026-09-29-pr-029-leitor-de-extrato]] — criado.
