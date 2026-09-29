---
tipo: plano
data: 2026-09-29
status: aberta
tags: [plano, produto, mvp]
autor: VictorNascimento14
---

# Especificação do MVP — SaaS Spend Management

Especificação técnica e funcional entregue pelo dono em 2026-09-29, junto com o briefing de design.
Transcrita aqui para ser a referência do produto inteiro — inclusive do que **não** entra na v1, que é
só front-end ([[ADR-001-frontend-primeiro-com-dados-locais]]). A coluna "v1" diz o que o front-end já
cobre; o resto espera backend.

## 1. Objetivo

Um gestor de PME conecta o e-mail ou sobe o extrato do cartão (CSV) e vê, **em menos de 5 minutos**,
a lista consolidada dos gastos recorrentes com software — e passa a receber alerta de renovação.

- Tempo estimado de desenvolvimento do MVP completo: 2 a 3 semanas.
- Público inicial: pequenas empresas (5 a 50 funcionários) e BPOs financeiros.

## 2. Stack recomendada

| Camada | Escolha | v1 |
|---|---|---|
| Front-end | Next.js (React), Tailwind CSS, shadcn/ui | ✅ |
| Back-end e banco | Supabase (PostgreSQL, Auth, Row Level Security) | ⏳ |
| Extração e categorização | OpenAI API (`gpt-4o-mini`) | ⏳ — a v1 reconhece fornecedores por um catálogo local |
| Leitura de e-mail | OAuth2 Google (Workspace) ou Microsoft Graph | ⏳ — a v1 simula a conexão, rotulada como demonstração |
| Envio de alertas | Resend (e-mail) ou Evolution API / Z-API (WhatsApp) | ⏳ — a v1 guarda as preferências |
| Hospedagem | Vercel (front) e Supabase Cloud (banco) | ⏳ |

## 3. Modelo de dados

| Tabela | Campos |
|---|---|
| `organizations` | `id` (uuid, PK) · `name` (text) · `created_at` (timestamp) |
| `users` | `id` (uuid, PK) · `organization_id` (uuid, FK → organizations) · `email` (text, unique) · `role` (`admin` \| `member`) |
| `subscriptions` | `id` (uuid, PK) · `organization_id` (uuid, FK) · `vendor_name` · `category` · `amount` (numeric) · `currency` (`BRL`, `USD`) · `billing_cycle` (`monthly` \| `annually`) · `next_billing_date` (date) · `status` (`active` \| `review_needed` \| `cancelled`) · `source` (`email_scan` \| `csv_upload` \| `manual`) |

`subscriptions` é o coração do sistema. Os tipos do front-end seguem estes nomes para a troca pelo
banco ser 1:1.

## 4. Fluxos e requisitos funcionais

### Fluxo 1 — Onboarding e conexão de dados

1. O usuário cria conta com **e-mail corporativo**.
2. Duas entradas de dados:
   - **A — Upload de CSV:** extrato do cartão corporativo, campos mínimos *Data, Descrição, Valor*.
   - **B — OAuth Google/Microsoft:** o app lê e-mails com termos como "receipt", "invoice",
     "comprovante", "fatura", "cobrança".

### Fluxo 2 — Motor de processamento

- O texto da transação (CSV) ou o corpo do e-mail vai para o modelo de IA com o prompt: *"Analise este
  texto de fatura/extrato. Extraia: 1. Nome do Fornecedor SaaS, 2. Valor, 3. Moeda, 4. Data do
  Pagamento, 5. Categoria do Software. Retorne em formato JSON válido."*
- O resultado entra em `subscriptions` com status **`review_needed`**.

### Fluxo 3 — Dashboard de gestão

- **Cards do topo:** gasto total mensal (convertido para a moeda padrão da empresa), assinaturas
  detectadas (contador) e economia estimada (soma dos alertas de duplicidade/inatividade).
- **Tabela principal:** todas as assinaturas; permite editar valores, alterar a data de renovação,
  marcar responsável e excluir.
- **Duplicidades:** mais de uma assinatura na mesma categoria (ex.: dois CRMs) ganha a tag
  **"Ferramenta Redundante"**.

### Fluxo 4 — Alertas automáticos

- Cron diário (Supabase ou Vercel) procura assinaturas com `next_billing_date - 7 dias == hoje`.
- Dispara e-mail via Resend para o admin: *"Atenção: A assinatura do software [Vendor] no valor de
  [Amount] renovará em 7 dias."*

## 5. Requisitos não funcionais e segurança

- **Privacidade (LGPD):** OAuth só com escopo de leitura restrito (`gmail.readonly`), filtrando
  mensagens ligadas a faturas. **Nunca guardar o corpo de e-mails** — só os metadados da fatura extraída.
- **Performance:** upload e processamento de um CSV de até **500 linhas em menos de 10 segundos**.

## 6. Roteiro em 3 fases

| Fase | Dias | Entregas |
|---|---|---|
| 1 — MVP mínimo | 1 a 7 | Setup Supabase + Next.js com Tailwind/shadcn · login/cadastro · upload de CSV · dashboard com tabela e totais |
| 2 — Inteligência e integração | 8 a 14 | OpenAI para categorizar e identificar o SaaS · detecção de duplicidade (mesma categoria) · OAuth do Google Workspace |
| 3 — Automação e notificação | 15 a 21 | Cron de datas de renovação · alertas por e-mail com Resend · teste com 3 PMEs parceiras |

## 7. Briefing de design (resumo)

Telas pedidas: casca com sidebar (Dashboard, Assinaturas, Extratos/Integrações, Alertas,
Configurações) e header (seletor de empresa, busca global, notificações, avatar); dashboard com quatro
KPIs (gasto mensal, economia potencial, assinaturas ativas, alertas urgentes de renovação), a
`SubscriptionsTable` (software, categoria, valor/mês, ciclo, próxima cobrança, status, ações) e um
painel lateral de alertas e duplicidades; onboarding com arrastar-e-soltar de extrato (CSV/PDF) e
integração OAuth com Google Workspace / Microsoft 365. Tokens em [[linguagem-visual]].

## Como a v1 cobre isto

Ordem de execução em [[2026-09-29-plano-da-v1-do-frontend]]. O que ficou de fora espera backend e está
no [[roadmap]].
