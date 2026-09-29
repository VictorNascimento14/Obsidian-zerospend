---
tipo: adr
numero: 1
data: 2026-09-29
status: aceito
tags: [adr, arquitetura, dados]
autor: VictorNascimento14
---

# ADR-001 — Front-end primeiro, com dados locais atrás de uma fronteira

## Contexto

O dono pediu **apenas o front-end** por enquanto. A especificação do MVP
([[2026-09-29-especificacao-do-mvp]]) prevê Supabase, IA de extração, OAuth de e-mail e alertas por
e-mail — nada disso existe ainda. Mesmo assim as telas precisam de estado de verdade: criar conta,
trocar de empresa, importar um extrato, editar e excluir assinatura, dispensar alerta.

## Decisão

1. Toda leitura e escrita de dado passa por **`src/lib/data/`**. Componente nenhum toca
   `localStorage` direto; as telas assinam o repositório por hooks (`useSyncExternalStore`).
2. A persistência da v1 é o **`localStorage`** do navegador, numa chave versionada.
3. Os **tipos seguem o modelo de dados da especificação** (`organizations`, `users`,
   `subscriptions`, com `vendor_name`, `billing_cycle`, `next_billing_date`, `status`, `source`), em
   camelCase — para a troca pelo banco ser campo a campo.
4. As **regras de negócio são funções puras** em `src/lib/domain/` (cobrança, alertas, redundância,
   economia), testadas com Vitest. Elas não sabem de onde o dado vem, e sobrevivem à troca.
5. A "autenticação" da v1 é local: criar conta guarda o usuário no repositório, entrar confere e-mail e
   senha lá. **Não é segurança** — é o formato do fluxo, para a tela existir.
6. O que depende de backend e aparece na tela é **rotulado como demonstração**: a conexão com Google
   Workspace / Microsoft 365 e a leitura de PDF. O CSV, ao contrário, é lido de verdade no navegador.

## Consequências

- ✅ Trocar por Supabase mexe em `src/lib/data/` (e numa sessão com cookie), não nas telas.
- ✅ As regras de domínio viram a especificação executável do backend: o cron de alertas e a detecção de
  duplicidade do servidor podem reusar os mesmos casos de teste.
- ⚠️ Senha no `localStorage` é aceitável **só** porque não existe servidor; a tela de criar conta avisa
  isso. Com backend, a senha sai do cliente por completo.
- ⚠️ A guarda de rota é client-side: protege o fluxo, não o dado. Com Supabase ela vira `proxy.ts` +
  cookie de sessão.
- ⚠️ Dado de um navegador não aparece em outro.

## Implementado em

- [[2026-09-29-pr-010-dominio]] — tipos do domínio, categorias, datas sem hora, formatação e Vitest no CI.
- [[2026-09-29-pr-011-regras-de-cobranca]] — valor mensal, conversão de moeda e próxima cobrança efetiva.
- [[2026-09-29-pr-013-repositorio-local]] — o repositório no `localStorage` (chave versionada, validação na escrita, `useSyncExternalStore`).
- [[2026-09-29-pr-014-alertas-de-renovacao]] — regra de alertas de renovação.
- [[2026-09-29-pr-015-redundancia]] — ferramentas redundantes e economia potencial.

TODO: acrescentar os PRs das regras, do repositório local e da sessão conforme forem mergeados.
