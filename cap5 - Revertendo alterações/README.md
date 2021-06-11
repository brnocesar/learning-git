# Capítulo 5 - Revertendo alterações

1. [Revertendo _commits_](#1-revertendo-_commits_)  
2. [Mudando entre estados](#2-Mudando-entre-estados)  
3. [Armazanando-na-pilha](#3-Armazanando-na-pilha)

## 1 Revertendo _commits_

```sh
git checkout -- <file> # descarta alteração que nao está em stage
git reset HEAD <file> # remove a alteração de stage
git reset HEAD~N # reverte os últimos N commits
git revert <hash> # reverte um commit e gera um novo marcando isso
```

## 2 Mudando entre estados


```sh
git checkout <hash> # altera o estado do HEAD para o commit indicado
```


## 3 Armazanando na pilha

Não é possível mudar de _branch_ com alterações fora de _stage_ (não commitadas), então se isso for necessário é preciso mudar 

```sh
git stash # manda as alterações inacabas da branch para pilha de WIPs
git stash list # lista todos WIPs
git stash apply N # pega a alteracao de número N
git stash drop N # remove a alteracao de número N da pilha
git stash pop # pega a ultima alteração e remove da pilha
```