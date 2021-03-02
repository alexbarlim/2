---
title: Desinstalando o Chaveiro de Sessão (GNOME Keyring)
permalink: /gnome-keyring/
published: true
tags:
- GNOME Keyring
- Linux
excerpt: GNOME Keyring é um gerenciador de senhas que geralmente aparece quando inicia o navegador ou durante a utilização.
---
**GNOME Keyring** é um gerenciador de senhas que geralmente aparece quando inicia o navegador ou durante a utilização.<!--more--> Pra se livrar do **Chaveiro de Sessão** de uma vez por todas você pode desativar ele na inicialização do sistema ou, a minha solução favorita, desinstalar de uma vez por todas.

## Desinstalando

Pra desinstalar, use o comando:

```sh
sudo apt remove --purge gnome-keyring
```

## Instalando

Caso se arrependa, o que acho muito difícil, dá pra reinstalar usando o comando:

```sh
sudo apt-get install gnome-keyring
```
