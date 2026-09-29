---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 17
url: https://github.com/VictorNascimento14/ZeroSpend/pull/17
branch: feat/sessao-e-entrar
tags: [pr, frontend, sessao, autenticacao]
status: aberto
---

# PR #17 — feat(sessao): sessão local, guarda de rota e tela de entrar

## 🎯 Contexto

Ordem 15 de [[2026-09-29-plano-da-v1-do-frontend]]. A
[[ADR-001-frontend-primeiro-com-dados-locais]] manda a "autenticação" da v1 ser local — "não é
segurança, é o formato do fluxo". Aqui entram a pessoa, as empresas dela, a sessão, a tela de entrar e
a guarda que protege as áreas do app.

## 🔧 Mudanças

- **Banco local na versão 2** (`zerospend:v2`): além de empresas e assinaturas, `users`, `memberships`
  (quem acessa qual empresa, com papel) e `session`. `currentSession(banco)` resolve pessoa, empresa
  atual e papel; `startSession` e `endSession` no repositório.
- `src/lib/data/password.ts`: `hashPassword` (PBKDF2-SHA-256, 100 mil iterações) e `newSalt`.
- `src/lib/data/auth.ts`: `signIn(repositório, e-mail, senha)` e `normalizeEmail`.
- Sementes: a conta de demonstração (Admin Exemplo, `admin@zerospend.app`, senha "demonstracao")
  administra as duas empresas.
- `/entrar` (grupo `(auth)`, fora da casca): formulário, "Entrar na conta de demonstração" e o aviso
  de que a conta fica só no navegador.
- `SessionGate` em volta da casca; `safeRedirectPath` para o `?para=`; `useSession()`; `UserMenu`
  (avatar com nome, e-mail e "Sair") no header.
- 20 testes novos (87 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

- **Senha:** fica no `localStorage` como PBKDF2 com sal por pessoa, nunca em texto. Não é segurança —
  quem usa a máquina lê o armazenamento (ADR-001) —, mas quem digitar a senha real não a deixa legível.
  A tela avisa: "não use uma senha que você usa em outro lugar".
- **E-mail:** o login não revela quais e-mails têm conta (a mesma mensagem para e-mail desconhecido e
  senha errada).
- A conta de demonstração usa o e-mail fictício estável do cofre.

## 🧠 Decisões técnicas

- **Versão 2, chave nova, sem migração.** A v1 (sem usuários) nunca saiu de máquina de
  desenvolvimento; ela fica intacta no navegador, e a v2 começa da demonstração.
- **`memberships` em vez de `organization_id` no usuário** (como na especificação): o seletor de
  empresa do briefing e o BPO financeiro pedem uma pessoa em várias empresas, com papel por empresa.
- **Hash por PBKDF2 da Web Crypto**, assíncrono: a conferência fica em `signIn`, fora do repositório
  (que segue síncrono). A Web Crypto só existe em contexto seguro; fora dele, a tela explica.
- **O hash da demonstração é constante nas sementes** (sal fixo "ZEROSPEND-DEMO-1"), e um teste
  confere que ele bate com "demonstracao".
- **A guarda envolve a casca inteira:** sem sessão confirmada, nem a sidebar aparece. No servidor e na
  hidratação, a moldura vazia (com "Carregando…" para leitor de tela) — o HTML nunca diverge.
- **`?para=` só aceita caminho interno** (`//site.com`, `/\site.com` e `https://…` voltam para o
  dashboard) e nunca `/entrar`, que viraria laço.
- **"Sair" só encerra a sessão;** quem leva para `/entrar` é a guarda.

## ⚠️ Armadilhas e aprendizados

- `tsc` sozinho não conhece `PageProps<"/entrar">` de uma rota nova: o `type-check` do projeto roda
  `next typegen` antes, e é ele que vale.
- Teste de interface que procura `[role="alert"]` acha primeiro o anunciador de rota do Next (que lê o
  título da página). A mensagem de erro se acha por `form p[role="alert"]`.
- Os esqueletos sobre o fundo da página usam `bg-accent`: o `bg-muted` padrão do `Skeleton` é a mesma
  cor do fundo no claro ([[Tema]]).

## 🧪 Como testar

1. `npm test` — 87 testes (sessão, `signIn`, senha, conta de demonstração, `safeRedirectPath`).
2. Percorrido num Chromium headless, com o console limpo:
   - o HTML do servidor em `/dashboard` traz "Carregando…";
   - sem sessão, `/dashboard` → `/entrar?para=%2Fdashboard`;
   - senha errada e e-mail sem conta → "E-mail ou senha incorretos.";
   - com a senha certa, entra, e o avatar mostra "AE";
   - "Sair" volta para `/entrar`;
   - o botão da demonstração entra;
   - `/entrar` com sessão pula para o dashboard;
   - com `?para=//site.com`, entrar leva ao dashboard.

## 🚫 O que não foi verificado

- Criar conta é a ordem 16 (próximo PR); até lá, só a demonstração entra.
- App aberto pelo IP da rede local, sem https: a Web Crypto some e entrar com senha falha (com a
  mensagem explicando); o botão da demonstração continua funcionando.

## 📎 Documentação afetada

- [[SessaoLocal]] (novo) · [[GuardaDeSessao]] (novo) · [[Entrar]] (novo)
- [[RepositorioLocal]] · [[DadosDeDemonstracao]] · [[Casca]] · [[glossario]]
- [[ADR-001-frontend-primeiro-com-dados-locais]]
- [[2026]] (changelog)
