---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 37
url: https://github.com/VictorNascimento14/ZeroSpend/pull/37
branch: feat/configuracoes-membros
tags: [pr, frontend, configuracoes, sessao, dados]
status: merged
---

# PR #37 — feat(configuracoes): membros da empresa e convites

## 🎯 Contexto

Ordem 35 de [[2026-09-29-plano-da-v1-do-frontend]]. A especificação tem `users` com papel (`admin` ou
`member`), e a v1 já tinha os vínculos ([[SessaoLocal]]). Mas cada empresa só tinha quem a criou, sem
como chamar mais alguém.

## 🔧 Mudanças

- **Seção "Membros" em `/configuracoes`** ([[MembersSettings]]):
  - quem tem acesso, com iniciais, nome, e-mail, papel ("Administração" ou "Membro") e "Você";
  - os **convites pendentes**, com "Cancelar convite";
  - o **convite novo**, com e-mail e papel;
  - **"Remover"**, que pede confirmação ("Remover o acesso de …?").
- **Convite sem envio, com efeito de verdade:**
  - quem já tem conta neste navegador entra na empresa na hora;
  - quem não tem fica com o convite pendente, e **criar a conta com aquele e-mail aceita o convite**. A
    pessoa entra também na empresa convidada, com o papel do convite.

  A tela diz exatamente isso.
- **Repositório:** `inviteMember`, `revokeInvitation`, `removeMember` e `invitations` (campo aditivo com
  padrão na leitura). `addAccount` aceita os convites do e-mail. `useMembers()` entrega os membros sem
  o hash da senha.
- **Domínio:**
  - `Invitation` e `validateInviteDraft`, com a regra de e-mail corporativo da conta, que agora é uma
    função só;
  - `ROLE_LABELS` e `initials()`, que saiu do menu da conta;
  - `normalizeEmail`, que foi de `auth.ts` para `text.ts`, porque o repositório também precisa dele.
- Quem não administra vê a lista, sem convite nem remoção.
- 5 testes novos (169 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

- O convite guarda o **e-mail de quem foi convidado**: é dado pessoal, e é o mínimo para o convite
  funcionar. Cancelar apaga o convite, e aceitar também (ele vira vínculo).
- A lista de membros mostra nome e e-mail só para quem é da empresa. O hash da senha não sai do
  repositório (`useMembers` devolve só id, nome e e-mail).
- Nos testes e no cofre, só os fictícios estáveis: Pessoa Exemplo e `pessoa@exemplo.com`.

## 🧠 Decisões técnicas

- **Convite com efeito local, não "enviado".** A v1 não envia e-mail, e dizer "convite enviado" seria
  prometer um efeito que o código não produz. O convite vale para a conta criada neste navegador, que
  é o alcance real da v1.
- **Só e-mail da empresa.** Um convite para Gmail nunca viraria conta, porque criar conta exige e-mail
  corporativo. A regra é a mesma função nas duas pontas (`corporateEmailError`).
- **Ninguém tira o próprio acesso.** Assim a empresa nunca fica sem quem a administre, sem precisar
  contar administradores.
- **Remover pede confirmação,** e não "Desfazer": voltar exige um convite novo, e a descrição do diálogo
  diz isso.
- **A conta nova ainda cria a própria empresa.** O formulário de criar conta pede o nome da empresa, e
  o convite aceito aparece como mais uma empresa no seletor. Uma tela de "entrar numa empresa que me
  convidou" fica <A DEFINIR>, com o backend e o convite por e-mail.

## ⚠️ Armadilhas e aprendizados

- **Depois de "Sair", entrar volta para a página em que a pessoa estava** (`?para=`), e não para o
  dashboard. O teste precisou esperar a página certa. É o comportamento desejado da
  [[GuardaDeSessao]].

## 🧪 Como testar

`npm test` (169 testes), lint, type-check e build passam.

No navegador (Chromium headless, 1440px claro e escuro e 390px, com o console limpo):

1. Na conta de demonstração, "Admin Exemplo", com "Você" e "Administração", sem "Remover".
2. Convidar:
   - `pessoa@gmail.com` mostra "Use o e-mail da empresa…";
   - `Pessoa@Exemplo.com` mostra "Convite criado para pessoa@exemplo.com.", e o convite aparece
     pendente;
   - o mesmo e-mail de novo mostra "Já existe um convite para este e-mail.".
3. Sair e criar a conta de Pessoa Exemplo com esse e-mail: o seletor mostra "Exemplo Comércio Ltda" e
   "Exemplo Tecnologia Ltda".
4. Na Tecnologia, como membro: os dois nomes aparecem, sem convite nem remoção, e os dados da empresa
   ficam desabilitados.
5. De volta como administração:
   - Pessoa Exemplo aparece como "Membro", com "Remover";
   - confirmar mostra "Acesso de Pessoa Exemplo removido.".
6. A 390px, nada vaza.

## 🚫 O que não foi verificado

- Mudar o papel de quem já é membro: ficou de fora (remover e convidar de novo resolve).
  <A DEFINIR> se fizer falta.

## 📎 Documentação afetada

- [[MembersSettings]] (novo) · [[Configuracoes]] · [[SessaoLocal]] · [[RepositorioLocal]] · [[ModeloDeDominio]]
- [[CriarConta]] · [[criar-conta-ate-o-dashboard]] · [[glossario]]
- [[2026]] (changelog)
