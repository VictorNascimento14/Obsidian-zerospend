---
tipo: funcionalidade
camada: frontend
area: Onboarding
rota: /onboarding
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, onboarding, importacao]
---

# StatementImport

## O que é

O caminho do extrato do cartão até as assinaturas: a pessoa sobe o CSV, confere o que o
[[LeitorDeExtrato]] reconheceu e importa. Fica no [[Onboarding]].

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/components/onboarding/statement-import.tsx` | `StatementImport`: as duas etapas (escolher e revisar) e a importação |
| `src/components/onboarding/statement-dropzone.tsx` | `StatementDropzone`: a área de upload |
| `src/components/onboarding/detected-list.tsx` | `DetectedList`: as assinaturas encontradas, com a caixa de marcar |
| `src/lib/import/statement-file.ts` | `readStatementFile(arquivo)`: do arquivo até a leitura ([[LeitorDeExtrato]]) |
| `public/exemplo-extrato.csv` | o extrato de exemplo, fictício |

## Comportamento

1. **Escolher:**
   - a área tracejada (o card de upload do kit) aceita arrastar e soltar. Clicar nela, ou Tab e Enter,
     abre o seletor do sistema;
   - durante o arraste, a borda fica primária e o fundo, `cerulean-tint-50`;
   - aceita `.csv` e `.pdf` de até 2 MB;
   - o link "Baixe um extrato de exemplo" traz um arquivo com 42 lançamentos fictícios e 10 softwares
     do catálogo.
2. **Ler** (`readStatementFile`):
   - o PDF vai para o aviso de demonstração;
   - voltam com uma mensagem: o arquivo que não é CSV, o maior que 2 MB, o CSV sem as colunas e o que
     só tem cabeçalho;
   - o CSV é decodificado (UTF-8 e, se não for, Windows-1252) e lido.
3. **Revisar:**
   - o título diz "N assinaturas encontradas", e a descrição, "N lançamentos lidos em <arquivo>";
   - cada assinatura mostra monograma, categoria, valor da última cobrança, número de cobranças e a
     data da última;
   - todas vêm marcadas, menos as que já estão cadastradas na empresa, que trazem a etiqueta "Já
     cadastrada";
   - "N compras que não parecem software" e "N linhas que não deu para ler" ficam em `<details>`
     fechados;
   - uma linha avisa: "N créditos (pagamento da fatura, estorno) ficaram de fora".
4. **Importar N assinaturas:**
   - `addSubscriptions` grava as marcadas de uma vez, em revisão, mensais, em real e com a próxima
     cobrança um mês depois da última ([[RepositorioLocal]]);
   - aparece o aviso "N assinaturas importadas. Confira cada uma no dashboard.";
   - a pessoa vai para o [[Dashboard]], onde confirma ou descarta cada uma ([[SubscriptionsTable]]).
5. **"Escolher outro arquivo"** volta à primeira etapa.

## Estados (vazio, carregando, erro)

- **Erro:** aparece abaixo da área, com `role="alert"`, `aria-invalid` no campo e a borda em
  `destructive`. As mensagens:
  - "Esse tipo de arquivo não é lido. Envie o extrato em CSV.";
  - "O arquivo passa de 2 MB, bem mais que um extrato de cartão. Confira se é o arquivo certo.";
  - "O arquivo está vazio.";
  - "Não achei as colunas Data, Descrição e Valor no cabeçalho do arquivo.";
  - "O arquivo só tem o cabeçalho, sem nenhum lançamento."
- **PDF** (`role="status"`, com a etiqueta "Demonstração"): "Ler a fatura em PDF depende do servidor,
  que chega depois desta versão. Por enquanto, exporte o extrato em CSV no site do banco e envie aqui."
- **Nada reconhecido:** o título diz "Nenhuma assinatura encontrada", o botão de importar não aparece,
  e a tela sugere cadastrar pela "Nova assinatura".

## Regras de uso

- **Nada do arquivo é guardado**, nem as compras que não são software, nem o nome do arquivo. A tela
  promete isso ("não é guardado"), então nada de `localStorage` para retomar a revisão depois.
- O PDF só deixa de ser demonstração quando houver servidor que o leia
  ([[ADR-001-frontend-primeiro-com-dados-locais]]).
- O extrato de exemplo continua fictício: fornecedores do catálogo com identificador inventado e
  comércios com "EXEMPLO" no nome.

## Histórico de mudanças

- [[2026-09-29-pr-030-onboarding-extrato]] — criado.
