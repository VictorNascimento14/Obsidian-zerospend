---
tipo: funcionalidade
camada: frontend
area: Shell
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, casca]
---

# Casca

## O que é

A moldura das áreas do app: a sidebar com a navegação, o header e o espaço do conteúdo. No celular, a
sidebar vira uma folha lateral aberta pelo header.

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/app/(app)/layout.tsx` | a casca: `AppSidebar` + `AppHeader` + `<main>` |
| `src/components/layout/navigation.ts` | `NAV_ITEMS`: rota, rótulo e ícone de cada área |
| `src/components/layout/nav-links.tsx` | `NavLinks`: os links, com a área atual marcada (`aria-current`) |
| `src/components/layout/app-sidebar.tsx` | `AppSidebar`: marca + navegação, só no desktop (`md:`) |
| `src/components/layout/app-header.tsx` | `AppHeader`: botão do menu do celular (folha lateral) e alternador de tema |
| `src/components/layout/organization-switcher.tsx` | `OrganizationSwitcher`: a empresa atual e a troca; o diálogo "Nova empresa" |
| `src/components/layout/user-menu.tsx` | `UserMenu`: avatar com as iniciais; nome, e-mail e "Sair" |
| `src/components/layout/theme-toggle.tsx` | `ThemeToggle`: claro, escuro ou sistema |
| `src/components/layout/brand.tsx` | `Brand`: monograma "Z" na cor primária + "ZeroSpend" |
| `src/components/layout/page-header.tsx` | `PageHeader`: título (H4 do kit, `text-2xl`), descrição e as ações da página (`actions`) |

## Comportamento

- **Áreas**, na ordem do briefing: Dashboard (`/dashboard`), Assinaturas (`/assinaturas`), Extratos e
  integrações (`/integracoes`), Alertas (`/alertas`), Configurações (`/configuracoes`). A raiz `/`
  redireciona para o Dashboard.
- **Área atual:** `cerulean-tint-50` com texto `cerulean-shade-100` (escuro: `cerulean-shade-300` com
  `cerulean-tint-200`). As outras em `muted-foreground`, com hover em `accent`.
- **Celular (< `md`):** a sidebar some; o header mostra "Abrir o menu", que abre a mesma navegação numa
  folha à esquerda. Escolher uma área fecha a folha.
- **Tema:** menu com Claro, Escuro e Sistema, que fecha ao escolher; o ícone (sol/lua) troca pelo CSS.
- **Empresa:** à esquerda do header (também no celular), o nome da empresa atual abre a lista das
  empresas da pessoa (a atual marcada) e "Nova empresa". Escolher troca a empresa da sessão e fecha o
  menu.
- **Conta:** o avatar (iniciais) abre nome, e-mail e "Sair". A casca só aparece com sessão
  ([[GuardaDeSessao]]).
- **Título da aba:** "Página · ZeroSpend" (modelo no layout raiz).
- **Superfícies:** sidebar e header em `card` (brancos no claro); o conteúdo sobre o `background`.

## Estados (vazio, carregando, erro)

A casca não tem dado próprio: renderiza no servidor, e cada página cuida do seu estado.

## Regras de uso

- Área nova entra em `NAV_ITEMS` — a sidebar e o menu do celular leem a mesma lista.
- Peça nova do header só entra funcionando (seletor de empresa, busca, sino, avatar).
- Toda página começa por `PageHeader`.

## Histórico de mudanças

- [[2026-09-29-pr-016-casca]] — criada: sidebar, header com menu do celular e tema, cinco áreas.
- [[2026-09-29-pr-017-sessao-e-entrar]] — avatar com "Sair"; a casca passa a exigir sessão.
- [[2026-09-29-pr-023-nova-assinatura]] — `PageHeader` com ações.
- [[2026-09-29-pr-019-seletor-de-empresa]] — seletor de empresa e "Nova empresa"; menus de rádio fecham ao escolher.
