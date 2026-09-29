---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 32
url: https://github.com/VictorNascimento14/ZeroSpend/pull/32
branch: feat/integracoes
tags: [pr, frontend, integracoes]
status: merged
---

# PR #32 — feat(integracoes): mostrar a origem das assinaturas e importar pela página

## 🎯 Contexto

Ordem 30 de [[2026-09-29-plano-da-v1-do-frontend]]. "Extratos e integrações" é uma área da sidebar que o
briefing pede, e até aqui só tinha o título. O onboarding ([[2026-09-29-pr-030-onboarding-extrato]] e
[[2026-09-29-pr-031-onboarding-email]]) é a primeira passagem. Esta página é o endereço fixo das
entradas de dados, e sem ela não havia como importar outro extrato depois de a empresa ter assinaturas.

## 🔧 Mudanças

- **Página `/integracoes`** ([[Integracoes]]):
  - **origem das assinaturas**, em três cards: extrato CSV, e-mail e cadastro manual, cada um com o
    total e quantas estão em revisão (`SourceSummary`);
  - o upload do extrato ([[StatementImport]]) e a conexão do e-mail em demonstração
    ([[EmailConnectCard]]), os mesmos componentes do onboarding. Ficam lado a lado a partir de 1400px e
    empilhados abaixo disso;
  - nova descrição: "De onde vêm as assinaturas da empresa: o extrato do cartão, o e-mail e o cadastro
    manual."
- **`Kpi`**, o card de indicador do dashboard, passa a ser exportado e recebe o tom pelo nome da cor do
  kit (`cerulean`, `raspberry`, `plum`, `success`, `warning`), e não pelo papel que tinha no dashboard.
  `raspberry` é novo.

## 🕵️ Dado sensível (LGPD e sigilo)

Nada muda: a origem é contada a partir das assinaturas já gravadas, e o upload segue lendo o arquivo só
no navegador, sem guardá-lo.

## 🧠 Decisões técnicas

- **Origem das assinaturas em vez de histórico de importações.** A nota da página previa "o histórico de
  importações". Guardar esse histórico pediria uma tabela que o modelo de dados da
  [[2026-09-29-especificacao-do-mvp]] não tem, e a [[ADR-001-frontend-primeiro-com-dados-locais]]
  (decisão 3) manda os tipos seguirem esse modelo, para a troca pelo banco ser campo a campo. A origem
  (`source`) já está em cada assinatura: contar por ela responde "de onde vêm as minhas assinaturas" sem
  gravar nada novo (sinal derivado não se grava).
- **Cancelada não conta**, como em todo total.
- **Um componente para os dois indicadores.** `Kpi` já era o card do dashboard, e a página de
  integrações usa o mesmo. O tom pelo nome da cor tira do componente o papel "gasto" ou "economia", que
  não faz sentido fora do dashboard.
- **Raspberry para o cadastro manual.** As cores de estado (success, warning) têm significado, e origem
  não é estado. O extrato fica com cerulean, o e-mail com plum e o manual com raspberry. Contraste do
  ícone: 5,37 nos dois modos (o mínimo para ícone é 3:1).
- **Lado a lado só a partir de 1400px:** medido, o extrato fica com ~700px ao lado do card do e-mail
  (26rem), e a revisão cabe. A 1280px, empilhados, cada um com 976px.

## ⚠️ Armadilhas e aprendizados

- **Foto logo depois de navegar pela sidebar mostra dois itens "ativos":** a cor dos links tem
  transição de 150 ms. O `aria-current` muda na hora, e a cor termina em até 600 ms. É a mesma
  armadilha da borda de erro do [[2026-09-29-pr-030-onboarding-extrato]]: espere a transição antes de
  fotografar ou medir cor.

## 🧪 Como testar

`npm test` (150 testes), lint, type-check e build passam.

No navegador (Chromium headless, a 1920 e 1440px no claro, 1280px no escuro e 390px, com o console
limpo):

1. Na conta de demonstração, a página mostra: extrato CSV 6 ("2 em revisão"), e-mail 2 ("Todas
   confirmadas") e cadastro manual 6.
2. Subir o extrato de exemplo pela página: "Importar 5 assinaturas". Depois, a página mostra o extrato
   CSV com 11 ("7 em revisão").
3. Extrato e e-mail lado a lado a 1920px (1176 e 416px) e a 1440px (696 e 416px); empilhados a 1280 e
   390px. Nada vaza.
4. O dashboard continua com os mesmos quatro indicadores e as mesmas cores.

## 🚫 O que não foi verificado

- Os cards de origem não levam à lista filtrada, porque a lista não tem filtro por origem.
  <A DEFINIR> se valer a pena.

## 📎 Documentação afetada

- [[Integracoes]] · [[KpiCards]] · [[StatementImport]] · [[EmailConnectCard]]
- [[2026]] (changelog)
