# CLAUDE.md

> Instruções de documentação para o **Claude Code** operar neste cofre Obsidian do projeto **ZeroSpend**.
> Este cofre é um repositório Git sincronizado com o GitHub. Toda nota criada aqui vai pro repositório.

---

## 🎯 Propósito deste cofre

Documentar de forma viva e rastreável o **ZeroSpend** — gerenciador e auditor de assinaturas de
software para pequenas e médias empresas (*SaaS Spend Management*). O produto consolida os gastos
recorrentes com software de uma empresa, aponta ferramentas redundantes e avisa antes de cada
renovação. Público inicial: PMEs de 5 a 50 funcionários e BPOs financeiros que cuidam de várias delas.

Este cofre é a **fonte de verdade documentada** do projeto: ADRs, notas de PR, funcionalidades,
fluxos, runbooks e aprendizados. Se cofre e código divergirem, o cofre está desatualizado — e corrigir
isso faz parte da tarefa que descobriu a divergência.

> **Estado atual (2026-09-29): só front-end.** Não existe backend nem banco. Os dados vivem num
> repositório local no navegador (ver [[ADR-001-frontend-primeiro-com-dados-locais]]). O que o produto
> terá depois (Supabase, IA de extração, OAuth real, alertas por e-mail) está na especificação do MVP:
> [[2026-09-29-especificacao-do-mvp]]. As pastas `06 - Backend/` e `07 - Banco de Dados/` existem
> vazias, reservadas para quando isso mudar.

---

## 📍 Caminho canônico deste cofre — resolva por variável de ambiente

**Nunca escreva um caminho absoluto de máquina em nenhuma nota, script ou instrução.**

```bash
VAULT="${ZEROSPEND_VAULT:?defina ZEROSPEND_VAULT no seu shell profile}"
[ -d "$VAULT/00 - Índice" ] || { echo "ZEROSPEND_VAULT não aponta pro cofre"; exit 1; }
```

- Valor por máquina fica no shell profile de cada um, **não** no repositório.
- A tabela de máquinas conhecidas vive em [[caminho-canonico-do-cofre]] (`10 - Meta/`) — é
  documentação, não configuração.
- Se `ZEROSPEND_VAULT` não estiver definida ou não existir no disco: **pare e avise**. Nunca
  "documente no repo de código porque o cofre não estava acessível".

---

## 🗺️ Mapa de repositórios

| Papel | Repositório | Variável |
|---|---|---|
| **ZeroSpend** (código: front-end Next.js) | `VictorNascimento14/ZeroSpend` | `$ZEROSPEND_REPO` |
| **Docs ZeroSpend** (este cofre) | `VictorNascimento14/Obsidian-zerospend` | `$ZEROSPEND_VAULT` |

O design system vem do arquivo do Figma **SaaS Design System & UI Kit (Community)**. Os tokens
(cores, tipografia, cantos, sombras) estão transcritos em [[linguagem-visual]]; a decisão de como eles
entram no código está em [[ADR-002-design-system-do-figma-com-shadcn]].

---

## 🚦 Regra #0 — SEMPRE antes de começar

```bash
cd "$ZEROSPEND_VAULT" && git pull --rebase
```

Evita conflito com commits de outra máquina/pessoa. Se houver conflito, resolva antes de escrever.

---

## 🧹 Regra da raiz limpa (não negociável)

Na **raiz** do cofre só existem estes arquivos:

```
README.md
CLAUDE.md
.gitignore
.gitattributes
```

Qualquer outro `.md` na raiz é erro. Qualquer pasta fora do espinhaço `00 - Índice` … `10 - Meta` é
erro. Não existe prefixo ad-hoc (`ANALISE-`, `PLANO-`, `HANDOFF-`, `PENDENCIA-`, `INCIDENTE-`…) — cada
um tem destino nomeado:

| Se você ia criar… | Vai em | `tipo:` |
|---|---|---|
| `ANALISE-*`, `AUDITORIA-*` | `08 - Infra e Deploy/Auditorias/<YYYY-MM-DD>-<slug>.md` | `auditoria` |
| `DESIGN-*`, `PLANO-*` | decisão → `02 - ADRs/`; plano → `08 - Infra e Deploy/Planos/<YYYY-MM-DD>-<slug>.md` | `adr` \| `plano` |
| `HANDOFF-*`, `PENDENCIA-*`, `DIVIDA-TECNICA-*` | `08 - Infra e Deploy/Pendencias/<YYYY-MM-DD>-<slug>.md` | `handoff` \| `pendencia` \| `divida-tecnica` |
| `INCIDENTE-*`, `INVESTIGACAO-*` | `04 - Aprendizados/<YYYY>/<YYYY-MM-DD>-<slug>.md` | `incidente` |
| Visão geral do produto | `00 - Índice/visao-de-produto.md` (atualize, não crie) | `indice` |
| Extrato, fatura ou planilha de exemplo | **não vai pro cofre** — fixture de teste vive no repo de código, e só com dado fictício | — |

