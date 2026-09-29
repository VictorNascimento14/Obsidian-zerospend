---
tipo: indice
ultima_atualizacao: 2026-09-29
tags: [indice, frontend]
camada: frontend
---

# Front-end — mapa

Stack: Next.js 16 (App Router) · React 19 · TypeScript · Tailwind v4 · shadcn/ui (`base-nova`) ·
`lucide-react` · design system do Figma ([[ADR-002-design-system-do-figma-com-shadcn]], tokens em
[[linguagem-visual]]).

## Páginas

- [[Dashboard]]
- [[Assinaturas]]
- [[Integracoes]]
- [[Alertas]]
- [[Configuracoes]]
- [[Entrar]]
- [[CriarConta]]
- [[Onboarding]]
- [[TelasDeSistema]] — 404, erro e carregamento

## Componentes

- [[Tema]] — tokens do kit, tokens semânticos, Inter e modo escuro
- [[Primitivos]] — componentes de base do shadcn (`base-nova`) e as regras de uso
- [[Casca]] — sidebar, header, menu do celular e tema
- [[GuardaDeSessao]] — a casca só aparece com sessão
- [[KpiCards]] — os quatro indicadores do dashboard
- [[SubscriptionsTable]] — a tabela de assinaturas, com status e redundância
- [[AlertsPanel]] — renovações e duplicidades no dashboard
- [[SubscriptionForm]] — cadastro manual e os campos de assinatura
- [[StatementImport]] — subir o extrato do cartão, revisar e importar
- [[EmailConnectCard]] — a conexão com o e-mail da empresa (demonstração)
- [[AlertsCenter]] — a central de alertas: dispensar, confirmar e descartar
- [[Notificacoes]] — o sino do header, com o que falta tratar
- [[OrganizationSettings]] — nome, moeda padrão e cotação da empresa
- [[AlertSettings]] — antecedência e canais de alerta da empresa
- [[MembersSettings]] — membros, convites e remoção de acesso

## Fluxos

- [[criar-conta-ate-o-dashboard]] — de criar a conta até o painel

## Domínio e dados

- [[ModeloDeDominio]] — tipos, categorias, datas sem hora e formatação
- [[RegrasDeCobranca]] — valor por mês, conversão, gasto mensal e próxima cobrança
- [[DadosDeDemonstracao]] — as empresas e assinaturas da conta de demonstração
- [[RepositorioLocal]] — leitura e escrita no `localStorage`, com validação
- [[AlertasDeRenovacao]] — quais assinaturas renovam dentro da antecedência
- [[Redundancia]] — ferramentas redundantes e economia potencial
- [[SessaoLocal]] — pessoa, empresas, sessão e senha
- [[LeitorDeExtrato]] — ler o CSV do cartão e reconhecer os fornecedores
- [[Metadados]] — título das abas, ícone, manifesto e cor da barra
