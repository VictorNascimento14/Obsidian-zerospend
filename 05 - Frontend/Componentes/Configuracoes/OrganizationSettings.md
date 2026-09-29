---
tipo: funcionalidade
camada: frontend
area: Configuracoes
rota: /configuracoes
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, configuracoes]
---

# OrganizationSettings

## O que é

A seção "Empresa" das [[Configuracoes]]: nome, moeda padrão e cotação do dólar da empresa atual.

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/components/settings/organization-settings.tsx` | `OrganizationSettings` e o formulário |
| `src/lib/domain/validation.ts` | `validateOrganizationDraft`, `OrganizationDraft`, `BRL_PER_USD_MAX` |
| `src/lib/data/repository.ts` | `updateOrganization` ([[RepositorioLocal]]) |

## Comportamento

- **Campos:**
  - **Nome da empresa**, que aparece no seletor do topo;
  - **Moeda padrão** (Real ou Dólar): "Usada no gasto mensal, na economia e nas assinaturas novas.";
  - **Cotação do dólar**: "Quantos reais vale um dólar. Converte as assinaturas em outra moeda para os
    totais."
- **Salvar** grava, avisa "Dados da empresa salvos." e o resto do app muda na hora: o seletor, os
  totais do [[Dashboard]] ([[RegrasDeCobranca]]) e a moeda inicial da "Nova assinatura".
- **Trocar de empresa** refaz o formulário com os dados da nova.
- **Quem não administra** vê os campos desabilitados, sem "Salvar", e o aviso "Só quem administra a
  empresa altera estes dados.".

## Estados (vazio, carregando, erro)

- **Erro,** embaixo de cada campo (`FieldError`, com `aria-invalid` e `aria-describedby`):
  - "Informe o nome da empresa.";
  - "Escolha a moeda padrão.";
  - "Informe quantos reais vale um dólar, entre R$ 0,01 e R$ 100,00."
- **Carregando:** o esqueleto da [[GuardaDeSessao]].

## Regras de uso

- O repositório confere o papel. A tela desabilitada é só conforto.
- Os valores iniciais são uma cópia tirada ao montar
  ([[2026-09-29-valor-inicial-do-formulario-e-copia-tirada-ao-montar]]).
- O teto da cotação (R$ 100,00) é de sanidade: pega o "540" digitado sem a vírgula.

## Histórico de mudanças

- [[2026-09-29-pr-035-configuracoes-empresa]] — criado.
