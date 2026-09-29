---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 18
url: https://github.com/VictorNascimento14/ZeroSpend/pull/18
branch: feat/criar-conta
tags: [pr, frontend, sessao, autenticacao]
status: aberto
---

# PR #18 — feat(sessao): criar conta com e-mail corporativo e empresa

## 🎯 Contexto

Ordem 16 de [[2026-09-29-plano-da-v1-do-frontend]]. O fluxo 1 da
[[2026-09-29-especificacao-do-mvp]] começa com "o usuário cria conta com e-mail corporativo". Até
aqui, só a demonstração entrava.

## 🔧 Mudanças

- `validation.ts`: `AccountDraft`, `validateAccountDraft` e `PERSONAL_EMAIL_DOMAINS`.
- `auth.ts`: `createAccount(repositório, rascunho)`. Normaliza, valida, faz o hash da senha com sal
  novo, cria a empresa em real e entra nela.
- `repository.ts`: `addAccount(pessoa, empresa)` grava pessoa, empresa, vínculo de administração e
  sessão numa só vez. `ValidationError` passa a ser genérico, e `newId` e `EMAIL_TAKEN` são exportados.
- `billing.ts`: `DEFAULT_BRL_PER_USD` (5,40), a cotação inicial de uma empresa nova — a demonstração
  passa a usar a mesma constante.
- `/criar-conta` (`SignUpForm`), com os links cruzados entre "Entrar" e "Criar conta".
- 9 testes novos (96 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

- A senha segue como PBKDF2 com sal. O teste confere que o texto da senha não aparece no
  armazenamento.
- A tela de criar conta avisa que a conta fica só no navegador.
- A criação **revela** se um e-mail já tem conta ("Já existe uma conta com este e-mail."), ao
  contrário do entrar. É o padrão de cadastro, e aqui a lista de contas é local.

## 🧠 Decisões técnicas

- **E-mail corporativo por lista de provedores pessoais** (Gmail, Outlook, Hotmail, Yahoo, iCloud, BOL,
  UOL, Terra, IG, Proton…). Teto anotado no código com `ponytail:` — com backend, a conta confirma o
  domínio por e-mail.
- **Conta nova = pessoa + empresa + vínculo de administração + sessão, numa gravação só.** Não existe
  estado intermediário (pessoa sem empresa).
- **Unicidade do e-mail conferida duas vezes:** antes do hash (para a mensagem certa no campo) e na
  gravação (para duas abas não criarem a mesma conta).
- **Empresa nova começa em real, com a cotação inicial de R$ 5,40.** A cotação é da empresa e não vem
  da internet (ADR-001). A tela de KPIs vai mostrar a cotação usada, e Configurações vai permitir
  ajustá-la.
- **Validação do formulário é a do domínio** (`noValidate` no `<form>`): a mesma mensagem em português
  no navegador e nos testes, cada uma no seu campo (`aria-invalid` e `aria-describedby`).
- **Depois de criar, vai para o dashboard.** O onboarding (ordem 28) entra no meio desse fluxo.

## ⚠️ Armadilhas e aprendizados

- Texto de interface não presume gênero: "com acesso de administração", não "como administradora".
  Corrigido antes do PR, no texto e nos comentários.

## 🧪 Como testar

1. `npm test` — 96 testes (validação da conta, `createAccount` e a volta pelo `signIn`).
2. Percorrido num Chromium headless, com o console limpo:
   - "Criar conta" em `/entrar` leva a `/criar-conta`;
   - enviar vazio mostra os quatro erros, cada um no seu campo;
   - Gmail → "Use o e-mail da empresa — Gmail, Outlook e parecidos não valem.";
   - `admin@zerospend.app` → "Já existe uma conta com este e-mail.";
   - com e-mail corporativo, entra no dashboard com o avatar "PE" e a empresa nova (sem assinaturas),
     e a senha não aparece em texto no `localStorage`;
   - sair e entrar de novo com a conta criada funciona.

## 🚫 O que não foi verificado

- O dashboard de uma empresa sem assinaturas ainda não tem estado vazio (os KPIs são a ordem 18).

## 📎 Documentação afetada

- [[CriarConta]] (novo) · [[criar-conta-ate-o-dashboard]] (novo)
- [[SessaoLocal]] · [[Entrar]] · [[RegrasDeCobranca]] · [[glossario]]
- [[2026]] (changelog)
