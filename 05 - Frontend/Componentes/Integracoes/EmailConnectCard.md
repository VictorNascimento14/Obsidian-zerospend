---
tipo: funcionalidade
camada: frontend
area: Integracoes
rota: /onboarding
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, integracoes, onboarding]
---

# EmailConnectCard

## O que é

A conexão com a caixa de e-mail da empresa (Google Workspace ou Microsoft 365), que é a entrada B do
fluxo 1 da [[2026-09-29-especificacao-do-mvp]]. Na v1 ela é **simulada e rotulada como demonstração**
([[ADR-001-frontend-primeiro-com-dados-locais]]): mostra o que a conexão faria e um resultado de
exemplo, e não lê nem grava nada.

## Onde está no código

| Arquivo | O que tem |
|---|---|
| `src/components/integrations/email-connect-card.tsx` | `EmailConnectCard` (o card) e o diálogo de cada provedor |
| `src/lib/import/email.ts` | `InvoiceMetadata`, `EMAIL_PROVIDERS` (nome e escopo) e `demoInvoices(hoje)` |

## Comportamento

- **Card "E-mail da empresa":**
  - traz a etiqueta "Demonstração";
  - a descrição diz que a conexão é simulada e que nada é lido da caixa de entrada;
  - tem um botão por provedor, com o monograma dele.
- **Diálogo "Conectar o <provedor>":** a descrição repete que é demonstração.
  1. **O que a conexão faria:**
     - pediria permissão só de leitura (`gmail.readonly` no Google, `Mail.Read` na Microsoft);
     - procuraria só mensagens com "fatura", "cobrança", "comprovante", "invoice" ou "receipt";
     - de cada fatura, guardaria só fornecedor, valor, moeda, data e categoria, e nunca o corpo do
       e-mail.

     Os botões são "Cancelar" e "Simular a conexão".
  2. **Resultado:**
     - "4 faturas de exemplo encontradas", cada uma com monograma, categoria, data e valor na moeda da
       fatura;
     - o aviso "É tudo o que ficaria de cada fatura: fornecedor, valor, moeda, data e categoria. Nada
       foi importado.";
     - o botão "Fechar".
- Reabrir o diálogo recomeça da primeira etapa.

## Estados (vazio, carregando, erro)

Não há carregamento nem erro: nada sai do navegador. As faturas de exemplo têm datas relativas a hoje,
entre 2 e 14 dias atrás.

## Regras de uso

- **Nada da simulação vira assinatura:** faturas inventadas não entram na empresa de ninguém.
- **Metadados, nunca a mensagem:** `InvoiceMetadata` tem só os cinco campos da especificação, e o teste
  reprova campo a mais. Quando a conexão de verdade chegar, o que ela devolver cabe nesse tipo.
- **O texto fica no condicional** enquanto a conexão for simulada: descreve o que ela faria, sem afirmar
  que acontece.

## Histórico de mudanças

- [[2026-09-29-pr-031-onboarding-email]] — criado, no [[Onboarding]].
