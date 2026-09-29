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
| `src/lib/data/auth.ts` | `signIn(repositório, e-mail, senha)`, `SignInError`, `createAccount(repositório, rascunho)` e o `normalizeEmail` (que mora em `src/lib/domain/text.ts`) |
| `src/lib/domain/validation.ts` | `validateAccountDraft` e `PERSONAL_EMAIL_DOMAINS` (e-mail corporativo) |
| `src/lib/data/store.ts` | `useSession()` — a sessão resolvida, para a tela |

## Comportamento

- **Pessoa e empresas:** `memberships` liga pessoa a empresa com papel (`admin` ou `member`); uma
  pessoa pode ter várias empresas.
- **Sessão:** `{ userId, organizationId }` gravada no banco local. `startSession` abre na primeira
  empresa da pessoa (sem empresa, recusa); `endSession` apaga.
- **`currentSession`:** resolve pessoa, empresa, papel e a lista de empresas da pessoa (em ordem
  alfabética) — `null` se a sessão aponta para algo que não existe mais.
- **Trocar de empresa:** `selectOrganization(id)` só aceita empresa em que a pessoa tem vínculo.
- **Empresa nova:** `addOrganization(nome)` cria em real, com a cotação inicial, dá o vínculo de
  administração e passa a sessão para ela.
- **Entrar:** `signIn` normaliza o e-mail (minúsculo, sem espaço nas pontas), recalcula o hash com o
  sal da pessoa e compara. E-mail desconhecido e senha errada dão a mesma mensagem: "E-mail ou senha
  incorretos."
- **Senha:** PBKDF2 com sal por pessoa; nunca em texto. A Web Crypto só existe em https ou
  `localhost` — fora disso, `hashPassword` falha com a explicação.
- **Criar conta:** `createAccount` normaliza e valida (nome, e-mail corporativo, senha de 8+, empresa),
  recusa e-mail que já tem conta e grava pessoa + empresa (em real, cotação inicial R$ 5,40) +
  vínculo de administração + sessão numa só gravação (`addAccount`). Os **convites pendentes** para o
  e-mail viram vínculo na mesma gravação: a pessoa entra também nas empresas que a convidaram
  ([[MembersSettings]]).
- **Conta de demonstração:** `admin@zerospend.app`, senha "demonstracao" ([[DadosDeDemonstracao]]).

## Regras de uso

- Nenhuma tela promete sigilo: a conta vive só no navegador.
- Senha nova sempre com `newSalt()` + `hashPassword` — nunca guardar o texto.

## Histórico de mudanças

- [[2026-09-29-pr-017-sessao-e-entrar]] — criada.
- [[2026-09-29-pr-018-criar-conta]] — `createAccount` e a regra de e-mail corporativo.
- [[2026-09-29-pr-019-seletor-de-empresa]] — `selectOrganization`, `addOrganization` e a lista de empresas na sessão.
- [[2026-09-29-pr-037-configuracoes-membros]] — a conta nova aceita os convites pendentes do e-mail.
