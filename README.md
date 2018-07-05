# Git Merge vs Git Rebase

## Objetivo

Tanto o `git merge` quanto o `git rebase` servem para integrar duas branches com commits distintos.

## Como funciona?

### Merge

Gera um commit adicional simbolizando a união de todos os commits de uma branch em outra.

[![merge](https://github.com/cwi-tech-talk/git-rebase/raw/develop/img/merge.png)](#merge)

#### Como Usar

`git pull develop`
`git checkout feature-branch`
`git merge develop`

#### Vantagens
- mais simples (fica)
- 1 commit que identifica o merge
- commit de correção de merge evidenciado

#### Desvantagens
- histórico sujo
- separar coisas juntadas

### Rebase

Move os commits da branch atual para o final da branch alvo, sem gerar commit adicional, no entanto, todos os commits da branch atual são reescritos na árvore de commits, ou seja, serão todos novos commits.

[![rebase](https://github.com/cwi-tech-talk/git-rebase/raw/develop/img/rebase.png)](#rebase)

#### Como Usar

`git pull develop`
`git checkout feature-branch`
`git rebase develop`

#### Como Usar Após Remote Branch

Quando a branch a ser feito rebase já existir no repositório remoto, o push para essa branch deverá ser feito usando o modificador `--force`. Usar isso com **muito** cuidado, conforme detalhe em  VerThe Golden Rule of Rebasing.

`git push --force`

#### Vantagens
- histórico de commit mais legível
- minha branch contém só os commits da minha branch
- resolver os conflitos mais fácil

#### Desvantagens
- fora de ordem cronológica
- reescreve o autor do commit
- resolver conflitos repetidos 
- confuso de usar

#### The Golden Rule of Rebasing
- Nunca fazer rebase de uma branch pública, ou seja, que esteja sendo usada por outros colaboradores
- Exceções:
    - Pode fazer rebase de uma branch pública quando for integrar ela a outra branch, removendo-a nesse momento.
    - Ao integrar alterações **obrigatórias** da branch principal, **sempre** em comum acordo com todos colaboradores dessa branch.

## Proposta
- **SEMPRE** integrar alterações na tua branch com `rebase`.
- **SEMPRE** entregar uma branch com `merge`, de preferência com `MR`.
- **NUNCA** entregar uma branch usando `fast-forward`.

### Resultado
- Mantém um commit que identifica o merge entre as duas branches
- Mantém o histórico de commit limpo com apenas os `merge commit` de entrega de feature.
- Mantém o autor dos commits
- Ordena a árvore de commit de acordo com a ordem de entrega das features
    - Commits individualmente ficam mesmo fora de ordem

## Dicas
- Resolver somente o que conflitou
    - **NUNCA** ajustar mais coisas do que o conflito atual, quando houver
- Replace local branch
    - Quando ocorrer de uma branch pública for reescrita (rebased), todos repositórios locais deverão subsituir a respectiva branch local pela atualizada no remote. Para fazer isso: 
    `git fetch origin`
    `git reset --hard origin/branch`
    
## Referências
- [Atlassian](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [GitFlow](https://www.atlassian.com/git/articles/git-team-workflows-merge-or-rebase)
- [Hackernoon](https://hackernoon.com/git-merge-vs-rebase-whats-the-diff-76413c117333)
