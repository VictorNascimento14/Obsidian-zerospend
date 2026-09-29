---
tipo: pr
data: 2026-09-29
autor: VictorNascimento14
projeto: ZeroSpend
pr: 36
url: https://github.com/VictorNascimento14/ZeroSpend/pull/36
branch: feat/configuracoes-alertas
tags: [pr, frontend, configuracoes, alertas, dados]
status: aberto
---

# PR #36 — feat(configuracoes): antecedência e canais de alerta da empresa

## 🎯 Contexto

Ordem 34 de [[2026-09-29-plano-da-v1-do-frontend]].

- A [[visao-de-produto]] fala em renovação "dentro da antecedência escolhida", e a antecedência estava
  fixa em 7 dias.
- A [[2026-09-29-especificacao-do-mvp]] prevê o envio de alertas por e-mail (Resend) ou WhatsApp e diz
  que "a v1 guarda as preferências".

## 🔧 Mudanças

- **Seção "Alertas" em `/configuracoes`** ([[AlertSettings]]):
  - **antecedência** de 3, 7 (padrão), 15, 30 ou 60 dias;
  - **canais** E-mail e WhatsApp, com o aviso "Nesta versão nada é enviado: os alertas aparecem só no
    app. A escolha fica guardada para quando o envio existir."
- **A antecedência vale no app inteiro:** alertas (`currentAlerts`), o indicador "Renovações em N
  dias", o título da central, o sino e o destaque "em N dias" da tabela. `currentAlerts` e
  `computeKpis` passam a ler a antecedência da própria empresa.
- **Modelo:** `Organization` ganha `renewalLeadDays` e `alertChannels`.
  - `RENEWAL_LEAD_OPTIONS`, `defaultAlertSettings()` e `validateAlertSettings` são novos.
  - A semente, a conta nova e a empresa nova nascem com o padrão (7 dias, e-mail ligado).
- **Repositório:**
  - `updateAlertSettings`, com a mesma regra de papel dos dados da empresa (`administeredOrganization`,
    agora compartilhado);
  - empresa gravada sem as preferências é lida com o padrão.
- 6 testes novos (164 no total).

## 🕵️ Dado sensível (LGPD e sigilo)

**O número de WhatsApp não é pedido.** Nesta versão nada é enviado, e dado pessoal que não é usado não
se coleta (minimização, LGPD). A tela diz isso: "O número não é pedido nesta versão, que não envia
nada."

## 🧠 Decisões técnicas

- **Os canais são guardados, mas nada é enviado, e a tela diz isso.** A regra 7 do repositório proíbe
  prometer um efeito que o código não produz. A antecedência, ao contrário, tem efeito na hora.
- **Antecedência numa lista fechada** (3 a 60 dias), e não em um campo livre. Com 60 dias dá tempo de
  renegociar um plano anual, e a lista evita validar número digitado.
- **Preferências na empresa**, como a moeda e a cotação, e não numa tabela à parte. A especificação só
  tem `organizations` para isso. Os campos novos seguem a regra de campo aditivo com padrão na leitura
  ([[ADR-001-frontend-primeiro-com-dados-locais]], Atualizações).
- **`currentAlerts(assinaturas, empresa, hoje)` perdeu o parâmetro de antecedência.** A antecedência é
  da empresa, então passá-la à parte deixaria uma tela esquecer de passar e voltar aos 7 dias.
  `renewalAlerts` continua recebendo o número, como regra pura.

## ⚠️ Armadilhas e aprendizados

- **O `id` do `Switch` do Base UI vai para o `<input>` escondido,** e o elemento com `role="switch"`
  ganha um id gerado. O rótulo funciona (o nome acessível é "E-mail"), mas o teste precisa achar o
  interruptor pelo papel e pelo nome, e não pelo id.

## 🧪 Como testar

`npm test` (164 testes), lint, type-check e build passam.

No navegador (Chromium headless, 1440px claro e escuro e 390px, com o console limpo), na conta de
demonstração:

1. Com 7 dias: "Renovações em 7 dias" 3, o sino com 7 e a tabela com 3 destaques.
2. 30 dias, WhatsApp ligado e e-mail desligado, depois "Salvar": aparece "Preferências de alerta
   salvas.", e o `localStorage` guarda 30 e `{"email":false,"whatsapp":true}`.
3. O dashboard mostra "Renovações em 30 dias" 11, e o sino, 15. A central diz "Renovações nos próximos
   30 dias", com 11.
4. Recarregar mantém a escolha. Como membro, tudo fica desabilitado, sem "Salvar".
5. A 390px, nada vaza.

## 🚫 O que não foi verificado

- O envio (e-mail com Resend, WhatsApp) é do backend, <A DEFINIR> com ele, assim como o número do
  WhatsApp e quem recebe.

## 📎 Documentação afetada

- [[AlertSettings]] (novo) · [[Configuracoes]] · [[AlertasDeRenovacao]] · [[KpiCards]] · [[AlertsCenter]]
- [[SubscriptionsTable]] · [[ModeloDeDominio]] · [[RepositorioLocal]] · [[DadosDeDemonstracao]] · [[glossario]]
- [[2026]] (changelog)
