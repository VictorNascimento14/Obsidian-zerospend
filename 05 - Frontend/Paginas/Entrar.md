---
tipo: funcionalidade
camada: frontend
area: Autenticacao
rota: /entrar
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, pagina, autenticacao]
---

# Entrar

## O que é

A tela de entrada do ZeroSpend, fora da [[Casca]], centrada sobre o fundo.

## Onde está no código

`src/app/(auth)/entrar/page.tsx` (lê o `?para=`) e `src/components/auth/sign-in-form.tsx`
(`SignInForm`); o layout das telas de conta é `src/app/(auth)/layout.tsx`.

## Comportamento

- **Formulário** de e-mail e senha: "Entrar" confere pela [[SessaoLocal]] e volta para o `?para=` (ou o
  dashboard). Erro aparece embaixo, com `role="alert"`: "E-mail ou senha incorretos."
- **"Entrar na conta de demonstração":** abre a sessão da demonstração direto; logo abaixo, as
  credenciais (`admin@zerospend.app`, senha "demonstracao").
- **Aviso:** "Nesta versão, a conta fica guardada só neste navegador. Não use uma senha que você usa em
  outro lugar."
- **Quem já tem sessão** é levado direto para o destino.

## Estados (vazio, carregando, erro)

- **Enviando:** o botão vira "Entrando…" e fica desabilitado.
- **Erro:** a mensagem embaixo do formulário.

## Histórico de mudanças

- [[2026-09-29-pr-017-sessao-e-entrar]] — criada.
