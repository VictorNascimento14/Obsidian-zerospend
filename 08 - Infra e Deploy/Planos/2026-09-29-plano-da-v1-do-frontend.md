---
tipo: plano
data: 2026-09-29
status: concluido
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

| Ordem | Branch | Entrega | PR |
|---|---|---|---|
| 1 | `chore/scaffolding` | Next.js 16 + TypeScript + Tailwind v4 + ESLint, página mínima | [[2026-09-29-pr-001-scaffolding\|#1]] |
| 2 | `chore/ci` | CI de lint, type-check e build + checagem da seção de documentação | [[2026-09-29-pr-002-ci\|#2]] |
| 3 | `ui/tema` | shadcn/ui, tokens do kit, fonte Inter e modo escuro | [[2026-09-29-pr-003-tema\|#3]] |
| 4 | `ui/primitivos-base` | Button, Badge, Card, Avatar, Separator, Skeleton | [[2026-09-29-pr-006-primitivos-base\|#6]] |
| 5 | `ui/primitivos-formulario` | Input, Label, Select, Textarea, Switch, Checkbox | [[2026-09-29-pr-007-primitivos-formulario\|#7]] |
| 6 | `ui/primitivos-sobreposicao` | Dialog, AlertDialog, DropdownMenu, Sheet, Tooltip, toasts | [[2026-09-29-pr-008-primitivos-sobreposicao\|#8]] |
| 7 | `ui/tabela` | Table | [[2026-09-29-pr-009-tabela\|#9]] |
| 8 | `feat/dominio` | tipos do domínio, categorias, formatação de moeda e data (+ testes no CI) | [[2026-09-29-pr-010-dominio\|#10]] |
| 9 | `feat/regras-de-cobranca` | valor mensal, conversão de moeda, próxima cobrança efetiva | [[2026-09-29-pr-011-regras-de-cobranca\|#11]] |
| 10 | `feat/sementes` | empresas e assinaturas de demonstração | [[2026-09-29-pr-012-sementes\|#12]] |
| 11 | `feat/repositorio-local` | repositório no `localStorage` com validação | [[2026-09-29-pr-013-repositorio-local\|#13]] |
| 12 | `feat/alertas-de-renovacao` | regra de renovação por antecedência | [[2026-09-29-pr-014-alertas-de-renovacao\|#14]] |
| 13 | `feat/redundancia` | ferramentas redundantes e economia estimada | [[2026-09-29-pr-015-redundancia\|#15]] |
| 14 | `ui/casca` | sidebar, header e as páginas da navegação | [[2026-09-29-pr-016-casca\|#16]] |
| 15 | `feat/sessao-e-entrar` | sessão local, guarda de rota, tela de entrar | [[2026-09-29-pr-017-sessao-e-entrar\|#17]] |
| 16 | `feat/criar-conta` | criar conta com e-mail corporativo e empresa | [[2026-09-29-pr-018-criar-conta\|#18]] |
| 17 | `feat/seletor-de-empresa` | trocar de empresa no header, nova empresa | [[2026-09-29-pr-019-seletor-de-empresa\|#19]] |
| 18 | `feat/dashboard-kpis` | cards de KPI | [[2026-09-29-pr-020-dashboard-kpis\|#20]] |
| 19 | `feat/tabela-de-assinaturas` | `SubscriptionsTable` no dashboard | [[2026-09-29-pr-021-tabela-de-assinaturas\|#21]] |
| 20 | `feat/painel-de-alertas` | painel de alertas e duplicidades | [[2026-09-29-pr-022-painel-de-alertas\|#22]] |
| 21 | `feat/nova-assinatura` | cadastro manual | [[2026-09-29-pr-023-nova-assinatura\|#23]] |
| 22 | `feat/editar-assinatura` | editar valor, ciclo, data, status e responsável | [[2026-09-29-pr-024-editar-assinatura\|#24]] |
| 23 | `feat/excluir-assinatura` | excluir com confirmação e desfazer | [[2026-09-29-pr-025-excluir-assinatura\|#25]] |
| 24 | `feat/revisar-deteccao` | confirmar ou descartar assinatura em revisão | [[2026-09-29-pr-026-revisar-deteccao\|#26]] |
| 25 | `feat/pagina-assinaturas` | lista completa com busca, filtros e ordenação | [[2026-09-29-pr-027-pagina-assinaturas\|#27]] |
| 26 | `feat/busca-global` | busca ⌘K | [[2026-09-29-pr-028-busca-global\|#28]] |
| 27 | `feat/leitor-de-extrato` | leitor de CSV e reconhecimento de fornecedores | [[2026-09-29-pr-029-leitor-de-extrato\|#29]] |
| 28 | `feat/onboarding-extrato` | onboarding com upload de CSV e revisão | [[2026-09-29-pr-030-onboarding-extrato\|#30]] |
| 29 | `feat/onboarding-email` | conexão Google Workspace / Microsoft 365 (demonstração) | [[2026-09-29-pr-031-onboarding-email\|#31]] |
| 30 | `feat/integracoes` | página Extratos e integrações | [[2026-09-29-pr-032-integracoes\|#32]] |
| 31 | `feat/central-de-alertas` | página de alertas com dispensar | [[2026-09-29-pr-033-central-de-alertas\|#33]] |
| 32 | `feat/notificacoes` | sino de notificações | [[2026-09-29-pr-034-notificacoes\|#34]] |
| 33 | `feat/configuracoes-empresa` | nome, moeda padrão e cotação | [[2026-09-29-pr-035-configuracoes-empresa\|#35]] |
| 34 | `feat/configuracoes-alertas` | antecedência e canais de alerta | [[2026-09-29-pr-036-configuracoes-alertas\|#36]] |
| 35 | `feat/configuracoes-membros` | membros e convites | [[2026-09-29-pr-037-configuracoes-membros\|#37]] |
| 36 | `feat/telas-de-sistema` | 404, erro e carregamento | [[2026-09-29-pr-038-telas-de-sistema\|#38]] |
| 37 | `chore/metadados` | ícone, manifesto e metadados | [[2026-09-29-pr-039-metadados\|#39]] |
| 38 | `docs/readme` | como rodar, conta de demonstração e mapa do app | [[2026-09-29-pr-040-readme\|#40]] |

## Resultado

**Concluído em 2026-09-29:** as 38 ordens viraram 40 PRs (#1 a #40), cada um com o fluxo completo e
squash and merge, um de cada vez. Dois PRs ficaram fora do plano, os dois correções que apareceram no
caminho:

- [[2026-09-29-pr-004-next-dev|#4]] (`chore/next-dev`): o `next dev` anexava regras de agente ao
  `CLAUDE.md`;
- [[2026-09-29-pr-005-titulos-do-kit|#5]] (`fix/titulos-do-kit`): os títulos do kit nos tamanhos
  padrão do Tailwind.

Por isso, da ordem 4 em diante o número do PR é a ordem + 2.

**Desvios do plano, cada um registrado na nota do PR:**

- **Ordem 30:** o "histórico de importações" virou "origem das assinaturas". O modelo da especificação
  não tem tabela de importações, e a ADR-001 (decisão 3) manda seguir o modelo
  ([[2026-09-29-pr-032-integracoes]]).
- **Ordem 31:** a regra do formato gravado mudou. Campo novo com padrão na leitura entra na mesma chave;
  só mudança que quebra a leitura antiga pede chave nova e migração (ADR-001, Atualizações;
  [[2026-09-29-pr-033-central-de-alertas]]).
- **Ordem 32:** o sino não tem "lida/não lida"; o número é o que falta tratar
  ([[2026-09-29-pr-034-notificacoes]]).
- **Ordem 29:** a conexão de e-mail simulada não importa nada, para não pôr assinatura inventada na
  empresa de ninguém ([[2026-09-29-pr-031-onboarding-email]]).

**O que fica para depois da v1** (backend, IA de extração, OAuth real, envio de alertas, PDF, Open
Graph) está nos "<A DEFINIR>" das notas de PR e na [[2026-09-29-especificacao-do-mvp]].
