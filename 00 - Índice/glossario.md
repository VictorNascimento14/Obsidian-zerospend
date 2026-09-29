---
tipo: glossario
ultima_atualizacao: 2026-09-29
tags: [glossario]
---

# Glossário

| Termo | Significado no ZeroSpend |
|---|---|
| **Assinatura** | Um software que a empresa paga de forma recorrente (`subscription`). |
| **E-mail corporativo** | E-mail do domínio da empresa. A v1 recusa provedores pessoais (Gmail, Outlook e parecidos). |
| **Empresa** | A organização dona das assinaturas (`organization`). Uma pessoa pode cuidar de várias e troca pelo seletor do header. |
| **Catálogo de fornecedores** | A lista local de SaaS conhecidos com que a v1 reconhece assinaturas no extrato — não é IA ([[LeitorDeExtrato]]). |
| **Categoria** | O tipo de ferramenta (CRM, Design, Comunicação…), de uma lista fechada — é por ela que a redundância é detectada. |
| **Ciclo** | De quanto em quanto tempo a cobrança se repete: mensal (`monthly`) ou anual (`annually`). |
| **Próxima cobrança** | Data da próxima renovação (`next_billing_date`). Se a gravada já passou, a tela mostra a próxima a partir de hoje, sem regravar ([[RegrasDeCobranca]]). |
| **Valor/mês** | O custo mensal equivalente: o valor da cobrança, ou o anual dividido por 12. |
| **Cotação** | Quantos reais vale um dólar, informada pela empresa; converte o gasto para a moeda padrão. |
| **Moeda padrão** | A moeda em que a empresa vê os totais; valor em outra moeda é convertido. |
| **Status** | `Ativa` (confirmada), `Em revisão` (detectada, ainda não confirmada) ou `Cancelada`. |
| **Origem** | De onde a assinatura veio: varredura de e-mail, extrato CSV ou cadastro manual. |
| **Ferramenta redundante** | Assinatura que divide a categoria com outra — ex.: dois CRMs. A cancelada não conta, e "Outros" nunca é redundante ([[Redundancia]]). |
| **Economia potencial** | Estimativa do que dá para cortar consolidando as redundâncias: em cada grupo, mantém a mais cara e soma o custo mensal das outras. |
| **Alerta de renovação** | Aviso de que uma assinatura renova dentro da antecedência da empresa (7 dias por padrão, escolhida em [[AlertSettings]]); a cancelada não avisa ([[AlertasDeRenovacao]]). |
| **Canais de alerta** | Por onde a empresa quer ser avisada: e-mail ou WhatsApp. A v1 guarda a escolha e não envia nada ([[AlertSettings]]). |
| **Responsável** | A pessoa da empresa que responde por aquela assinatura — texto livre, porque nem sempre ela tem conta no ZeroSpend. |
| **Demonstração** (etiqueta) | O que depende do servidor e aparece na tela para mostrar o fluxo: a conexão com o e-mail e a leitura de PDF. Não lê nem grava nada ([[EmailConnectCard]], [[StatementImport]]). |
| **Dispensar (alerta)** | Marcar um alerta como tratado, para a empresa inteira. Ele some da central e do painel, e volta quando a situação muda: a cobrança seguinte, ou outra ferramenta no grupo ([[AlertsCenter]]). |
| **Conta de demonstração** | `admin@zerospend.app` (senha "demonstracao"): administra as duas empresas fictícias. |
| **BPO financeiro** | Escritório que terceiriza o financeiro de outras empresas. |

A regra exata de cada cálculo mora na nota da funcionalidade que o implementa.
