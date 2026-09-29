---
tipo: aprendizado
data: 2026-09-29
contexto: PR #2 (CI) — a máquina reiniciou no meio do PR
tags: [aprendizado, infra, git]
autor: VictorNascimento14
---

# Queda da máquina deixa objeto do git vazio — e o git não o regrava

## O sintoma

A máquina reiniciou no meio do [[2026-09-29-pr-002-ci]]. Na volta, no repositório de código:

- `git branch` → `fatal: failed to resolve HEAD as a valid ref`;
- os dois workflows recém-escritos com **0 bytes** no disco;
- `git fsck --full` → `object file .git/objects/… is empty` para os dois blobs que o `git add` tinha
  gravado segundos antes; o ref da branch nova (`refs/heads/chore/ci`) também vazio, e o fim de
  `.git/logs/HEAD` preenchido com bytes nulos.

Só o que foi gravado nos segundos anteriores à queda ficou vazio. O que já estava no GitHub (a `main`
e o cofre) seguiu íntegro.

## A causa

O sistema de arquivos (ext4) já tinha registrado a criação dos arquivos, mas o conteúdo ainda não
tinha chegado ao disco. O mais provável é a alocação atrasada, que o ext4 usa por padrão.

A armadilha que faz isso custar caro é do git: ele considera um objeto **presente se o arquivo
existe**, sem olhar o conteúdo. Reescrever o arquivo com o mesmo texto e dar `git add` gera o mesmo
hash; o git vê que o arquivo do objeto já existe e **não o regrava**. O commit passa sem erro, e o
repositório segue corrompido até um `push` ou um `fsck` acusarem.

Reproduzido com git 2.43: com um objeto de 0 bytes no lugar, `git add` do mesmo conteúdo e `commit`
passam, e o objeto continua com 0 bytes.

## A correção

Com o `git fsck --full` listando o que está vazio (conferido também com `od`):

1. apagar os objetos vazios que o `fsck` aponta — só eles;
2. apagar o ref e o reflog vazios da branch e recriar a branch no commit de origem
   (`git update-ref refs/heads/<branch> <sha>`);
3. cortar os bytes nulos do fim de `.git/logs/HEAD`;
4. `git reset` para refazer o índice a partir do `HEAD` (a árvore de trabalho não muda);
5. `git fsck --full` limpo **antes** de reescrever o conteúdo perdido.

O conteúdo perdido era só o que ainda não tinha sido publicado, e foi reescrito.

## Como evitar

- Depois de uma queda, `git fsck --full` nos dois repositórios (código e cofre) **antes** de qualquer
  `git add`.
- Arquivo que deveria ter conteúdo e está vazio:
  `find . -path ./.git -prune -o -type f -size 0 -print` (os `.gitkeep` do cofre são vazios de
  propósito).
- Push cedo: o que já está no GitHub sobrevive a qualquer queda local.
