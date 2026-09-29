---
tipo: funcionalidade
camada: frontend
area: Autenticacao
rota: /criar-conta
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, pagina, autenticacao]
---

# Criar conta

## O que é

A tela onde uma empresa começa no ZeroSpend: a pessoa cria a conta com o e-mail da empresa e já
entra nela, com acesso de administração.

## Onde está no código

`src/app/(auth)/criar-conta/page.tsx` e `src/components/auth/sign-up-form.tsx` (`SignUpForm`). A regra
é `createAccount` ([[SessaoLocal]]).

## Comportamento

- **Campos:** seu nome, e-mail da empresa, senha (dica: "Pelo menos 8 caracteres.") e nome da
  empresa.
- **Erros**, cada um embaixo do seu campo (`aria-invalid` e `aria-describedby`):
  - "Informe seu nome.";
  - "Informe um e-mail válido.";
  - "Use o e-mail da empresa — Gmail, Outlook e parecidos não valem.";
  - "Já existe uma conta com este e-mail.";
  - "A senha precisa de pelo menos 8 caracteres.";
  - "Informe o nome da empresa."
- **Deu certo:** a pessoa entra na empresa nova (em real, cotação inicial R$ 5,40) e vai para o
  [[Onboarding]], para trazer as assinaturas do extrato. Se havia convite pendente para o e-mail, ela
  entra também na empresa que convidou ([[MembersSettings]]).
- **Links:** "Já tem conta? Entrar" aqui, e "Não tem conta? Criar conta" em [[Entrar]].
- **Aviso:** a conta fica guardada só neste navegador.

## Estados (vazio, carregando, erro)

- **Enviando:** o botão vira "Criando…" e fica desabilitado.
- **Erro:** nos campos, ou uma mensagem geral embaixo do formulário (por exemplo, sem https).

## Histórico de mudanças

- [[2026-09-29-pr-018-criar-conta]] — criada.
- [[2026-09-29-pr-030-onboarding-extrato]] — depois de criar a conta, vai para o [[Onboarding]].
- [[2026-09-29-pr-037-configuracoes-membros]] — aceita os convites pendentes do e-mail.
