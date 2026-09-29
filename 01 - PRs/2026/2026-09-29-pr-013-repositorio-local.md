---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 13
url: https://github.com/VictorNascimento14/ZeroSpend/pull/13
branch: feat/repositorio-local
tags: [pr, frontend, dados]
status: merged
---

# PR #13 — feat(dados): repositório local com validação

## 🎯 Contexto

Ordem 11 de [[2026-09-29-plano-da-v1-do-frontend]]. É a fronteira da
[[ADR-001-frontend-primeiro-com-dados-locais]]: toda leitura e escrita de dado passa por
`src/lib/data/`, e é aqui que o Supabase entra depois, sem mexer nas telas.

## 🔧 Mudanças

- `src/lib/data/repository.ts` (novo): `createRepository(armazenamento, hoje)`, com as operações:
  - `getDatabase`, `subscribe` e `invalidate`;
  - `addSubscription`, `updateSubscription` e `removeSubscription`;
  - `ValidationError`, com o erro de cada campo;
  - as chaves `STORAGE_KEY` (`zerospend:v1`) e `BACKUP_KEY`.
- `src/lib/data/store.ts` (novo): `getRepository()`, o repositório do navegador sobre o
  `window.localStorage`, que relê quando outra aba grava. Tem também `useDatabase()`, que usa
  `useSyncExternalStore`.
- `src/lib/domain/validation.ts` (novo): `SubscriptionDraft`, `FieldErrors` e
  `validateSubscriptionDraft`.
- `src/lib/domain/types.ts`: as uniões viram listas em tempo de execução (`CURRENCIES`,
  `BILLING_CYCLES`, `SUBSCRIPTION_STATUSES`, `SUBSCRIPTION_SOURCES`), com o tipo derivado delas.
- 16 testes novos (53 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

A partir daqui, o navegador guarda as assinaturas da empresa no `localStorage`, legível por qualquer
script da mesma origem e por quem usa a máquina. É o combinado da ADR-001 para a v1, e nada disso é
segurança. Por enquanto, os únicos dados são os fictícios da demonstração.

## 🧠 Decisões técnicas

- **O armazenamento entra por parâmetro.** `createRepository` recebe qualquer coisa com
  `getItem`/`setItem`: no app, o `localStorage`; nos testes, um `Map`. O Vitest continua no ambiente
  `node`, sem jsdom.
- **Primeira leitura de um navegador vazio semeia a demonstração** ([[DadosDeDemonstracao]]) com o
  dia local, e grava.
- **Nada se perde calado:** conteúdo ilegível (JSON quebrado, versão desconhecida) vai para
  `zerospend:v1:backup` antes de semear de novo.
- **Toda escrita valida o resultado final** (na edição, a mistura do que existia com o que mudou) e
  grava campo a campo — o que não é do modelo não entra. Se o `setItem` falhar (cota cheia), a exceção
  sobe e o estado em memória não muda.
- **`getDatabase` devolve a mesma referência até alguém gravar.** O `useSyncExternalStore` exige isso;
  com um objeto novo a cada leitura, o React entra em laço.
- **No servidor, `useDatabase()` é `null`.** O HTML sai com esqueleto, e o primeiro render do cliente
  é igual a ele (sem divergência de hidratação); os dados entram logo depois.
- **Validação pura no domínio, aplicada no repositório.** A tela pode usar
  `validateSubscriptionDraft` para mostrar o erro antes (conforto), mas a regra é a do repositório.
- **Id novo por `crypto.randomUUID`**, com alternativa por `getRandomValues`: o `randomUUID` só existe
  em contexto seguro, e quem abre o app pelo IP da rede local (celular) não teria.

## ⚠️ Armadilhas e aprendizados

- O lint roda com `--max-warnings=0`: desestruturar um campo só para descartá-lo (`const { id: _id,
  ...resto }`) vira aviso e reprova. Aqui, o registro já é montado campo a campo, então a mistura
  inteira pode passar.
- Duas abas gravando quase ao mesmo tempo: vale a última (a outra relê pelo evento `storage`). Limite
  aceito para dados locais.

## 🧪 Como testar

1. `npm test` — 53 testes. Os do repositório cobrem:
   - semear e reler;
   - referência estável;
   - backup de conteúdo ilegível;
   - criar, editar e remover;
   - erro por campo sem gravar;
   - empresa inexistente;
   - cota cheia sem mudar nada;
   - id sem `randomUUID`.
2. Vitrine temporária (fora do commit) com `useDatabase()` num Chromium headless:
   - o HTML do servidor traz o esqueleto;
   - o cliente mostra "19 assinaturas em 2 empresas";
   - o `localStorage` ganha só `zerospend:v1`;
   - recarregar relê;
   - o console fica limpo.

## 🚫 O que não foi verificado

- Nenhuma tela usa o repositório ainda (a casca é a ordem 14).
- Navegador com armazenamento bloqueado: a leitura falha, e a tela de erro é a ordem 36.

## 📎 Documentação afetada

- [[RepositorioLocal]] (novo)
- [[ModeloDeDominio]]
- [[ADR-001-frontend-primeiro-com-dados-locais]]
- [[2026]] (changelog)
