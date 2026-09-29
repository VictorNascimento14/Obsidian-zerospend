---
tipo: indice
ultima_atualizacao: 2026-09-29
tags: [indice, produto]
---

# Visão de produto

**ZeroSpend** ajuda pequenas e médias empresas a cortar custo com software: junta num lugar só todas
as assinaturas que a empresa paga, mostra quanto isso custa por mês, aponta ferramentas que fazem a
mesma coisa e avisa antes de cada renovação — para ninguém ser cobrado por algo esquecido.

## Para quem

- **Gestor de PME** (5 a 50 funcionários) que paga software no cartão corporativo e não sabe o total.
- **BPO financeiro** que cuida das contas de várias empresas — por isso o seletor de empresa no topo.

## A promessa

Conectar o e-mail ou subir o extrato do cartão e ver, **em menos de 5 minutos**, a lista consolidada
dos gastos recorrentes, com alerta de renovação dali em diante.

## O que a v1 entrega (só front-end)

1. **Casca** — sidebar (Dashboard, Assinaturas, Extratos e integrações, Alertas, Configurações) e
   header (seletor de empresa, busca global, notificações, avatar).
2. **Dashboard** — quatro KPIs (gasto mensal, economia potencial, assinaturas ativas, renovações
   urgentes), a tabela de assinaturas e o painel de alertas e duplicidades.
3. **Assinaturas** — lista completa com busca e filtros; cadastrar, editar (valor, ciclo, data,
   status, responsável), confirmar o que foi detectado e excluir.
4. **Importação** — onboarding com upload de extrato CSV (lido de verdade no navegador) e conexão de
   e-mail Google Workspace / Microsoft 365 (simulada e rotulada como demonstração).
5. **Alertas** — renovação dentro da antecedência escolhida, ferramentas redundantes e itens em
   revisão; dispensar o que já foi tratado.
6. **Configurações** — empresa, moeda padrão e cotação, antecedência e canais de alerta, membros.
7. **Conta** — entrar e criar conta com e-mail corporativo.

## Fora da v1

Backend (Supabase), extração por IA, OAuth real, envio de alerta por e-mail/WhatsApp e cron — ver
[[2026-09-29-especificacao-do-mvp]] e [[ADR-001-frontend-primeiro-com-dados-locais]].
