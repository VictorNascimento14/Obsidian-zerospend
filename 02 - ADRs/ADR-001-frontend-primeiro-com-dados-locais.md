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

## Atualizações

- **2026-09-29, [[2026-09-29-pr-033-central-de-alertas]]:** a chave versionada da decisão 2 muda só
  quando o formato **quebra a leitura antiga** (campo renomeado, tipo trocado, estrutura removida), e
  nesse caso com migração. **Campo novo, com padrão na leitura, entra na mesma chave**, como coluna nova
  com valor padrão no banco. Trocar a chave sem migrar ressemearia a demonstração por cima das contas e
  assinaturas de quem já usa o app. O primeiro caso foi `dismissals` (as dispensas de alerta).

## Implementado em

- [[2026-09-29-pr-010-dominio]] — tipos do domínio, categorias, datas sem hora, formatação e Vitest no CI.
- [[2026-09-29-pr-011-regras-de-cobranca]] — valor mensal, conversão de moeda e próxima cobrança efetiva.
- [[2026-09-29-pr-013-repositorio-local]] — o repositório no `localStorage` (chave versionada, validação na escrita, `useSyncExternalStore`).
- [[2026-09-29-pr-014-alertas-de-renovacao]] — regra de alertas de renovação.
- [[2026-09-29-pr-015-redundancia]] — ferramentas redundantes e economia potencial.
- [[2026-09-29-pr-017-sessao-e-entrar]] — sessão local e guarda de rota. A senha fica no navegador como
  PBKDF2 com sal (não em texto) — mais cuidado que o mínimo desta ADR, sem virar segurança.

- [[2026-09-29-pr-012-sementes]] — os dados de demonstração, semeados no navegador vazio.
- [[2026-09-29-pr-018-criar-conta]] — conta local com e-mail corporativo.
- [[2026-09-29-pr-019-seletor-de-empresa]] — uma pessoa em várias empresas (`memberships`).
- [[2026-09-29-pr-023-nova-assinatura]], [[2026-09-29-pr-024-editar-assinatura]],
  [[2026-09-29-pr-025-excluir-assinatura]] e [[2026-09-29-pr-026-revisar-deteccao]] — escrita pelo
  repositório, com a validação da regra.
- [[2026-09-29-pr-029-leitor-de-extrato]] e [[2026-09-29-pr-030-onboarding-extrato]] — o CSV lido de
  verdade no navegador; o PDF rotulado como demonstração (decisão 6).
- [[2026-09-29-pr-031-onboarding-email]] — a conexão de e-mail simulada e rotulada como demonstração
  (decisão 6).
- [[2026-09-29-pr-032-integracoes]] — a origem das assinaturas derivada, sem tabela fora do modelo
  (decisão 3).
- [[2026-09-29-pr-033-central-de-alertas]] — as dispensas de alerta, primeiro campo aditivo com padrão
  na leitura (Atualizações).
- [[2026-09-29-pr-035-configuracoes-empresa]], [[2026-09-29-pr-036-configuracoes-alertas]] e
  [[2026-09-29-pr-037-configuracoes-membros]] — a regra de papel conferida no repositório; preferências e
  convites como campos aditivos.
- [[2026-09-29-pr-038-telas-de-sistema]] — a tela de erro do armazenamento bloqueado.
