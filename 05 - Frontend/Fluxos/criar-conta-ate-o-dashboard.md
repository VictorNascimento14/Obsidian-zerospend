---
tipo: funcionalidade
camada: frontend
area: Autenticacao
rota: /criar-conta → /onboarding → /dashboard
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, fluxo, autenticacao]
---

# Criar conta até o dashboard

## O que é

O caminho de uma empresa nova, da primeira tela até o painel — o fluxo 1 da
[[2026-09-29-especificacao-do-mvp]].

## Comportamento

1. Quem não tem sessão e abre qualquer área cai em [[Entrar]] ([[GuardaDeSessao]]).
2. "Não tem conta? Criar conta" leva a [[CriarConta]].
3. A conta nova cria a pessoa, a empresa e o vínculo de administração, e já abre a sessão na empresa
   ([[SessaoLocal]]). Se alguém convidou esse e-mail, a pessoa entra também nessa empresa, que aparece
   no seletor ([[MembersSettings]]).
4. A pessoa chega ao [[Onboarding]] ("Traga suas assinaturas"). Ali sobe o extrato do cartão, confere
   o que foi reconhecido e importa ([[StatementImport]]), ou vai direto para o dashboard.
5. No [[Dashboard]], as assinaturas importadas aparecem em revisão, para confirmar ou descartar. Se a
   pessoa pulou a importação, o estado vazio aponta de volta para o onboarding.

## Histórico de mudanças

- [[2026-09-29-pr-018-criar-conta]] — criado: criar conta → dashboard.
- [[2026-09-29-pr-030-onboarding-extrato]] — o onboarding entra entre a conta e o dashboard.
- [[2026-09-29-pr-037-configuracoes-membros]] — convites pendentes aceitos ao criar a conta.
