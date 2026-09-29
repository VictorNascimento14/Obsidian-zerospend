---
tipo: runbook
camada: infra
escopo: CI do repositório de código e checagem de documentação do PR
ultima_atualizacao: 2026-09-29
tags: [runbook, infra, ci]
tempo_estimado: 5 min
---

# Runbook — CI do ZeroSpend

## Quando usar

Um check do PR ficou vermelho, ou você quer saber o que o CI confere antes de abrir um PR.

## O que roda

| Workflow | Job | Quando | O que confere |
|---|---|---|---|
| `ci.yml` | `lint · type-check · build` | todo PR e todo push na `main` | `npm ci`, `npm run lint`, `npm run type-check`, `npm run build` (Node 22) |
| `pr-documentacao.yml` | `seção 📓 Documentação` | PR aberto, editado, reaberto ou com push novo | o corpo tem `## 📓 Documentação` com ao menos um link para o cofre |

## Passos

1. `gh pr checks <NNN>` mostra qual job falhou; `gh run view <id> --log-failed` mostra só as linhas
   da falha.
2. **Falhou `lint · type-check · build`:** reproduza local com os mesmos comandos do CI —
   `npm ci && npm run lint && npm run type-check && npm run build`. Conserte, commite e dê push: a
   rodada anterior do PR é cancelada sozinha.
3. **Falhou `seção 📓 Documentação`:** o código está certo; falta o corpo do PR. Edite o corpo com
   `gh pr edit <NNN> --body-file corpo.md` (se falhar,
   `gh api repos/:owner/:repo/pulls/<NNN> -X PATCH -F body=@corpo.md`). A edição dispara a checagem
   de novo, sem precisar de commit.

## Como saber que deu certo

`gh pr checks <NNN>` com os dois jobs em `pass`. Criado em [[2026-09-29-pr-002-ci]]; rodar o app e os
checks na máquina está em [[runbook-rodar-local]].
