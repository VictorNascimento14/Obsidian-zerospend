---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 38
url: https://github.com/VictorNascimento14/ZeroSpend/pull/38
branch: feat/telas-de-sistema
tags: [pr, frontend, casca]
status: merged
---

# PR #38 — feat(casca): telas de página não encontrada, erro e carregamento

## 🎯 Contexto

Ordem 36 de [[2026-09-29-plano-da-v1-do-frontend]]. Até aqui:

- um endereço errado caía na 404 padrão do Next, em inglês;
- um erro de tela mostrava a tela de erro padrão;
- a nota [[RepositorioLocal]] deixava para esta ordem o caso do armazenamento bloqueado, que derruba a
  leitura dos dados.

## 🔧 Mudanças

- **`src/app/not-found.tsx`** ([[TelasDeSistema]]): "Página não encontrada", com "O endereço não existe
  ou mudou de lugar." e "Ir para o dashboard". Fica fora da casca, com status HTTP 404 e o título
  "Página não encontrada · ZeroSpend".
- **Erro em dois níveis**, com a mesma tela (`ErrorScreen`), "Tentar de novo" e "Ir para o dashboard":
  - `src/app/(app)/error.tsx`: erro numa página. A casca continua, e o erro ocupa só o conteúdo;
  - `src/app/error.tsx`: erro na guarda de sessão, que é quem lê o armazenamento, ou nas telas de
    conta. Aparece sem a casca, com a marca.
- **Armazenamento, com nome:**
  - `SecurityError` (bloqueado) mostra "O navegador não deixou guardar os dados", com o caminho
    (liberar o armazenamento do site ou sair da janela anônima);
  - `QuotaExceededError` (cheio) pede para liberar espaço;
  - o resto mostra "Algo deu errado nesta tela".
- **`src/app/(app)/loading.tsx`:** o esqueleto da página (título, descrição, quatro cards e um bloco)
  dentro da casca, enquanto a página nova chega.
- **`StatusScreen`:** o miolo compartilhado (ícone no tom de informação ou de alerta, título, texto e
  saídas).

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum. O erro vai para o `console.error` do próprio navegador. Nada é enviado, porque não existe
serviço de relatório de erro na v1.

## 🧠 Decisões técnicas

- **Dois `error.tsx`, porque a fronteira de erro de um segmento não pega o `layout` do mesmo segmento.**
  - O `SessionGate` mora em `(app)/layout.tsx` e lê o `localStorage`. O erro dele sobe até o
    `app/error.tsx`.
  - O erro de uma página para no `(app)/error.tsx`, e a pessoa continua com a navegação.
- **O caso do armazenamento tem mensagem própria.** É o erro que a v1 sabe explicar (dado só no
  navegador, ADR-001) e o único com saída do lado de quem usa.
- **A 404 fica fora da casca.** Vale com ou sem sessão, e "Ir para o dashboard" pede para entrar,
  quando é preciso (conferido: sem sessão, leva a `/entrar`).
- **Não há `global-error.tsx`.** O layout raiz só monta tema, fonte e avisos, e quebrar ali é
  improvável. Com o backend (e relatório de erro), ele passa a valer a pena.
- **O contornado "Ir para o dashboard" ganha `bg-card`**, pela mesma razão do
  [[2026-09-29-pr-030-onboarding-extrato]]: sobre o fundo da página, ele some.

## ⚠️ Armadilhas e aprendizados

- **No Next 16.3, o `error.tsx` recebe `retry`,** que refaz o segmento. O `reset` antigo só limpa o
  estado, e a documentação instalada (`node_modules/next/dist/docs`) manda preferir o `retry`, estável
  desde a 16.3.0.
- **O `not-found.tsx` comum aceita `metadata`** (conferido: o título sai "Página não encontrada ·
  ZeroSpend"), embora a documentação só mostre o exemplo no `global-not-found`.
- **O esqueleto de carregamento só aparece com rede lenta.** Localmente a navegação é instantânea. Para
  ver o esqueleto, o teste simula a rede pelo CDP (`Network.emulateNetworkConditions`).

## 🧪 Como testar

`npm test` (169 testes), lint, type-check e build passam.

No navegador (Chromium headless):

1. `/nao-existe` responde com status 404 e o título "Página não encontrada · ZeroSpend", no claro a
   1440px e no escuro a 390px, sem vazar. "Ir para o dashboard" sem sessão leva a `/entrar`.
2. Com o `localStorage` lançando `SecurityError`, `/dashboard` mostra "O navegador não deixou guardar os
   dados", com "Tentar de novo" e "Ir para o dashboard".
3. Com uma assinatura de nome `null` gravada de propósito, `/assinaturas` mostra "Algo deu errado
   nesta tela", e a sidebar continua.
4. Com a rede lenta simulada, navegar para Alertas mostra o esqueleto dentro da casca.

## 🚫 O que não foi verificado

- `QuotaExceededError` num navegador de verdade com a cota cheia: a mensagem existe, mas o teste só
  simulou o bloqueio.
- Relatório de erro para a equipe: <A DEFINIR> com o backend.

## 📎 Documentação afetada

- [[TelasDeSistema]] (nova) · [[RepositorioLocal]] · [[Casca]] · [[GuardaDeSessao]]
- [[2026]] (changelog)