Pasta nova dentro de 00–10 é permitida quando há ≥ 3 notas do mesmo tipo; pasta nova **na raiz** exige
PR neste `CLAUDE.md`.

> **Por que a regra é dura.** Cofres irmãos acumularam dezenas de notas soltas na raiz, cada uma "só uma
> exceção". Documentação boa que ninguém acha é documentação perdida.

---

## ✅ Checklist de documentação por PR

Para **todo PR**, no mínimo:

- [ ] **Nota de PR** em `01 - PRs/<YYYY>/<YYYY-MM-DD>-pr-<NNN>-<slug>.md`
- [ ] **Entrada no changelog** em `03 - Changelog/<YYYY>.md`, seção `## 🚧 [Não lançado]`
- [ ] **Entrada no MOC** [[prs]] (`00 - Índice/prs.md`), no mesmo commit

Condicionalmente:

- [ ] **ADR** em `02 - ADRs/` — decisão difícil de reverter, que afeta múltiplos módulos ou troca de
      trade-off
- [ ] **Nota de funcionalidade** em `05 - Frontend/` — componente, página, fluxo ou módulo criado ou
      alterado de forma relevante
- [ ] **Nota de aprendizado** em `04 - Aprendizados/<YYYY>/` — descoberta não-óbvia, armadilha, bug
      instrutivo
- [ ] **Runbook** em `08 - Infra e Deploy/Runbooks/` — procedimento operacional novo

---

## 📋 Tabela de decisão — "se mudou X, documentar em Y"

| O que mudou no PR | Onde documentar |
|---|---|
| Qualquer PR | `01 - PRs/<YYYY>/` + entrada em `03 - Changelog/<YYYY>.md` + [[prs]] |
| Decisão arquitetural (stack, padrão, persistência, regra de negócio) | `02 - ADRs/ADR-NNN-<titulo>.md` |
| Casca: sidebar, header, rotas, guarda de sessão | `05 - Frontend/Componentes/Shell/<PascalCase>.md` |
| Componente reusado por 2+ áreas | `05 - Frontend/Componentes/Comuns/<PascalCase>.md` |
| Componente de uma área só | `05 - Frontend/Componentes/<Area>/<PascalCase>.md` — áreas: `Dashboard`, `Assinaturas`, `Integracoes`, `Alertas`, `Configuracoes`, `Onboarding`, `Autenticacao` |
| Página/rota nova | `05 - Frontend/Paginas/<PascalCase>.md` |
| Fluxo de UX de várias telas (ex.: criar conta → onboarding → dashboard) | `05 - Frontend/Fluxos/<slug-kebab>.md` |
| Regra de domínio (cobrança, alerta, redundância, importação) ou camada de dados | `05 - Frontend/Componentes/Comuns/<PascalCase>.md` + ADR se mudar a estratégia |
| Token do design system (cor, tipo, raio, sombra) | [[linguagem-visual]] + ADR se mudar a estratégia |
| CI, deploy, ambiente | `08 - Infra e Deploy/Runbooks/` ou `Planos/` |
| Bug instrutivo, debug não-óbvio | `04 - Aprendizados/<YYYY>/<YYYY-MM-DD>-<slug>.md` |
| Termo do domínio com significado próprio no produto | `00 - Índice/glossario.md` |

---

## 📐 Nomenclatura de arquivos

| Tipo | Convenção | Exemplo |
|---|---|---|
| PR | `YYYY-MM-DD-pr-NNN-slug-kebab.md` | `2026-09-29-pr-001-scaffolding.md` |
| ADR | `ADR-NNN-titulo-kebab.md` (NNN com zero à esquerda) | `ADR-001-frontend-primeiro-com-dados-locais.md` |
| Componente / Página | `PascalCase.md` | `Paginas/Dashboard.md` |
| Fluxo | `slug-kebab.md` | `criar-conta-ate-o-dashboard.md` |
| Aprendizado / Incidente | `YYYY-MM-DD-slug-kebab.md` | `2026-09-29-data-sem-hora-volta-um-dia.md` |
| Runbook | `runbook-slug-kebab.md` | `runbook-rodar-local.md` |
| Plano / Pendência | `YYYY-MM-DD-slug-kebab.md` | `2026-09-29-plano-da-v1-do-frontend.md` |
| Changelog anual | `YYYY.md` | `2026.md` |
| MOC | `slug-kebab.md` | `zerospend-frontend.md` |

