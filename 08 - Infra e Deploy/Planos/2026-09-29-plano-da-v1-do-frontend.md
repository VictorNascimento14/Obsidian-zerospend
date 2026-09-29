---
tipo: plano
data: 2026-09-29
status: em-andamento
tags: [plano, frontend, v1]
autor: VictorNascimento14
---

# Plano da v1 do front-end

Pedido do dono em 2026-09-29: só front-end por enquanto, **um PR por peça**, bem dividido, cada um
publicado com o fluxo completo (nota no cofre antes do PR, CI verde, squash and merge). Um PR por vez,
mergeado antes do próximo começar — PR empilhado sobre branch de outro PR, com squash, pode mergear
numa base morta e nunca chegar à `main`.

Escopo: [[2026-09-29-especificacao-do-mvp]] (o que depende de backend fica fora). Tokens:
[[linguagem-visual]]. Decisões de base: [[ADR-001-frontend-primeiro-com-dados-locais]] e
[[ADR-002-design-system-do-figma-com-shadcn]].

| Ordem | Branch | Entrega |
|---|---|---|
| 1 | `chore/scaffolding` | Next.js 16 + TypeScript + Tailwind v4 + ESLint, página mínima |
| 2 | `chore/ci` | CI de lint, type-check e build + checagem da seção de documentação |
| 3 | `ui/tema` | shadcn/ui, tokens do kit, fonte Inter e modo escuro |
| 4 | `ui/primitivos-base` | Button, Badge, Card, Avatar, Separator, Skeleton |
| 5 | `ui/primitivos-formulario` | Input, Label, Select, Textarea, Switch, Checkbox |
| 6 | `ui/primitivos-sobreposicao` | Dialog, AlertDialog, DropdownMenu, Sheet, Tooltip, toasts |
| 7 | `ui/tabela` | Table |
| 8 | `feat/dominio` | tipos do domínio, categorias, formatação de moeda e data (+ testes no CI) |
| 9 | `feat/regras-de-cobranca` | valor mensal, conversão de moeda, próxima cobrança efetiva |
| 10 | `feat/sementes` | empresas e assinaturas de demonstração |
| 11 | `feat/repositorio-local` | repositório no `localStorage` com validação |
| 12 | `feat/alertas-de-renovacao` | regra de renovação por antecedência |
| 13 | `feat/redundancia` | ferramentas redundantes e economia estimada |
| 14 | `ui/casca` | sidebar, header e as páginas da navegação |
| 15 | `feat/sessao-e-entrar` | sessão local, guarda de rota, tela de entrar |
| 16 | `feat/criar-conta` | criar conta com e-mail corporativo e empresa |
| 17 | `feat/seletor-de-empresa` | trocar de empresa no header, nova empresa |
| 18 | `feat/dashboard-kpis` | cards de KPI |
| 19 | `feat/tabela-de-assinaturas` | `SubscriptionsTable` no dashboard |
| 20 | `feat/painel-de-alertas` | painel de alertas e duplicidades |
| 21 | `feat/nova-assinatura` | cadastro manual |
| 22 | `feat/editar-assinatura` | editar valor, ciclo, data, status e responsável |
| 23 | `feat/excluir-assinatura` | excluir com confirmação e desfazer |
| 24 | `feat/revisar-deteccao` | confirmar ou descartar assinatura em revisão |
| 25 | `feat/pagina-assinaturas` | lista completa com busca, filtros e ordenação |
| 26 | `feat/busca-global` | busca ⌘K |
| 27 | `feat/leitor-de-extrato` | leitor de CSV e reconhecimento de fornecedores |
| 28 | `feat/onboarding-extrato` | onboarding com upload de CSV e revisão |
| 29 | `feat/onboarding-email` | conexão Google Workspace / Microsoft 365 (demonstração) |
| 30 | `feat/integracoes` | página Extratos e integrações |
| 31 | `feat/central-de-alertas` | página de alertas com dispensar |
| 32 | `feat/notificacoes` | sino de notificações |
| 33 | `feat/configuracoes-empresa` | nome, moeda padrão e cotação |
| 34 | `feat/configuracoes-alertas` | antecedência e canais de alerta |
| 35 | `feat/configuracoes-membros` | membros e convites |
| 36 | `feat/telas-de-sistema` | 404, erro e carregamento |
| 37 | `chore/metadados` | ícone, manifesto e metadados |
| 38 | `docs/readme` | como rodar, conta de demonstração e mapa do app |

A numeração dos PRs no GitHub pode não bater 1:1 com a ordem — a nota de cada PR é a fonte, e o
mapa real entra aqui quando o plano fechar.
