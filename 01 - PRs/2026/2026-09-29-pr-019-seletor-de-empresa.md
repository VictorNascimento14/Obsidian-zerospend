---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 19
url: https://github.com/VictorNascimento14/ZeroSpend/pull/19
branch: feat/seletor-de-empresa
tags: [pr, frontend, sessao, casca]
status: aberto
---

# PR #19 — feat(sessao): trocar de empresa no header e criar empresa nova

## 🎯 Contexto

Ordem 17 de [[2026-09-29-plano-da-v1-do-frontend]]. O briefing pede um "seletor de organização/empresa"
no header, e o público inclui o BPO financeiro, que cuida das contas de várias empresas. A partir
daqui, as telas mostram os dados da empresa da sessão.

## 🔧 Mudanças

- `repository.ts`:
  - `selectOrganization(id)` troca a empresa da sessão, mas só para uma com vínculo;
  - `addOrganization(nome)` cria a empresa em real, com a cotação inicial e vínculo de administração, e
    entra nela;
  - `CurrentSession` ganha `organizations`, as empresas da pessoa em ordem alfabética.
- `src/components/layout/organization-switcher.tsx` (novo): o seletor à esquerda do header e o diálogo
  "Nova empresa".
- `theme-toggle.tsx` e o seletor: os itens de rádio fecham o menu ao escolher (`closeOnClick`).
- 4 testes novos (97 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

Nenhum dado novo. A troca de empresa é conferida pelo vínculo, mas continua não sendo segurança: o
navegador guarda todas as empresas da máquina (ADR-001).

## 🧠 Decisões técnicas

- **O seletor mostra só as empresas com vínculo**, e o repositório recusa trocar para outra ("Você não
  tem acesso a esta empresa.") — a tela não é a única barreira.
- **Empresa nova entra na hora.** Quem cria, entra nela com acesso de administração. Não existe empresa
  sem ninguém.
- **Escolher fecha o menu.** No Base UI, o item de rádio do menu não fecha por padrão. Para trocar de
  empresa, isso é o esperado; o alternador de tema passou a se comportar igual, para o header ser
  coerente.
- **O diálogo não promete o que não existe.** Nada de "ajuste a cotação em Configurações": essa tela é
  a ordem 33. Diz só "Os valores começam em real."
- **Seletor visível no celular** (não some com a sidebar): nome longo encurta com reticências.

## ⚠️ Armadilhas e aprendizados

- Teste que escolhe um item de rádio e clica de novo no gatilho **fecha** o menu, se o item não fechar
  sozinho — foi assim que o comportamento apareceu.
- Diálogo leva ~200 ms para sair do DOM depois de fechar (a animação). Teste que confere logo em
  seguida vê o diálogo "ainda aberto".

## 🧪 Como testar

1. `npm test` — 97 testes (lista das empresas, troca com e sem vínculo, criação, nome vazio).
2. Percorrido num Chromium headless, com o console limpo:
   - com a demonstração, o header mostra "Exemplo Tecnologia Ltda";
   - o menu lista Clínica Exemplo e Exemplo Tecnologia Ltda (✓);
   - escolher a Clínica troca o header e a sessão gravada;
   - "Nova empresa" vazio → "Informe o nome da empresa.";
   - "Exemplo Filial" cria, entra, avisa "Você está em Exemplo Filial." e fecha o diálogo;
   - o tema fecha o menu ao escolher;
   - no celular, o seletor cabe no header.

## 🚫 O que não foi verificado

- As páginas ainda não mostram dados da empresa (os KPIs são a próxima ordem): a troca só aparece no
  header por enquanto.

## 📎 Documentação afetada

- [[Casca]] · [[SessaoLocal]]
- [[2026]] (changelog)
