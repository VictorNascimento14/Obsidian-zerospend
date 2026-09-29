---
tipo: funcionalidade
camada: frontend
area: Autenticacao
rota: /criar-conta → /dashboard
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
   ([[SessaoLocal]]).
4. A pessoa chega ao [[Dashboard]] da empresa nova, ainda sem assinaturas.

<A DEFINIR> no PR do onboarding (ordem 28): entre os passos 3 e 4 entra a importação do extrato.

## Histórico de mudanças

- [[2026-09-29-pr-018-criar-conta]] — criado: criar conta → dashboard.
