---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 33
url: https://github.com/VictorNascimento14/ZeroSpend/pull/33
branch: feat/central-de-alertas
tags: [pr, frontend, alertas, dados]
status: merged
---

# PR #33 — feat(alertas): central de alertas com dispensar

## 🎯 Contexto

Ordem 31 de [[2026-09-29-plano-da-v1-do-frontend]]. A [[visao-de-produto]] pede "Alertas: renovação
dentro da antecedência escolhida, ferramentas redundantes e itens em revisão; dispensar o que já foi
tratado". Até aqui, `/alertas` só tinha o título, e o painel do dashboard mostrava os alertas sem ter
como tratá-los.

## 🔧 Mudanças

- **Central de alertas em `/alertas`** ([[Alertas]] e [[AlertsCenter]]), com três seções:
  - **Renovações nos próximos 7 dias** e **Ferramentas redundantes**, cada alerta com "Dispensar". O
    aviso "Alerta dispensado." traz "Desfazer";
  - **Em revisão**, com "Confirmar" e "Descartar" (as mesmas ações da tabela, agora exportadas);
  - **"N alertas dispensados"**, fechado, com "Voltar a mostrar".
- **Domínio:** `currentAlerts(assinaturas, empresa, hoje)` junta renovações e duplicidades com uma
  **chave por situação** ([[AlertasDeRenovacao]]). `AlertDismissal` é o tipo novo.
- **Repositório:**
  - `dismissals` no banco local;
  - `dismissAlert` (dispensar de novo não muda nada) e `restoreAlert`;
  - `useOrganizationData` entrega `dismissedAlertKeys`.
- **Painel do dashboard** ([[AlertsPanel]]): mostra só o que não foi dispensado e ganha "Ver todos",
  que leva à central.
- **`AlertItem` e `describeAlert`:** o texto e o visual de um alerta, que saíram do painel. Painel e
  central usam os mesmos, e o sino (ordem 32) vai usar também.
- 4 testes novos (154 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

A dispensa guarda só a empresa, a chave do alerta (tipo, ids e data da cobrança) e o dia da dispensa.
Nenhum dado pessoal.

## 🧠 Decisões técnicas

- **A chave nomeia a situação, não só a assinatura.**
  - A renovação leva a data da cobrança (`renovacao:<id>:<data>`), então dispensar a de outubro não
    cala a de novembro.
  - A duplicidade leva quem está no grupo (`redundancia:<categoria>:<ids>`), então uma ferramenta nova
    na categoria alerta de novo.
- **"Em revisão" não se dispensa, se resolve.** A cobrança conta até alguém confirmar ou descartar (o
  gasto só exclui a cancelada), então a seção traz as duas ações, e não "Dispensar".
- **A dispensa vale para a empresa, não para a pessoa.** Quem trata a renovação trata para todos.
- **Campo novo com padrão na leitura, na mesma chave.**
  - A regra antiga mandava trocar a chave a cada mudança de formato, mas trocar sem migrar ressemeia a
    demonstração por cima das contas e assinaturas de quem já usa o app.
  - `dismissals` é aditivo: o banco gravado antes dele é lido com `dismissals: []`, como uma coluna
    nova com valor padrão.
  - Chave nova e migração ficam para a mudança que quebra a leitura antiga (renomear, trocar tipo,
    remover).
  - Registrado na [[ADR-001-frontend-primeiro-com-dados-locais]] (Atualizações) e em
    [[RepositorioLocal]], com teste.
- **Dispensar tem desfazer e volta.** O aviso traz "Desfazer", e a lista de dispensados, "Voltar a
  mostrar". Ninguém perde um alerta por um clique errado.
- **As dispensas se acumulam.** São uma linha curta por alerta tratado, e anos delas cabem no
  `localStorage`. O teto está no código (`ponytail:`): com o backend, viram tabela com limpeza.

## ⚠️ Armadilhas e aprendizados

- **A chave da duplicidade ordena os ids.** O grupo vem ordenado pela economia, e a ordem muda quando
  um valor muda; sem ordenar, a mesma situação ganharia outra chave.

## 🧪 Como testar

`npm test` (154 testes), lint, type-check e build passam. Os testes novos cobrem:

- a chave muda com a data e com o grupo;
- dispensar duas vezes grava uma;
- empresa inexistente e chave vazia são recusadas;
- o banco gravado sem `dismissals` é lido sem ressemear.

No navegador (Chromium headless, 1440px claro e escuro e 390px, com o console limpo), na conta de
demonstração:

1. "Ver todos", no painel, leva a `/alertas`: 3 renovações, 2 duplicidades e 2 em revisão.
2. Dispensar "GitHub renova em 2 dias":
   - sobram 2 renovações e aparece "1 alerta dispensado";
   - "Desfazer" traz o alerta de volta, e "Voltar a mostrar" também.
3. Dispensar "Duplicidade em Design", confirmar ChatGPT Team e descartar Adobe Acrobat Pro: a seção de
   revisão mostra "Nada em revisão.".
4. Recarregar mantém tudo, e o painel do dashboard não mostra a duplicidade dispensada.
5. A Clínica Exemplo não herda as dispensas da Tecnologia.
6. A 390px, nada vaza.

## 🚫 O que não foi verificado

- Antecedência diferente de 7 dias: a configuração é a ordem 34 do plano.

## 📎 Documentação afetada

- [[Alertas]] · [[AlertsCenter]] (novo) · [[AlertsPanel]] · [[AlertasDeRenovacao]] · [[RepositorioLocal]]
- [[ModeloDeDominio]] · [[ADR-001-frontend-primeiro-com-dados-locais]] · [[glossario]]
- [[2026]] (changelog)
