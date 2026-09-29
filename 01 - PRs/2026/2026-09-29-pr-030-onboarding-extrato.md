---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 30
url: https://github.com/VictorNascimento14/ZeroSpend/pull/30
branch: feat/onboarding-extrato
tags: [pr, frontend, onboarding, importacao]
status: merged
---

# PR #30 — feat(onboarding): subir o extrato do cartão e revisar o que foi reconhecido

## 🎯 Contexto

Ordem 28 de [[2026-09-29-plano-da-v1-do-frontend]]. No fluxo 1 da [[2026-09-29-especificacao-do-mvp]],
depois de criar a conta a empresa traz os dados, e a entrada A é o upload do extrato do cartão (CSV). O
briefing pede um onboarding com arrastar e soltar para o extrato (CSV ou PDF). O leitor veio no
[[2026-09-29-pr-029-leitor-de-extrato]], e este PR traz a tela. A conexão do e-mail (entrada B) é a
ordem 29.

## 🔧 Mudanças

- **Tela `/onboarding`** ("Traga suas assinaturas"), no grupo `(app)`: [[Onboarding]] e
  [[StatementImport]].
  - **Área de upload** (o card do kit):
    - é tracejada e aceita arrastar e soltar;
    - é o rótulo do campo de arquivo: clicar em qualquer ponto dela, ou Tab e Enter, abre o seletor.
  - **Revisão:**
    - as assinaturas encontradas vêm com caixa de marcar; as já cadastradas começam desmarcadas, com a
      etiqueta "Já cadastrada";
    - as compras que não parecem software e as linhas ilegíveis ficam em `<details>`;
    - os créditos aparecem só contados.
  - **"Importar N assinaturas"** grava as marcadas numa operação só, em revisão, e leva ao dashboard.
  - **PDF:** aviso com a etiqueta "Demonstração", porque a leitura depende do servidor.
  - **Extrato de exemplo** (`public/exemplo-extrato.csv`), com 42 lançamentos fictícios, para baixar
    e testar.
- **Leitura do arquivo:**
  - `src/lib/import/statement-file.ts` (novo): `readStatementFile(arquivo)` confere o tipo e o tamanho
    (2 MB), decodifica e lê, com uma mensagem para cada recusa;
  - `decodeStatement` em `csv.ts`: UTF-8 estrito e, se não for, Windows-1252.
- **Repositório:** `addSubscriptions(empresa, rascunhos)` grava todas ou nenhuma, e `addSubscription`
  passa a delegar para ela.
- **Onde o fluxo muda:**
  - criar conta leva ao onboarding, e não mais ao dashboard;
  - o estado vazio do dashboard e da lista (`NoSubscriptionsYet`) ganha o link "Importar do extrato".
