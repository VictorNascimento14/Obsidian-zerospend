---
tipo: funcionalidade
camada: frontend
area: Integracoes
rota: /integracoes
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, pagina, integracoes]
---

# Extratos e integrações

## O que é

A área `/integracoes` da [[Casca]]: o endereço fixo das entradas de dados da empresa. O [[Onboarding]]
é a primeira passagem, e aqui a pessoa volta para importar outro extrato e para ver de onde vêm as
assinaturas.

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/app/(app)/integracoes/page.tsx` | a página (metadados "Extratos e integrações · ZeroSpend") |
| `src/components/integrations/source-summary.tsx` | `SourceSummary`: a origem das assinaturas |
| [[StatementImport]] e [[EmailConnectCard]] | o upload do extrato e a conexão do e-mail, os mesmos do onboarding |

## Comportamento

- **Descrição:** "De onde vêm as assinaturas da empresa: o extrato do cartão, o e-mail e o cadastro
  manual."
- **Origem das assinaturas:** três cards no formato dos [[KpiCards]], cada um com o total e a legenda.
  - **Extrato CSV** (ícone cerulean), **E-mail** (plum) e **Cadastro manual** (raspberry).
  - A legenda diz "N em revisão", "Todas confirmadas" ou "Nenhuma assinatura".
  - Cancelada não conta. Nada disso é gravado: a contagem sai do campo de origem de cada assinatura, a
    cada leitura.
- **Extrato do cartão** ([[StatementImport]]) e **E-mail da empresa** ([[EmailConnectCard]], em
  demonstração), lado a lado a partir de 1400px e empilhados abaixo disso. Importar leva ao
  [[Dashboard]], como no onboarding.
- **Grade dos cards de origem:** 1 coluna no celular e 3 a partir de `sm`.

## Estados (vazio, carregando, erro)

- **Carregando:** o esqueleto da [[GuardaDeSessao]].
- **Empresa sem assinaturas:** os três cards com 0 e "Nenhuma assinatura".
- Os estados do upload estão em [[StatementImport]].

## Regras de uso

- **Sem histórico de importações.** A especificação não tem tabela para isso, e a
  [[ADR-001-frontend-primeiro-com-dados-locais]] (decisão 3) manda seguir o modelo dela. Ver
  [[2026-09-29-pr-032-integracoes]].

## Histórico de mudanças

- [[2026-09-29-pr-016-casca]] — criada, com título e descrição.
- [[2026-09-29-pr-032-integracoes]] — origem das assinaturas, upload do extrato e conexão do e-mail.