> ⚠️ **Nunca** espaço em nome de arquivo. Nunca `.md` na raiz. Nunca acento em nome de arquivo ou pasta
> novos (as pastas do espinhaço `00 - Índice` … `10 - Meta` são a única exceção, e já existem).

---

## 🏷️ Frontmatter obrigatório por tipo

Toda nota tem frontmatter YAML. `tipo:` sempre presente; `tags:` sempre com o próprio tipo como 1ª tag.

| `tipo:` | Campos obrigatórios | Opcionais |
|---|---|---|
| `pr` | `tipo, data, projeto, pr, url, tags, status` | `autor, branch` |
| `adr` | `tipo, numero, data, status, tags` | `autor, substitui, substituida_por` |
| `funcionalidade` | `tipo, camada, ultima_atualizacao, tags` | `area, rota` |
| `aprendizado` / `incidente` | `tipo, data, contexto, tags` | `autor` |
| `runbook` | `tipo, camada, escopo, ultima_atualizacao, tags` | `tempo_estimado` |
| `indice` | `tipo, ultima_atualizacao, tags` | `camada, status` |
| `changelog` | `tipo, ano, tags` | — |
| `plano` / `pendencia` / `auditoria` | `tipo, data, tags, status` | `autor, prazo` |
| `meta` / `glossario` | `tipo, ultima_atualizacao, tags` | — |

`status:` — `pr`: `aberto|em-review|merged|fechado` · `adr`: `proposto|aceito|rejeitado|substituído` ·
`plano`/`pendencia`/`auditoria`: `aberta|em-andamento|resolvida|descartada`.

**Tipo novo não se inventa em nota** — se precisar de um, abra PR alterando esta tabela.

> Única exceção: `README.md` e `CLAUDE.md` da raiz não têm frontmatter — são contrato do repositório.

---

## 🧰 Templates disponíveis (`09 - Templates/`)

| Template | Quando usar |
|---|---|
| [[template-pr]] | Toda nota de PR |
| [[template-adr]] | Toda ADR |
| [[template-funcionalidade]] | Componente, página, fluxo, módulo |
| [[template-aprendizado]] | `04 - Aprendizados/` |
| [[template-runbook]] | Todo procedimento operacional |

Copie o template, preencha os `<placeholders>`, **remova as seções que não se aplicam** e não deixe
placeholder vazio — se falta informação, escreva `TODO: <o que falta>`.

---

## 🔗 Regras de linkagem

Este cofre usa **wiki-links** (`[[Nome da Nota]]`), nunca link markdown para outra nota.

1. Nota de **PR** linka: funcionalidades criadas/alteradas · ADRs · aprendizados · o changelog do ano.
2. Nota de **funcionalidade** tem "Histórico de mudanças" linkando cada PR que a alterou.
3. **ADR** linka os PRs que a implementaram (e é linkada por eles).
4. **Aprendizado** linka o PR de origem.
5. Use o nome do arquivo **sem extensão**: `[[Dashboard]]`, nunca `[[Dashboard.md]]`.
6. Todo MOC de `00 - Índice/` recebe a entrada nova **no mesmo commit** que cria a nota.
7. **Link direcional**: quem é criado depois linka quem já existe, e a nota antiga ganha o backlink no
   mesmo commit.

---

## 🔒 Sigilo, LGPD e dado financeiro (regra do domínio)

O ZeroSpend lida com **o dinheiro de uma empresa**: extrato de cartão corporativo, fatura recebida por
e-mail, valor de contrato, quem é responsável por qual ferramenta. Extrato e caixa de e-mail carregam
dado pessoal (nome de portador, e-mail de funcionário) sob a Lei 13.709/2018, e o gasto da empresa é
informação comercial sigilosa. Este cofre opera no **modo mais restritivo**:

- ❌ **Nunca** linha de extrato real, número de cartão, fatura real, corpo ou assunto de e-mail real,
  nem o gasto real de uma empresa real — nem em nota, nem em log colado, nem em print.
- ✅ Use os exemplos fictícios **estáveis** (sempre estes):
  - administradora: `Admin Exemplo` · e-mail: `admin@zerospend.app`
  - pessoa da equipe: `Pessoa Exemplo` · e-mail: `pessoa@exemplo.com`
  - empresas: `Exemplo Tecnologia Ltda` e `Clínica Exemplo`
- ✅ Nome de fornecedor de software (Slack, Notion, Figma…) é nome público de produto e pode aparecer;
  o valor que acompanha é sempre fictício.
