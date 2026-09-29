---
tipo: funcionalidade
camada: frontend
area: Comuns
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, sessao, autenticacao]
---

# Sessão local

## O que é

Quem está usando o ZeroSpend neste navegador, em qual empresa, e como a senha é conferida. É a
"autenticação" da v1: local, e **não é segurança** — é o formato do fluxo
([[ADR-001-frontend-primeiro-com-dados-locais]]).

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/lib/domain/types.ts` | `User` (com `passwordHash` e `passwordSalt`), `Membership`, `Role` |
| `src/lib/data/repository.ts` | `Session`, `CurrentSession`, `currentSession(banco)`, `startSession`, `endSession` |
| `src/lib/data/password.ts` | `hashPassword(senha, sal)` (PBKDF2-SHA-256, 100 mil iterações) e `newSalt()` |
| `src/lib/data/auth.ts` | `signIn(repositório, e-mail, senha)`, `SignInError`, `normalizeEmail` |
| `src/lib/data/store.ts` | `useSession()` — a sessão resolvida, para a tela |

## Comportamento

- **Pessoa e empresas:** `memberships` liga pessoa a empresa com papel (`admin` ou `member`); uma
  pessoa pode ter várias empresas.
- **Sessão:** `{ userId, organizationId }` gravada no banco local. `startSession` abre na primeira
  empresa da pessoa (sem empresa, recusa); `endSession` apaga.
- **`currentSession`:** resolve pessoa, empresa e papel — `null` se a sessão aponta para algo que não
  existe mais.
- **Entrar:** `signIn` normaliza o e-mail (minúsculo, sem espaço nas pontas), recalcula o hash com o
  sal da pessoa e compara. E-mail desconhecido e senha errada dão a mesma mensagem: "E-mail ou senha
  incorretos."
- **Senha:** PBKDF2 com sal por pessoa; nunca em texto. A Web Crypto só existe em https ou
  `localhost` — fora disso, `hashPassword` falha com a explicação.
- **Conta de demonstração:** `admin@zerospend.app`, senha "demonstracao" ([[DadosDeDemonstracao]]).

## Regras de uso

- Nenhuma tela promete sigilo: a conta vive só no navegador.
- Senha nova sempre com `newSalt()` + `hashPassword` — nunca guardar o texto.

## Histórico de mudanças

- [[2026-09-29-pr-017-sessao-e-entrar]] — criada.