- 5 testes novos (148 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

O extrato é lido no navegador e não sai dele. **Nada do arquivo é guardado**: nem as compras que não
são software, nem as linhas ilegíveis, nem o nome do arquivo. Só as assinaturas marcadas viram
registro. Isso foi conferido no navegador: depois da importação, o `localStorage` não tem nenhuma
descrição do extrato.

O extrato de exemplo é fictício: os comércios têm "EXEMPLO" no nome ("PADARIA EXEMPLO", "POSTO
EXEMPLO"…), e os softwares aparecem pelo nome público do catálogo, com identificadores inventados.

## 🧠 Decisões técnicas

- **A tela fica no grupo `(app)`, com a casca.** Quem acabou de criar a conta vê a empresa no seletor e
  pode sair para o dashboard pelo "Ir para o dashboard". A guarda de sessão vale sem código novo.
- **Windows-1252 quando o arquivo não é UTF-8.** Banco brasileiro costuma exportar em Windows-1252, e
  lido como UTF-8, "Descrição" vira "Descri��o" e o cabeçalho não é achado. O `TextDecoder` com
  `fatal: true` recusa o que não é UTF-8, e aí a leitura passa para Windows-1252.
- **Já cadastrada começa desmarcada** (`isAlreadyTracked`), mas pode ser marcada: a empresa pode ter
  duas contas do mesmo serviço.
- **A importação é tudo ou nada** (`addSubscriptions`). Todas são validadas antes da gravação, então um
  rascunho inválido não deixa metade importada.
- **A tela só promete o que o código faz.**
  - "O arquivo não é guardado" é verdade por construção: só o estado do componente vê o arquivo.
  - O PDF não finge leitura: fica rotulado como demonstração, como manda a
    [[ADR-001-frontend-primeiro-com-dados-locais]].
- **Limite de 2 MB.** Um extrato de 500 linhas tem uns 40 KB, então o limite barra o arquivo trocado
  por engano sem atrapalhar o uso real.
- **Na tela de criar conta, o redirecionamento de quem já tem sessão espera o envio terminar.** A
  sessão nasce no meio do `createAccount`. Sem essa espera, o efeito que manda para o dashboard
  disputaria o destino com o `router.replace("/onboarding")`.
- **O botão contornado sobre o fundo da página ganha `bg-card`.** O `outline` do tema usa o fundo da
  página e a borda `grey-100`, e sobre o `grey-50` o botão sumia. O ajuste é por `className`, sem mexer
  no primitivo.

## ⚠️ Armadilhas e aprendizados

- **A borda de erro da área muda com transição de 150 ms** (`transition-colors`): uma foto tirada logo
  depois do erro mostra a cor antiga. Espere a transição antes de medir.
- **A sombra do anel de foco é uma lista**, e a do anel (3px) é a quarta. Cortar o texto do
  `box-shadow` para medir engana.
- **Rótulo nativo em volta do `Checkbox` do Base UI funciona:** clicar no texto, clicar na caixa e
  apertar Espaço marcam uma vez só, sem clique duplo.

## 🧪 Como testar

`npm test` (148 testes), lint, type-check e build passam.

No navegador (Chromium headless, a 390, 1280 e 1440px, nos modos claro e escuro, com o console limpo):

1. Criar conta leva a `/onboarding`. "Ir para o dashboard" mostra o vazio com "Importar do extrato", que
   volta ao onboarding.
2. Subir o `exemplo-extrato.csv`:
   - aparecem "10 assinaturas encontradas", "42 lançamentos lidos" e o botão "Importar 10
     assinaturas";
   - 11 compras que não parecem software;
   - "4 créditos (pagamento da fatura, estorno) ficaram de fora".
3. Desmarcar pelo nome, pela caixa ou pelo Espaço: o botão acompanha a contagem.
4. Importar 9: o dashboard mostra 9 linhas "Em revisão" e o aviso "9 assinaturas importadas…"; nada do
   extrato fica no `localStorage`.
5. Na conta de demonstração:
   - 5 aparecem como "Já cadastrada" (Slack, Zoom, Google Workspace, Figma e ChatGPT Team);
   - o botão diz "Importar 5 assinaturas".
6. Outros arquivos:
   - o Windows-1252 é lido;
   - o PDF mostra o aviso de demonstração;
   - colunas erradas e `.xlsx` mostram a mensagem de erro, com `aria-invalid`.
7. Arrastar e soltar: a borda fica primária durante o arraste, e soltar lê o arquivo.
8. A 390px, nada vaza na horizontal.

## 🚫 O que não foi verificado

- Leitor de tela real no campo de arquivo. O nome acessível vem de `aria-labelledby`, e a dica, de
  `aria-describedby`.
- Arrastar de verdade com o mouse: o teste dispara `dragover` e `drop` com um `DataTransfer`.
- **Reimportar o mesmo extrato depois de descartar uma detecção:** ela aparece de novo, porque o
  descarte não deixa memória. <A DEFINIR> se o backend guardar um histórico de importações.
- **"RD Station" (catálogo) e "RD Station Marketing" (demonstração)** não contam como a mesma
  assinatura: o `isAlreadyTracked` compara o nome inteiro.

## 📎 Documentação afetada

- [[Onboarding]] (nova) · [[StatementImport]] (novo) · [[LeitorDeExtrato]] · [[RepositorioLocal]]
- [[SubscriptionsTable]] · [[Assinaturas]] · [[CriarConta]] · [[criar-conta-ate-o-dashboard]]
- [[Dashboard]]: além do estado vazio, a nota corrige o ponto de quebra. Desde o
  [[2026-09-29-pr-024-editar-assinatura]] ele é 1700px, e a nota ainda dizia 1600px.
- [[2026]] (changelog)