- ✅ Contagem agregada pode: "18 assinaturas em 9 categorias". Identificador de pessoa, não.
- ❌ Nunca token, senha, chave ou segredo de OAuth. Nem em "exemplo".
- ⚠️ **Corpo de e-mail nunca é guardado** — regra do produto desde a especificação: da fatura só ficam
  os metadados extraídos (fornecedor, valor, moeda, data, categoria).

> O histórico do Git é **permanente**: um `git rm` no commit seguinte não apaga o blob. O erro é barato
> de cometer e caríssimo de desfazer.

---

## 🔁 Fluxo final obrigatório

```bash
cd "$ZEROSPEND_VAULT"
git pull --rebase          # de novo: outra máquina pode ter escrito enquanto você escrevia
git add .
git commit -m "<mensagem seguindo a convenção>"
git push
```

O `git pull --rebase` acontece **duas vezes**: antes de escrever (Regra #0) e antes de publicar.
Nunca deixe nota não-commitada. Cofre só é útil sincronizado.

---

## 📝 Convenção de mensagem de commit

| Prefixo | Quando |
|---|---|
| `docs(pr-NNN):` | Nota de PR (ex.: `docs(pr-007):`) |
| `docs(adr):` | ADR nova ou atualizada |
| `docs(frontend):` | `05 - Frontend/` |
| `docs(infra):` | `08 - Infra e Deploy/` |
| `docs(changelog):` | `03 - Changelog/` |
| `docs(aprendizado):` | `04 - Aprendizados/` |
| `docs(indice):` | `00 - Índice/` |
| `chore:` | Estrutura, templates, `10 - Meta/`, config do Obsidian |

- Presente do indicativo: "adiciona", "documenta", "corrige".
- Primeira linha ≤ 72 caracteres.
- **Zero menção a ferramenta de IA** — nem `Co-Authored-By`, nem "Generated with", em lugar nenhum.
- Mensagem de commit também é histórico permanente: **nenhum dado pessoal, nenhum segredo** nela.

---

## 🩺 Verificação de saúde (rode antes de um push grande)

Detalhes em [[checklist-de-saude-do-cofre]]. Todas devem sair **vazias**:

```bash
cd "$ZEROSPEND_VAULT"
# (a) raiz limpa: só README.md e CLAUDE.md
find . -maxdepth 1 -name '*.md' -not -name 'README.md' -not -name 'CLAUDE.md'
# (b) espaço em nome de arquivo (as pastas do espinhaço são exceção)
find . -path ./.git -prune -o -name '* *' -not -name '?? - *' -print
# (c) nota sem tipo: no frontmatter
grep -rL --include='*.md' '^tipo:' . | command grep -vE '^(\./)?(README|CLAUDE)\.md$'
# (d) numeração de ADR duplicada
ls "02 - ADRs" | grep -oE '^ADR-[0-9]{3}' | sort | uniq -d
```

---

## 🚫 Não faça

- ❌ **Não documente código sem ler o diff.** Descrição de PR não é fonte.
- ❌ **Não invente decisão técnica que não existiu.** Se o autor não justificou, escreva `TODO: confirmar`.
- ❌ **Não invente fato sobre o sistema.** O que não está decidido se escreve como `<A DEFINIR>`.
- ❌ **Não duplique conteúdo entre notas.** Wiki-link em vez de copiar.
- ❌ **Não esqueça o `git pull --rebase`** no início e no fim.
- ❌ **Não comite segredo nem dado real.** Ver a seção de sigilo.
- ❌ **Não crie `.md` na raiz** nem subpasta fora de 00–10.
- ❌ **Não crie nota sem frontmatter.**
- ❌ **Não reaproveite número de ADR.** Consulte `proximo_numero_livre` em [[adrs]] e incremente no
  mesmo commit.
- ❌ **Não use `--no-verify` nem `--force`** sem ordem explícita.

---

## 📚 Referências rápidas

- Estrutura e propósito de cada pasta: [[guia-de-uso]] (`10 - Meta/`)
- Path do cofre por máquina: [[caminho-canonico-do-cofre]] (`10 - Meta/`)
- Saúde do cofre: [[checklist-de-saude-do-cofre]] (`10 - Meta/`)
- Ponto de partida do produto: [[visao-de-produto]] · [[glossario]] · [[roadmap]] · [[linguagem-visual]]
- Especificação do MVP (inclui o que vem depois do front-end): [[2026-09-29-especificacao-do-mvp]]
- Plano de execução da v1: [[2026-09-29-plano-da-v1-do-frontend]]
- Changelog atual: [[2026]]
