---
tipo: funcionalidade
camada: frontend
area: Onboarding
rota: /onboarding
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, pagina, onboarding]
---

# Onboarding

## O que é

"Traga suas assinaturas" é onde a empresa traz os gastos com software para o ZeroSpend. É a primeira
tela depois de [[CriarConta]] e cobre o fluxo 1 da [[2026-09-29-especificacao-do-mvp]].

## Onde está no código

`src/app/(app)/onboarding/page.tsx` (título, descrição e "Ir para o dashboard"), [[StatementImport]] e
[[EmailConnectCard]].

## Comportamento

- **Extrato do cartão (CSV):** subir, revisar e importar. Ver [[StatementImport]].
- **"Ir para o dashboard"**, no topo, para quem prefere cadastrar à mão.
- **Como se chega:**
  - criando a conta ([[criar-conta-ate-o-dashboard]]);
  - pelo "Importar do extrato" do estado vazio do [[Dashboard]] e de [[Assinaturas]].
- **E-mail da empresa** (Google Workspace ou Microsoft 365), abaixo do extrato: **demonstração**. Mostra o
  que a conexão faria e faturas de exemplo, sem ler nem importar nada. Ver [[EmailConnectCard]].

## Estados (vazio, carregando, erro)

- **Carregando:** o esqueleto da [[GuardaDeSessao]].
- Os estados do upload (erro, PDF e revisão) estão em [[StatementImport]].

## Histórico de mudanças

- [[2026-09-29-pr-030-onboarding-extrato]] — criada, com o extrato do cartão.
- [[2026-09-29-pr-031-onboarding-email]] — o card do e-mail da empresa (demonstração).
