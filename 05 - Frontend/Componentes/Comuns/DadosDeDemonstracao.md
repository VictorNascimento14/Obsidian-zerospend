---
tipo: funcionalidade
camada: frontend
area: Comuns
rota: —
ultima_atualizacao: 2026-09-29
tags: [funcionalidade, dados]
---

# Dados de demonstração

## O que é

As empresas e assinaturas com que a conta de demonstração nasce. Tudo fictício; cada item existe
para mostrar uma regra do produto.

## Onde está no código

`src/lib/data/seed.ts` — `createDemoData(today)` e `DEMO_ORGANIZATION_IDS`. Testes em
`src/lib/data/seed.test.ts`.

## Comportamento

As datas são **relativas ao dia em que a demonstração é criada** (`inDays` a partir de `today`).

**Exemplo Tecnologia Ltda** (BRL, cotação R$ 5,40), gasto mensal R$ 7.444,75:

| Assinatura | Categoria | Cobrança | Ciclo | Próxima | Status | Por quê |
|---|---|---|---|---|---|---|
| Slack | Comunicação | R$ 880,00 | Mensal | +12 dias | Ativa | |
| Zoom | Reuniões e vídeo | R$ 159,90 | Mensal | +5 dias | Ativa | renovação do briefing |
| Google Workspace | Produtividade | R$ 1.176,00 | Mensal | +3 dias | Ativa | renovação urgente |
| Figma | Design | US$ 75,00 | Mensal | +18 dias | Ativa | redundância (com Canva) · dólar |
| Canva | Design | R$ 1.199,00 | Anual | +40 dias | Ativa | redundância · anual |
| HubSpot | CRM | R$ 1.450,00 | Mensal | +9 dias | Ativa | redundância (com Pipedrive) |
| Pipedrive | CRM | US$ 99,00 | Mensal | +21 dias | Ativa | redundância · dólar |
| GitHub | Desenvolvimento | US$ 84,00 | Mensal | +2 dias | Ativa | renovação urgente |
| RD Station Marketing | Marketing | R$ 890,00 | Mensal | +26 dias | Ativa | |
| Conta Azul | Financeiro | R$ 229,00 | Mensal | +15 dias | Ativa | |
| Gupy | RH | R$ 7.800,00 | Anual | +120 dias | Ativa | anual caro |
| 1Password | Segurança | US$ 239,40 | Anual | +200 dias | Ativa | dólar · anual |
| Dropbox | Armazenamento | R$ 119,00 | Mensal | +8 dias | Cancelada | não conta em nada |
| ChatGPT Team | Outros | US$ 60,00 | Mensal | +6 dias | Em revisão | veio do extrato |
| Adobe Acrobat Pro | Outros | R$ 85,00 | Mensal | +25 dias | Em revisão | veio do extrato |

**Clínica Exemplo** (BRL, cotação R$ 5,40): Google Workspace (R$ 294,00, +14), Canva (R$ 34,90, +20),
Zoom (R$ 79,90, +2) e Conta Azul (R$ 129,00, +11) — todas mensais e ativas, sem redundância.

## Regras de uso

- Mudou a demonstração? Atualize esta tabela e o teste do gasto mensal no mesmo PR.
- Uma ferramenta por categoria, fora os pares de redundância — senão a demonstração ganha duplicidade
  falsa.
- Só nomes fictícios estáveis do cofre (empresas, pessoas) e valores inventados.

## Histórico de mudanças

- [[2026-09-29-pr-012-sementes]] — criado.
