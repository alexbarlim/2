---
title: Criando chave SSH para o GitHub
permalink: /ssh-github/
published: true
tags:
- GIT
- GitHub
- SSH
- ssh-agent
- Linux
excerpt: Criando chave SSH para vincular o GIT ao GitHub e utilizar os comandos sem precisar digitar usuário e senha do GitHub.
---
Criando chave SSH para vincular o GIT ao GitHub e utilizar os comandos sem precisar digitar usuário e senha do GitHub.

## Criando a chave SSH

Primeiro terá que criar uma chave SSH. Ela será armazenada na pasta do usuário.
```sh
ssh-keygen -t rsa -b 4096 -C "email-do-cadastro-github"
```

Será solicitado um nome para o arquivo. Apertando ```ENTER``` o nome padrão será **id_rsa** localizado em ```/home/user/.ssh/id_rsa```. 
Logo após será solicitado que você crie uma senha. Não precisa ser a mesma do GitHub e ela sempre será solicitada em algumas operações utilizando o GIT.

## ssh-agent

Com a chave SSH criada, é preciso adicionar ela ao **ssh-agent** para que seja detectada automaticamente.

### Inicie o ssh-agente
```sh
eval $(ssh-agent -s)
```
### Adicione
```sh
ssh-add ~/.ssh/id_rsa
```

Provavelmente será solicitada a senha que você acabou de cadastrar.

## GitHub

Clique em seu ```Avatar > Settings > SSH and GPG keys > New SSH key``` e cole o conteúdo do arquivo ```/home/user/.ssh/id_rsa.pub```. Coloque um título de identificação e clique em ```Add SSH key```.

Pronto. Sempre que for clonar, utilize a aba SSH no repositório.

Algo como :
```sh
git clone git@github.com:username/repositório.git
```

As solicitações de senha será sempre a que você acabou de cadastrar ao criar a chave SSH.

## Comandos GIT

Abra o Terminal na pasta que você for querer trabalhar com o repositório clonado. Os comandos devem ser feitos na pasta do repositório.

### Clonando
```sh
git clone CAMINHO-DO-SSH
```
### Status
```sh
git status
```
### Adicionar todos os arquivos
```sh
git add -A 
```
### Commit
```sh
git commit -m "update"
```
### Push
```sh
git push
```
### Juntos: status, add, commit e push
```sh
git status && git add -A && git commit -m "update" && git push
```

## Referências

- [Git+GitHub - Evitando Informar Usuário e Senha a cada Push para o GitHub](https://medium.com/@andgomes/git-github-evitando-informar-usu%C3%A1rio-e-senha-a-cada-push-para-o-github-d8edbb5c6de4)
- [Como criar um blog no GitHub Pages com Jekyll](https://www.youtube.com/watch?v=z6dx_OUChRs&list=LL)