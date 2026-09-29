---
tipo: funcionalidade
camada: frontend
area: Shell
rota: 404 · erro · carregamento
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, pagina, casca]
---

# Telas de sistema

## O que é

O que aparece quando o endereço não existe, quando uma tela quebra e enquanto uma página chega.

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/app/not-found.tsx` | a página não encontrada (404), fora da casca |
| `src/app/(app)/error.tsx` | erro numa página do app, dentro da casca |
| `src/app/error.tsx` | erro fora de uma página (guarda de sessão, telas de conta), sem a casca |
| `src/app/(app)/loading.tsx` | o esqueleto da página, dentro da casca |
| `src/components/layout/error-screen.tsx` | `ErrorScreen`: o texto certo para cada erro, "Tentar de novo" (`retry`) e "Ir para o dashboard" |
| `src/components/layout/status-screen.tsx` | `StatusScreen`: ícone (informação ou alerta), título, texto e saídas |

## Comportamento

- **404** (status HTTP 404, título "Página não encontrada · ZeroSpend"):
  - "Página não encontrada — O endereço não existe ou mudou de lugar.";
  - "Ir para o dashboard", que pede para entrar se não houver sessão.
- **Erro:**
  - armazenamento bloqueado (`SecurityError`): "O navegador não deixou guardar os dados — Nesta
    versão, o ZeroSpend guarda os dados só neste navegador. Libere o armazenamento do site (ou saia da
    janela anônima) e tente de novo.";
  - armazenamento cheio (`QuotaExceededError`): "O armazenamento deste navegador para o site está
    cheio. Libere espaço nos dados do site e tente de novo.";
  - qualquer outro: "Algo deu errado nesta tela — Tente de novo. Se o erro continuar, recarregue a
    página.".

  O ícone de alerta vem em `warning`, e o erro vai para o `console.error`.
- **Onde o erro aparece:** numa página, a casca continua (sidebar e header). Na guarda de sessão, que lê
  o armazenamento, a tela aparece sozinha, com a marca.
- **Carregamento:** título, descrição, quatro cards e um bloco em esqueleto, com `aria-busy` e
  "Carregando…" para o leitor de tela. O primeiro acesso usa o esqueleto da [[GuardaDeSessao]].

## Estados (vazio, carregando, erro)

Estas telas são os próprios estados de erro e carregamento do app.

## Regras de uso

- Erro novo com saída do lado de quem usa ganha mensagem própria em `ErrorScreen`. O resto fica no
  texto geral.
- Não existe `global-error.tsx` enquanto o layout raiz for só tema, fonte e avisos.

## Histórico de mudanças

- [[2026-09-29-pr-038-telas-de-sistema]] — criadas.
