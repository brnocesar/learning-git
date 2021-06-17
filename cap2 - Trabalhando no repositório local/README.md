# Capítulo 2 - Trabalhando no repositório local

1. [Inicializando um repositório local](#2-inicializando-um-repositório-local)  
2. [Adicionando alterações](#3-adicionando-alterações)  
3. [Fazendo o _commit_](#4-fazendo-o-_commit_)  
4. [Visualizando o histórico](#5-visualizando-o-histórico)  
5. [Visualizando as alterações](#5-visualizando-as-alterações)  
6. [Ignorando arquivos](#6-ignorando-arquivos)  

## 1 Inicializando um repositório local

Caso o desenvolvimento já tenha sido iniciado antes da criação do projeto no GitLab, podemos inicializar um repositório local e posteriormente adicionar o _remote_ para subir o projeto para nuvem.

Primeiro vá até o diretório que será o repositório e então rode o seguinte comando:

```sh
git init
```

Note que agora temos uma pasta com nome `.git`, este diretório armazena todas as informações ...

## 2 Adicionando alterações

Outro comando que será muito utilizado é o `git status`. Usamos ele para verificar o estado do repositório em relação às alterações realizadas.

Por exemplo, caso você tenha inicializado o repositório em um projeto que já estava em desenvolvimento e possui vários arquivos, o retorno desse comando será uma lista de _untracked files_ e essa lista irá conter todos os arquivos "monitoráveis" (vamos chegar no `.gitignore` daqui a pouco). Ou seja, são arquivos que o git não conhece e por isso ainda não está rastreando suas alterações.

Para que o Git seja capaz de monitorar esses arquivos devemos colocá-los em _stage_ (também referido como _index_) e para isso usamos o comando `git add`. Essa ação pode ser feita cada arquivo de forma individual ou para um diretório inteiro:

```sh
git add arquivo1.txt arquivo2.txt arquivoN.txt # adiciona três arquivos
git add pasta1/ # adiciona todos os arquivos dentro do diretório pasta1
git add . # adiciona tudo a partir do diretório que você está
```

Quando temos alterações em _stage_ (adicionadas mas não "_commitadas_") o comando `git status` apresenta um retorno diferente, agora além da lista de _untracked files_ também temos a de _changes to be committed_. Ou seja, o Git já está monitorando essas alterações, porém elas ainda não fazem parte de um _checkpoint_ do repositório (o famoso _commit_).

Caso seja necessário retirar alguma alteração de _stage_, o retorno do `git status` indica o comando adequado para isso.

Outro comando interessante para usar é o `git diff`. Neste ponto já temos arquivos monitorados pelo Git, então se adicionar-mos alguma alteração em um desses arquivos, esse comando permite verificar a diferença entre o que está em _stage_ e o que não está. Como os editores decentes possuem recursos visualmente mais agradáveis, provavelmente esse comando será pouco utilizado.

## 3 Fazendo o _commit_

A incorporação de alterações ao repositório local (também referido como _HEAD_) é feita através do _commit_ com o comando `git commit`. 

O _commit_ deve conter uma mensagem de forma obrigatória e não pode ser uma string vazia. Ao escrever a mensagem procure fazê-la refletindo as alterações feitas no código da forma clara e sucinta. Algumas referências sugerem que a mensagem de _commit_ tenha no máximo 80 caracteres, mas caso você tenha bom senso basta utilizar. Abaixo são paresentados alguns exemplos:

```sh
git commit -m "Corrige bug de seleção múltipla de produtos"
git commit -m "Refatora procedure de atualização da tabela de produtos"
git commit -m "Implementa adição de novos produtos atráves de arquivo"
git commit -m "Adiciona filtros na tela para listagem de produtos"
```

<figure>
	<img src="cap2-1-flow.png" />
	<figcaption>Figura 4 - Fluxo de alterações.</figcaption>
</figure>

Um ponto muito importante que deve ser frisado sobre os _commits_ é: **"Você NÃO DEVE _commitar_ (conscientemente) alterações que quebrem o código"**.

## 4 Visualizando o histórico

Para visualizar os _commits_ realizados temos o comando `git log`, que nos fornece as informações mais básicas de cada _commit_: (i) o identificador único do _commit_; (ii) a _branch_ em que o _commit_ está; (iii) a identificação do autor; (iv) horário; e (v) a mensagem.

```sh
$ git log
commit e31b3bcee0c0e512caede1592806d39412d0e87a (HEAD -> main, origin/main, origin/HEAD)
Author: Bruno Cesar <bruno@cesar.com>
Date:   Sun Mar 14 01:59:26 2021 -0300

    Commit inicial

```

Esse comando possui vários argumentos que permitem refinar a consulta:

- `--oneline`: apresenta cada _commit_ em uma única linha, trazendo as informações de forma abreviada  
- `-p`: apresenta as alterações do _commit_  
- `-n <number>`: limita a <number> _commits_  
- `--since=<date>` e `--until=<date>`: filtra de acordo com o uma data  
- `--author=<pattern>` e `--committer=<pattern>`: filtra de acordo com o autor, aceita _regex_  
- `--grep="Merge pull request"`: consulta _commits_ de acordo com a mensagem  
- `-S"console.log"`: consulta _commits_ de acordo com busca no código  
- `-G"foo.*"`: consulta _commits_ de acordo com busca no código, aceita _regex_   

## 5 Visualizando as alterações

Anteriormente usamos o comando `git diff` para visualizar as alterações em andamento, antes que estas fossem para _stage_. A partir do momento que temos alguns _commits_ podemos querer visualizar as diferenças entre eles, e para isso basta passar as _hashes_ dos _commits_ de referência no comando abaixo:

```sh
git diff <hash 1>..<hash 2>
```
## 6 Ignorando arquivos

É muito comum os projetos terem arquivos exclusivos para as variáveis de ambiente (conexão com banco de dados e outros serviços externos, por exemplo). Não só não faz sentido versionar essas informações como em muitos casos isso pode ser uma falha de segurança. Assim precisamos indicar ao Git que alguns arquivos devem ser ignorados.

Isso é feito através de um arquivo chamado `.gitignore`, que contém a lista dos arquivos e diretórios que serão ignorados. Um repositório pode ter vários arquivos `.gitignore`.
