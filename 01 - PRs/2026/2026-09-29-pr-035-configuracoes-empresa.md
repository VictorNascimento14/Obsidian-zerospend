---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 35
url: https://github.com/VictorNascimento14/ZeroSpend/pull/35
branch: feat/configuracoes-empresa
tags: [pr, frontend, configuracoes, dados]
status: aberto
---

# PR #35 — feat(configuracoes): editar nome, moeda padrão e cotação da empresa

## 🎯 Contexto

Ordem 33 de [[2026-09-29-plano-da-v1-do-frontend]]. A especificação converte o gasto "para a moeda
padrão da empresa", e a empresa nascia em real, com cotação de R$ 5,40, sem como mudar. `/configuracoes`
só tinha o título.

## 🔧 Mudanças

- **Seção "Empresa" em `/configuracoes`** ([[Configuracoes]] e [[OrganizationSettings]]):
  - nome da empresa;
  - moeda padrão (Real ou Dólar), com a dica "Usada no gasto mensal, na economia e nas assinaturas
    novas.";
  - cotação do dólar, com a dica "Quantos reais vale um dólar…".

  "Salvar" mostra o aviso "Dados da empresa salvos.", e o erro aparece embaixo de cada campo.
- **Quem não administra** vê os campos desabilitados e "Só quem administra a empresa altera estes
  dados.".
- **Repositório:** `updateOrganization(empresa, rascunho)` valida e confere se quem está na sessão
  administra a empresa ([[RepositorioLocal]]).
- **Domínio:**
  - `validateOrganizationDraft` (nome obrigatório, moeda conhecida e cotação de R$ 0,01 a R$ 100,00);
  - `OrganizationDraft` e `BRL_PER_USD_MAX`;
  - `CURRENCY_LABELS` ("Real (R$)", "Dólar (US$)"), agora também usado pelo formulário de assinatura.
- **`Field`**, primitivo novo do shadcn, gerado sem mudança ([[Primitivos]]). As próximas seções de
  configurações usam o mesmo.
- 4 testes novos (158 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

Nome e moeda da empresa não são dado pessoal. Nada novo é gravado além dos três campos que já existiam.

## 🧠 Decisões técnicas

- **A permissão é conferida no repositório, não só na tela.** A tela desabilita o formulário, e o
  repositório recusa quem não administra. É "a validação da tela é conforto; a do repositório é a
  regra", aplicada ao papel. Hoje todo vínculo é de administração, e os membros chegam na ordem 35.
- **Teto de R$ 100,00 na cotação.** Sem ele, "540" digitado sem a vírgula multiplicaria cada
  assinatura em dólar por 100, calado. O teto é de sanidade (o real nunca passou de R$ 7), e não uma
  regra de câmbio.
- **Cotação em `<input type="number">`,** como o valor da assinatura. Num navegador em pt-BR, "5,10"
  digitado chega como `5.10` (conferido), então não há parser próprio de decimal.
- **Os valores iniciais são uma cópia tirada ao montar o formulário,** e trocar de empresa refaz o
  formulário (`key`). Ver o aprendizado abaixo.

## ⚠️ Armadilhas e aprendizados

- **Base UI avisa quando o valor inicial de um campo não controlado muda depois de montado.** Salvar
  muda a empresa, e o `defaultValue` mudaria junto. A correção foi a mesma do diálogo de edição
  ([[2026-09-29-pr-024-editar-assinatura]]). Como é a segunda vez, virou aprendizado:
  [[2026-09-29-valor-inicial-do-formulario-e-copia-tirada-ao-montar]].

## 🧪 Como testar

`npm test` (158 testes), lint, type-check e build passam.

No navegador (Chromium headless em pt-BR, 1440px claro e escuro e 390px, com o console limpo):

1. A página mostra "Exemplo Tecnologia Ltda", "Real (R$)" e 5,4.
2. Nome vazio e cotação "540" mostram as duas mensagens, com `aria-invalid`.
3. Com "Exemplo Tecnologia S.A.", "5,10" (chega como 5.10) e Dólar, "Dados da empresa salvos." aparece,
   e o seletor do topo mostra o nome novo.
4. O dashboard mostra o gasto mensal em US$ 1.439,87, "Com o dólar a R$ 5,10…".
5. Trocar para a Clínica Exemplo mostra os dados dela.
6. Com o vínculo mudado para membro, os campos ficam desabilitados, sem "Salvar", com o aviso.
7. A 390px, nada vaza.

## 🚫 O que não foi verificado

- Cotação automática (buscar o dólar do dia): depende de serviço externo e fica <A DEFINIR> com o
  backend.

## 📎 Documentação afetada

- [[Configuracoes]] · [[OrganizationSettings]] (novo) · [[RepositorioLocal]] · [[ModeloDeDominio]]
- [[Primitivos]] · [[2026-09-29-valor-inicial-do-formulario-e-copia-tirada-ao-montar]] (novo)
- [[2026]] (changelog)
