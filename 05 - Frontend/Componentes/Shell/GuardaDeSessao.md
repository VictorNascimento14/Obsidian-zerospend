---
tipo: funcionalidade
camada: frontend
area: Shell
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, sessao, casca]
---

# Guarda de sessão

## O que é

O componente que só deixa a [[Casca]] aparecer para quem tem sessão. Protege o fluxo, não o dado — o
dado está no navegador de qualquer jeito ([[ADR-001-frontend-primeiro-com-dados-locais]]).

## Onde está no código

`src/components/auth/session-gate.tsx` (`SessionGate`), envolvendo a casca em `src/app/(app)/layout.tsx`.
O caminho de volta passa por `safeRedirectPath` (`src/components/auth/safe-redirect.ts`).

## Comportamento

- **Dados locais ainda não lidos** (servidor e hidratação): mostra a moldura vazia da casca, com
  "Carregando…" para leitor de tela.
- **Sem sessão:** troca a rota para `/entrar?para=<caminho atual>`.
- **Com sessão:** mostra a casca e a página.
- **Volta depois de entrar:** `safeRedirectPath` só aceita caminho interno — `//site.com`, `/\site.com`,
  `https://…` e o próprio `/entrar` viram `/dashboard`.

## Estados (vazio, carregando, erro)

- **Carregando:** a moldura vazia (sidebar e header sem conteúdo, dois blocos de esqueleto).
- **Erro:** a guarda é quem lê o armazenamento. Se o navegador o bloquear, o erro sobe até
  `app/error.tsx` e aparece sem a casca ([[TelasDeSistema]]).

## Histórico de mudanças

- [[2026-09-29-pr-017-sessao-e-entrar]] — criada.
- [[2026-09-29-pr-038-telas-de-sistema]] — erro da guarda (armazenamento) na tela de erro da raiz.
