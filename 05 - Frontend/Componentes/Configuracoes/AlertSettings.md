---
tipo: funcionalidade
camada: frontend
area: Configuracoes
rota: /configuracoes
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, configuracoes, alertas]
---

# AlertSettings

## O que é

A seção "Alertas" das [[Configuracoes]]: quando uma renovação vira alerta (antecedência) e por onde a
empresa quer ser avisada (canais).

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/components/settings/alert-settings.tsx` | `AlertSettings` e o formulário |
| `src/lib/domain/alerts.ts` | `RENEWAL_LEAD_OPTIONS`, `DEFAULT_RENEWAL_LEAD_DAYS`, `defaultAlertSettings()`, `AlertSettings` (o tipo) |
| `src/lib/domain/validation.ts` | `validateAlertSettings` |
| `src/lib/data/repository.ts` | `updateAlertSettings` ([[RepositorioLocal]]) |

## Comportamento

- **Antecedência:** "3 dias antes", "7 dias antes (padrão)", "15 dias antes", "30 dias antes" ou "60
  dias antes". Salva, ela muda na hora os alertas ([[AlertasDeRenovacao]]):
  - o sino ([[Notificacoes]]) e a central ([[AlertsCenter]]);
  - o painel e o indicador "Renovações em N dias" do dashboard ([[KpiCards]]);
  - o destaque "em N dias" da tabela ([[SubscriptionsTable]]).
- **Canais:** interruptores **E-mail** ("Para quem administra a empresa.") e **WhatsApp** ("O número
  não é pedido nesta versão, que não envia nada."). Acima deles, o aviso: "Nesta versão nada é enviado:
  os alertas aparecem só no app. A escolha fica guardada para quando o envio existir."
- **Salvar:** "Preferências de alerta salvas."
- **Quem não administra** vê tudo desabilitado, sem "Salvar", com "Só quem administra a empresa altera
  estes dados.".

## Estados (vazio, carregando, erro)

- **Erro** (não acontece pela tela, que só oferece as opções válidas): "Escolha a antecedência." ou
  "Escolha os canais.".
- **Carregando:** o esqueleto da [[GuardaDeSessao]].

## Regras de uso

- Nenhuma tela promete aviso por e-mail ou WhatsApp enquanto nada for enviado.
- O número de WhatsApp só entra quando o envio existir (LGPD: não se coleta o que não se usa).
- A antecedência é da empresa. Nenhuma tela usa `DEFAULT_RENEWAL_LEAD_DAYS` direto, a não ser como
  padrão de empresa nova.

## Histórico de mudanças

- [[2026-09-29-pr-036-configuracoes-alertas]] — criado.
