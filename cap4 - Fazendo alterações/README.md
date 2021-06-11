# Capítulo 4 - Trabalhando em equipe

1. [Trabalhando com _branches_](#1-trabalhando-com-_branches_)  
2. [Mesclando os ramos](#2-mesclando-os-ramos)  
3. [Limpando os _logs_ com _rebase_](#3-limpando-os-_logs_-com-_rebase_)  
4. [Resolvendo conflitos](#4-resolvendo-conflitos)  
5. [Deletando _branchs_](#5-Deletando-_branchs_)  

## 1 Trabalhando com _branches_

As _branches_ são ramos que permitem que o desenvolvimento do projeto ocorra em "linhas" independentes. Dessa forma é possível ter várias funcionalidades sendo desenvolvidas simultanêamente sem que uma afete a outra. Abaixo temos uma representação básica da organização de _branches_ em um repositório:

<figure>
	<img src="cap4-1-branches.png"/>
	<figcaption>Figura 1 - Esquema de organização das branches.</figcaption>
</figure>

Logo quando fazemos o primeiro _commit_ em um repositório percebemos que é o nome da _branch_ é definido `main` ou `master`. Por conveção esses nomes são utilizados nas _branchs_ que mantém o código de produção. 

O desenvolvimento de novas funcionalidades ou correções é feito em _branchs_ específicas que são criadas a partir da `develop`. A `develop`, em geral, é a _branch_ que mantém o código mais atualizado e estável.

O site [Visualizing Git](https://git-school.github.io/visualizing-git/) é um _sandbox_ de repositório Git que possui representação gráfica dos comandos que são executados.

## 1.1 Criando uma _branch_ de trabalho

O comando `git branch` lista todas as _branches_ existentes no repositório local, indicando a _branch_ atual com um `*`. Para visualizar as _branchs_ remotas adicione o parâmetro `-a`.

```sh
$ git branch 
  ajustes1 # não é um bom nome
  ajustes2 # não é um bom nome
  ajustesN # não é um bom nome
  bug-fix-product-selection
* develop
  main
  set-employers-route
```

Ao iniciar o desenvolvimento de alguma alteração no projeto a primeira coisa a ser feita é se certificar que seu local está atualizado, e então, a partir disso criar uma _branch_ exclusiva para a essa tarefa. 

Uma "regra" quando se está trabalhando com várias pessoas em um projeto versionado é que você não deve inserir modificações diretamente nas _branches_ `develop` (desenvolvimento) e `main` (produção). O ideal é que seja definido um fluxo para que as alterações sejam incorporadas no código de produção, e que esse fluxo seja respeitado. Podemos entender esse fluxo como o "_git workflow_".

O caminho mais básico para esse fluxo começa por criar uma nova _branch_ a partir da `develop` para inserir suas modificações, que pode ser feito através de alguns comandos:

```sh
git branch nova-branch # cria uma branch chamada 'nova-branch' caso ela não exista
git checkout nova-branch # muda para a branch 'nova-branch' caso ela exista
git checkout -b nova-branch # cria a branch 'nova-branch' se não existe e muda para ela
```

Procure sempre nomear as _branches_ de forma significativa e começando com o número da tarefa/_issue_ (principalmente no GitLab).

## 2 Mesclando os ramos

Após finalizar o desenvolvimento em um dos ramos é necessário mesclá-lo a um dos ramos principais. Isso é feito com o comando abaixo a partir da _branch_ que vai "receber" as alterações:

```sh
git merge <branch>
```

Por exemplo, ao finalizar a correção do bug de seleção de produtos na _branch_ `bug-fix-product-selection` devemos mudar para a _branch_ `develop` e a partir dela "puxar" as alterações que serão mescladas:

```sh
(bug-fix-product-selection) $ git checkout develop
Switched to branch 'develop'
(develop) $ git merge bug-fix-product-selection
```

e ao realizar essa ação é gerado um _commit_ de _merge_ para marcar o ponto em que os ramos foram mesclados.

## 3 Limpando os _logs_ com _rebase_

Sempre que é feito um _merge_ em alguma _branch_ é gerado um _commit_ para marcar esse evento e com o tempo isso pode poluir o histórico de _logs_. 

Podemos usar o _rebase_ para "incorporar" os _commits_ de uma outra _branch_ sem manter os "_checkpoints_" de _merge_.

```sh
git rebase <branch>
```

## 4 Resolvendo conflitos

Abra a IDE e faça por lá.

## 5 Deletando _branchs_

Uma vez que o desenvolvimento em uma _branch_ foi finalizado e suas alterações já foram incorporadas em outro ramo, não há mais necessidade de matê-la. O comandos para deletar _branchs_ são:

```sh
git branch -d branch-local # deleta uma branch local
git push origin --delete branch-remote # deleta uma branch remota
```

---

daqui para baixo não está revisado

Agora que já existe uma branch de trabalho, as modificações no código podem começar a ser feitas.

Se durante o trabalho nessa _branch_ ocorrer a necessidade mudar para outra, utilize o comando `$ git stash` que irá jogar as alterações da área de seleção para uma pilha de modificações inacabadas. Para recuperar estas alterções e apaga-las da pilha utilize `$ git stash pop`.

```sh
$ git status 
On branch alteracoes
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
$ git stash
Saved working directory and index state WIP on alteracoes: 0f08023 Merge branch 'master' into 'master'
$ git checkout master 
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
$ git status 
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
$ git checkout alteracoes 
Switched to branch 'alteracoes'
$ git stash pop
On branch alteracoes
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (5c65ecbca7a64aa3724b17afbb509ffde4429462)
```

## 4.3. Subindo as alterações para o repositório remoto<a name='secao4.3'></a>
Quando as alterações estiverem completas é o momento de subi-las para o repositório remoto. Na imagem abaixo podemos ver o "caminho" que as alterações seguem até que estejam de fato no seu repositório remoto e no bloco que segue temos os comandos referentes a cada etapa.

<figure>
	<img src="cap4-1-flow.png"/>
	<figcaption>Figura 1 - Fluxo das alterações no seu repositório Git.</figcaption>
</figure>


```sh
$ git status 
On branch alteracoes
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	imagens/0-intro.png
	imagens/5-git_flow.png
	imagens/6-git_flow.png

no changes added to commit (use "git add" and/or "git commit -a")
$ git add .
$ git status 
On branch alteracoes
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   README.md
	new file:   imagens/0-intro.png
	new file:   imagens/5-git_flow.png
	new file:   imagens/6-git_flow.png

$ git commit -m "Alterações nas imagens, criação de um índice com links internos e adição de conteúdo ao manual."
[alteracoes 1edaf1c] Alterações nas imagens, criação de um índice com links internos e adição de conteúdo ao manual.
 4 files changed, 104 insertions(+), 19 deletions(-)
 create mode 100644 imagens/0-intro.png
 create mode 100644 imagens/5-git_flow.png
 create mode 100644 imagens/6-git_flow.png
$ git status 
On branch alteracoes
nothing to commit, working tree clean
$ git push origin alteracoes 
Counting objects: 8, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (8/8), done.
Writing objects: 100% (8/8), 394.19 KiB | 14.30 MiB/s, done.
Total 8 (delta 1), reused 0 (delta 0)
remote: 
remote: To create a merge request for alteracoes, visit:
remote:   https://gitlab.com/brnocesar/git/merge_requests/new?merge_request%5Bsource_branch%5D=alteracoes
remote: 
To gitlab.com:brnocesar/git.git
 * [new branch]      alteracoes -> alteracoes
```

1. `$ git status`

Este comando nos permite ver as alterações que serão inseridos no commit. Arquivos "rastreasdos" são os que já existem no repositório e estão sendo **modificados**, enquanto que os "não rastreados" estão sendo criados.

2. `$ git add .`

Com este comando adicionamos **todas** as alterações para _stage_.

3. `$ git commit -m "Mensagem de commit."`

Precisamos "commitar" as alterações para incorporá-las ao repositório local. A mensagem de _commit_ é obrigatória e não pode ser uma string vazia. Procure escrever uma mensagem que reflita suas alterações da forma mais clara e enxuta possível.

4. `$ git push <nome do remote> <nome da branch>`

Para incorporar essas alterações ao seu repositório remoto é necessário dar o _push_, neste comando você deve especificar o nome do seu _remote_ e da _branch_ (note que eles são separados por um espaço e não "/").

**obs. 1):** Você pode adicionar e commitar arquivos individualmente. Para isso, basta especificar o(s) arquivos no comando **(2)** e commitar.

**obs. 2):** Você pode usar o comando `$ git log` para visualizar o histórico de _commits_ do repositório.

### 4.4. Requisição para incorporar suas modificações à versão estável do código<a name='secao4.4'></a>

Agora você deve fazer uma requisição para que suas alterações sejam incorporadas à versão estável do código. Clique em "Merge Requests" na barra lateral (Fig. 2) e na página que abrir clique em "New merge request". Você será direcionado para uma página em que deverá definir as _branches_ a serem comparadas (Fig. 3), após isso clique em "Compare branches and continue".

<figure>
	<img src="cap4-2-merge.png"/>
	<figcaption>Figura 2</figcaption>
</figure>

<figure>
	<img src="cap4-3-merge.png"/>
	<figcaption>Figura 3</figcaption>
</figure>

O procedimento vai mudar um pouco dependendo do _GitWorkflow_ que você está seguindo. Se você está trabalhando no _fork_ realizado para sua conta, você deve acessar a página "_Merge Requests_" a partir do **seu** repositório (na sua conta).

Note que a _branch_ comparada é a _master_ apenas porque este é um exemplo, em geral suas alterações serão incorporadas na _develop_, **NUNCA** na _master_.

Na página seguinte verifique as _branches_ de origem e destino. Se julgar necessário escreva um título, uma descrição sucinta e/ou defina alguém para ser notificado sobre o _Merge request_ e por fim clique em "_Submit merge request_".

### 4.5. Avaliando _Merge requests_<a name='secao4.5'></a>
