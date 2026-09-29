---
tipo: funcionalidade
camada: frontend
area: Configuracoes
rota: /configuracoes
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, configuracoes, sessao]
---

# MembersSettings

## O que é

A seção "Membros" das [[Configuracoes]]: quem tem acesso à empresa atual, os convites pendentes e o
convite novo.

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/components/settings/members-settings.tsx` | `MembersSettings`, o formulário de convite e a remoção com confirmação |
| `src/lib/data/store.ts` | `useMembers()`: membros (id, nome, e-mail, papel) e convites da empresa da sessão |
| `src/lib/data/repository.ts` | `inviteMember`, `revokeInvitation`, `removeMember`; `addAccount` aceita convites ([[RepositorioLocal]]) |
| `src/lib/domain/validation.ts` | `validateInviteDraft`, com a regra de e-mail corporativo da conta |

## Comportamento

- **Lista de membros**, por nome: iniciais, nome, e-mail, o papel ("Administração" ou "Membro") e "Você"
  na própria linha.
- **Convites pendentes** (quando há): e-mail, papel, "convite de DD/MM/AAAA" e "Cancelar convite".
- **Convidar** (e-mail e papel, "Membro" por padrão):
  - quem já tem conta neste navegador entra na hora: "Pessoa Exemplo já tinha conta neste navegador e
    agora faz parte da empresa.";
  - quem não tem fica pendente: "Convite criado para pessoa@exemplo.com.". Criar a conta com esse
    e-mail aceita o convite ([[criar-conta-ate-o-dashboard]]).

  O aviso fixo diz: "Nesta versão o convite não é enviado por e-mail. Quem criar conta com este e-mail,
  neste navegador, entra na empresa; quem já tem conta aqui entra na hora."
- **Remover:** "Remover o acesso de …?", com "A conta continua existindo, mas deixa de ver esta empresa.
  Para voltar, é preciso um convite novo.". Depois, o aviso "Acesso de … removido.".
- **Quem não administra** vê a lista e os convites, sem convidar, cancelar ou remover: "Só quem
  administra a empresa convida e remove pessoas.".

## Estados (vazio, carregando, erro)

- **Erros do convite**, embaixo do campo:
  - "Informe um e-mail válido.";
  - "Use o e-mail da empresa — Gmail, Outlook e parecidos não valem.";
  - "Esta pessoa já faz parte da empresa.";
  - "Já existe um convite para este e-mail.";
  - "Escolha o papel."
- **Carregando:** o esqueleto da [[GuardaDeSessao]].

## Regras de uso

- Ninguém tira o próprio acesso (o botão não aparece, e o repositório recusa), então a empresa nunca
  fica sem quem a administre.
- A tela não diz "enviado" enquanto nada for enviado.
- O hash da senha não sai do repositório: a tela recebe só id, nome e e-mail.

## Histórico de mudanças

- [[2026-09-29-pr-037-configuracoes-membros]] — criado.
