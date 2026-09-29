---
tipo: funcionalidade
camada: frontend
area: Comuns
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, dados]
---

# Repositório local

## O que é

A única porta de leitura e escrita dos dados na v1 ([[ADR-001-frontend-primeiro-com-dados-locais]]):
guarda empresas e assinaturas no `localStorage` do navegador, valida toda escrita e avisa a tela
quando algo muda. É aqui que o Supabase entra depois — as telas não mudam.

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/lib/data/repository.ts` | `createRepository(armazenamento, hoje)`, `Database`, `ValidationError`, `STORAGE_KEY`, `BACKUP_KEY` |
| `src/lib/data/store.ts` | `getRepository()` (o do navegador) e `useDatabase()` |
| `src/lib/domain/validation.ts` | `validateSubscriptionDraft`, `SubscriptionDraft`, `FieldErrors` |

## Comportamento

- **Formato:** `{ version: 2, organizations, subscriptions, users, memberships, session }` na chave
  `zerospend:v2` (a v1, sem usuários, fica intacta no navegador — ver [[SessaoLocal]]).
- **Navegador vazio:** a primeira leitura semeia a demonstração ([[DadosDeDemonstracao]]) com o dia
  local e grava.
- **Conteúdo ilegível** (JSON quebrado, outra versão): copiado para `zerospend:v2:backup`, e a
  demonstração é semeada de novo.
- **Escrita:**
  - `addSubscription(empresa, rascunho)`: exige empresa existente;
  - `addSubscriptions(empresa, rascunhos)`: a importação do extrato. Valida todas antes e grava de uma
    vez, então ou entram todas ou nenhuma (`addSubscription` é o caso de uma só);
  - `updateSubscription(id, mudanças)`: valida a mistura com o que já existia;
  - `removeSubscription(id)`: devolve a removida, para o "desfazer";
  - `restoreSubscription(assinatura)`: o "desfazer" — volta com o mesmo id, uma vez só;
  - `updateOrganization(empresa, rascunho)`: nome, moeda padrão e cotação. Valida e só aceita quem
    está na sessão e administra a empresa ([[OrganizationSettings]]);
  - `updateAlertSettings(empresa, preferências)`: antecedência e canais, com a mesma regra de papel
    ([[AlertSettings]]);
  - `inviteMember(empresa, convite)`, `revokeInvitation(id)` e `removeMember(empresa, pessoa)`: membros
    e convites, com a regra de papel, e ninguém tira o próprio acesso ([[MembersSettings]]);
  - `dismissAlert(empresa, chave)` e `restoreAlert(empresa, chave)`: as dispensas de alerta
    (`dismissals`), e dispensar de novo não muda nada ([[AlertsCenter]]).

  Toda escrita valida, grava só os campos do modelo e avisa quem assina. Erro de campo sobe como
  `ValidationError` (com `fields`), e o armazenamento cheio sobe como a exceção do navegador — nos
  dois casos, nada muda.
- **Outra aba gravou:** o evento `storage` invalida o cache, e a tela relê.
- **Servidor e hidratação:** `useDatabase()` devolve `null`.

## Estados (vazio, carregando, erro)

- **Carregando:** `useDatabase()` é `null` — a tela mostra esqueleto.
- **Vazio:** não acontece na v1, porque o primeiro acesso semeia a demonstração.
- **Erro:** armazenamento bloqueado derruba a leitura (a tela de erro é a ordem 36 do plano).

## Regras de uso

- Tela **nunca** toca `localStorage`: lê por `useDatabase()` e escreve por `getRepository()`.
- A validação da tela é conforto; a do repositório é a regra.
- **Campo novo, com padrão na leitura, entra na mesma chave**, como coluna nova com valor padrão no
  banco: o `parse` completa o que falta (`dismissals` e `invitations` ausentes viram `[]`; empresa sem antecedência e
  canais ganha `defaultAlertSettings()`), e o que já estava gravado
  continua valendo. Há teste para isso.
- **Mudança que quebra a leitura antiga** (renomear, trocar tipo, remover) pede chave nova e
  **migração**, nunca ressemear por cima de dado de alguém. Ver
  [[ADR-001-frontend-primeiro-com-dados-locais]] (Atualizações).

## Histórico de mudanças

- [[2026-09-29-pr-013-repositorio-local]] — criado.
- [[2026-09-29-pr-017-sessao-e-entrar]] — versão 2: usuários, vínculos e sessão (`startSession`, `endSession`).
- [[2026-09-29-pr-025-excluir-assinatura]] — `restoreSubscription`.
- [[2026-09-29-pr-030-onboarding-extrato]] — `addSubscriptions` (tudo ou nada).
- [[2026-09-29-pr-033-central-de-alertas]] — `dismissals`, `dismissAlert` e `restoreAlert`; campo novo com padrão na leitura.
- [[2026-09-29-pr-035-configuracoes-empresa]] — `updateOrganization`, com a conferência de papel.
- [[2026-09-29-pr-036-configuracoes-alertas]] — `updateAlertSettings`; empresa sem preferências lida com o padrão.
- [[2026-09-29-pr-037-configuracoes-membros]] — `invitations`, `inviteMember`, `revokeInvitation`, `removeMember`; `addAccount` aceita convites.
