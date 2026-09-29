---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 31
url: https://github.com/VictorNascimento14/ZeroSpend/pull/31
branch: feat/onboarding-email
tags: [pr, frontend, onboarding, integracoes]
status: aberto
---

# PR #31 — feat(onboarding): simular a conexão com o e-mail da empresa

## 🎯 Contexto

Ordem 29 de [[2026-09-29-plano-da-v1-do-frontend]]. O fluxo 1 da [[2026-09-29-especificacao-do-mvp]]
tem duas entradas: o extrato do cartão (A, entregue no [[2026-09-29-pr-030-onboarding-extrato]]) e a
caixa de e-mail por OAuth com Google Workspace ou Microsoft 365 (B). A v1 não tem servidor para o OAuth
nem para ler mensagens, então a [[ADR-001-frontend-primeiro-com-dados-locais]] (decisão 6) manda
mostrar a conexão **rotulada como demonstração**.

## 🔧 Mudanças

- **Card "E-mail da empresa"** no [[Onboarding]], abaixo do extrato, com a etiqueta "Demonstração" e os
  botões "Conectar Google Workspace" e "Conectar Microsoft 365" ([[EmailConnectCard]]).
- **O diálogo tem duas etapas:**
  1. **O que a conexão faria:**
     - pediria permissão só de leitura (`gmail.readonly` ou `Mail.Read`);
     - procuraria só as mensagens com os termos da especificação;
     - guardaria só os metadados da fatura.
  2. **"Simular a conexão":** 4 faturas de exemplo (fornecedor, categoria, data e valor na moeda da
     fatura) e o aviso "Nada foi importado".
- `src/lib/import/email.ts` (novo):
  - `InvoiceMetadata`: o que fica de uma fatura;
  - `EMAIL_PROVIDERS`: nome e escopo de cada provedor;
  - `demoInvoices(hoje)`: as faturas de exemplo.
- 2 testes novos (150 no total). Um deles trava a regra de que a fatura tem só os cinco campos de
  metadados.

## 🕵️ Dado sensível (LGPD e sigilo)

Nada é lido nem gravado: a simulação não toca o repositório, e isso foi conferido no navegador (o
`localStorage` fica igual antes e depois). As faturas de exemplo são fictícias, com fornecedores do
catálogo e valores inventados.

A regra da especificação, **nunca guardar o corpo do e-mail**, vira tipo e teste: `InvoiceMetadata` tem
só fornecedor, valor, moeda, data e categoria, e o teste reprova qualquer campo a mais.

## 🧠 Decisões técnicas

- **A simulação não importa nada.** Importar faturas inventadas poria assinaturas falsas na empresa de
  alguém, e um "em revisão" não basta para quem esquece que eram exemplo. A tela mostra o formato do
  resultado, os metadados, e para aí.
- **A tela só promete o que o código faz.** O texto sobre a conexão fica no condicional ("pediria",
  "procuraria", "guardaria"): descreve o que ela faria, sem afirmar que acontece. A etiqueta
  "Demonstração" aparece no card e na descrição do diálogo.
- **O escopo de cada provedor está no código** (`EMAIL_PROVIDERS`). A conexão de verdade vai usar os
  mesmos: `gmail.readonly`, que a especificação pede, e o `Mail.Read` do Microsoft Graph.
- **O card mora em `src/components/integrations/`,** porque a página "Extratos e integrações" (ordem 30)
  vai reusá-lo.
- **O diálogo volta ao começo quando abre, não quando fecha.** Se voltasse ao fechar, o conteúdo
  trocaria durante a animação de saída.
- **Os fornecedores aparecem como monograma** (regra do design system): "GW" e "M3", nas cores do
  catálogo, sem logotipo.

## ⚠️ Armadilhas e aprendizados

- O monograma de "Microsoft 365" sai "M3", porque a regra pega a inicial de cada palavra. É fiel à
  regra, mas estranho. Se incomodar, o caminho é um campo de monograma no catálogo, e não um caso
  especial no `VendorAvatar`.

## 🧪 Como testar

`npm test` (150 testes), lint, type-check e build passam.

No navegador (Chromium headless, 1440px claro e escuro e 390px, com o console limpo):

1. O onboarding mostra o card "E-mail da empresa", com a etiqueta "Demonstração".
2. "Conectar Google Workspace" abre "O que a conexão faria", com `gmail.readonly`.
3. "Simular a conexão" mostra 4 faturas de exemplo (Miro US$ 16,00…) e "Nada foi importado".
4. "Fechar" fecha o diálogo, e o foco volta ao botão. Reabrir pelo Enter recomeça da primeira etapa.
5. "Conectar Microsoft 365" mostra `Mail.Read`.
6. O `localStorage` fica igual. A 390px, o diálogo cabe na tela.

## 🚫 O que não foi verificado

- O OAuth de verdade, que é do backend: a tela da conexão real (estado "conectado", desconectar,
  última varredura) fica <A DEFINIR> quando ele existir.

## 📎 Documentação afetada

- [[EmailConnectCard]] (novo) · [[Onboarding]] · [[glossario]] (verbete "Demonstração")
- [[2026]] (changelog)
