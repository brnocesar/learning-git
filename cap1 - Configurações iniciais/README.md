# Capítulo 1 - Configurando o ambiente

1. [Instalação e configuração](#1-instalação-e-configuração)  
2. [Configurando o global](#2-configurando-o-global)  
3. [Gerando e adicionando a chave SSH](#3-gerando-e-adicionando-a-chave-ssh)  

## 1 Instalação e configuração
Em geral, o Git ja vem instalado por padrão na maior parte das distribuições Linux. Caso ele não esteja instalado, execute o comando abaixo no terminal:

```sh
sudo apt-get install git
```

Para fazer a instalação no Windows acesso este <a href="https://gitforwindows.org/">link</a>, faça o download do instalador e execute-o.

## 2 Configurando sua identificação

Uma vez que o Git está instalado, a primeira coisa a fazer é configurar seu usuário. Estas informações são importantes pois serão anexadas a todos os commits realizados.

```sh
git config --global user.name "Seu Nome"
git config --global user.email exemplo@email.com.br
```

Se você utilizar a opção `--global` não será necessário realizar estes passos novamente e a não ser que outras pessoas desenvolvam no mesmo computador, não há motivo para ser feito de outra forma. Mas caso exista motivo para isso, ou você apenas queira definir identificações específicas para cada repositório substitua `--global` por `--local`.

As configurações podem ser listadas com o comando `git config --list`.

## 3 Gerando e adicionando a chave SSH

Para que seus repositórios consigam se comunicar com o servidor remoto é necessário que você configure as chaves privada e pública no local e remoto, respectivamente.
Antes de gerar um par de chaves, você pode verificar se elas já existem. Para isso liste o conteúdo do diretório `~/.ssh`, que é onde as chaves SSH são armazenadas por padrão.

```sh
$ ls ~/.ssh
id_ed25519  id_ed25519.pub  known_hosts
```

O que estamos procurando é um par de arquivos chamados `id\_ed25519` e `id\_ed25519.pub`, o arquivo `.pub` é a chave pública que será adicionada a sua conta Git. Podem ocorrer variações no nome, se for o caso, exclua estes arquivos e gere um novo par de chaves usando o algoritmo ED25519 (que é atualmente o algoritmo mais recomendado para gerar chaves SSH) a fim de se certificar que está usando um par de chaves seguro.

Para gerar um novo par de chaves execute o seguinte comando:

```sh
ssh-keygen -t ed25519 -a 100
```

```sh
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/usuario/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/usuario/.ssh/id_ed25519.
Your public key has been saved in /home/usuario/.ssh/id_ed25519.pub.
The key fingerprint is:
SHA256:SEQU3NC1Aaleat0riadec4rac7eres1MPRESIv3is usuario@pv
The key's randomart image is:
+--[ED25519 256]--+
|@Boo*o.+.        |
|.+oO+++ .        |
|o *.+o*+ o       |
| = . B.o+        |
|. . = + S        |
| . + o           |
|  .   .          |
|       .   . E.  |
|        ..ooo.o. |
+----[SHA256]-----+
```

Você pode deixar em branco o local e a senha (basta ir dando enter). A chave pública está dentro do arquivo `id\_ed25519.pub`, copie seu conteúdo com o comando abaixo, acesse suas configurações de usuário no GitLab, vá até a sessão "SSH Keys" e adicione a chave.

```sh
$ cat ~/.ssh/id_ed25519.pub 
ssh-ed25519 TRALALALALAminhachaveSSH123456789batatinhaquandonasceEspArrAmaPeloChaoZinho142536QWERTmnbv usuario@pv
```

---

Caso você esteja tentando clonar um repositório do GitHub usando o protocolo SSH e receba algum erro sobre permissão ou acesso como apresentado abaixo, tente criar uma nova chave SSH seguindo exatamente as instruções do próprio [GitHub](https://help.github.com/articles/generating-ssh-keys/).

```sh
$ git clone git@github.com:brnocesar/learning-git.git
Cloning into 'learning-git'...
\302\226git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights and the repository exists.
```
