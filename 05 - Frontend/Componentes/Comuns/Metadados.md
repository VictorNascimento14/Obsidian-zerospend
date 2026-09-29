---
tipo: funcionalidade
camada: frontend
area: Comuns
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, metadados]
---

# Metadados

## O que é

O que o navegador e o sistema do celular leem do app: título das abas, ícone, manifesto e cor da barra.

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/app/layout.tsx` | `metadata`: título ("%s · ZeroSpend"), descrição, `applicationName`, `formatDetection`; `viewport.themeColor` |
| `src/app/icon.svg` | o ícone (o "Z" da marca sobre o cerulean) |
| `src/app/apple-icon.tsx` | o ícone da tela inicial do iPhone (PNG de 180px, gerado no build) |
| `src/app/manifest.ts` | o manifesto (`/manifest.webmanifest`) |
| `export const metadata` de cada página | o título da página ("Dashboard", "Alertas"…) |

## Comportamento

- **Título:** "Página · ZeroSpend". Sem título próprio, a página fica só com "ZeroSpend".
- **Ícone:** `/icon.svg` em toda aba. Fixado na tela inicial do iPhone, vale o `/apple-icon`.
- **Manifesto:**
  - nome "ZeroSpend", `pt-BR`, início em `/dashboard`, `standalone`;
  - fundo `#f3f3f4` (`grey-50`) e tema `#0068e9` (`cerulean`).
- **Barra do navegador no celular:** branca no modo claro e `#292831` (`grey-800`) no escuro, a cor do
  header.
- **Números não viram telefone** no Safari do iPhone (`format-detection: telephone=no`).

## Estados (vazio, carregando, erro)

Não se aplica.

## Regras de uso

- Página nova exporta `metadata` com o `title`, e o modelo põe "· ZeroSpend".
- Cor em hexadecimal só onde CSS não chega (manifesto, `themeColor`), sempre um token do kit e com
  comentário.
- Nada de logotipo de terceiro nem imagem baixada: o ícone é desenhado em SVG.

## Histórico de mudanças

- [[2026-09-29-pr-039-metadados]] — criada: ícone, manifesto e cor do tema.
