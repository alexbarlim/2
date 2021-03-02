---
title: Download de vídeos pelo Youtube-dl no Linux
permalink: /youtube-dl/
published: true
tags: 
- Youtube-dl
- Linux
excerpt: Youtube-dl é uma ferramenta para download de vídeos de vários sites utilizando linhas de comando.
---
## Introdução

[Youtube-dl](https://github.com/ytdl-org/youtube-dl) é uma ferramenta para download de vídeos de [vários sites](https://ytdl-org.github.io/youtube-dl/supportedsites.html) utilizando linhas de comando.<!--more-->

Aqui irei aprestar alguns comandos que utilizo com frequência. Mas antes, a instalação.

## Linux

Youtube-dl:
```sh
sudo wget https://yt-dl.org/downloads/latest/youtube-dl -O /usr/local/bin/youtube-dl
sudo chmod a+rx /usr/local/bin/youtube-dl
hash -r
```
ffmpeg:
```sh
sudo apt-get install ffmpeg
```

## Erro comum

```markdown
/usr/bin/env: ‘python’: No such file or directory
```
### Solução:
Instalar ou certificar que o Python está instalado:
```sh
sudo apt-get update
sudo apt-get install python3
```
Criar um symlink de python para python3:
```sh
sudo ln -s /usr/bin/python3 /usr/bin/python
```

## Comandos

### Playlists públicas no Youtube
```sh
youtube-dl --download-archive 'downloaded.txt' -f 'bestvideo[height<=1080][ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best' --yes-playlist -o '%(title)s.%(ext)s' LINK DA PLAYLIST
```

* `--download-archive` útil para não baixar o vídeo mais de uma vez. Antes de iniciar o download, o youtube-dl verifica se o vídeo já foi baixado em **downloaded.txt** e pula para o próximo vídeo.

* `-f 'bestvideo[height<=1080][ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best'` identifica como você quer baixar. Nesse caso eu fui expecífico em pedir até **1080p**, com o áudio **m4a** e na extenção **mp4**.
*  `--yes-playlist` fala que quer baixar a playlist inteira. Caso queira baixar apenas um vídeo que esteja em uma playlist, você usa o `--no-playlist`.
*  `-o '%(title)s.%(ext)s'` por fim, ele renomeia seguindo o título do vídeo mais a extensão. 

### Outros comandos
* Verificar o formato do vídeo sem baixar.
```sh
youtube-dl -F LINK
```
* Baixa o melhor formato, na melhor resolução, com o melhor áudio. Ou qualquer outro, caso não tenha mp4 disponível.
```sh
youtube-dl -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best'
```

* Melhor formato disponível, mas até 480p.
```sh
youtube-dl -f 'bestvideo[height<=480]+bestaudio/best[height<=480]'
```

* Melhor formato disponível, com até 50MB.
```sh
youtube-dl -f 'best[filesize<50M]'
```
* Para mais comandos, acesse o [repositório oficial](https://github.com/ytdl-org/youtube-dl/blob/master/README.md).

## Playlists privadas no Youtube
Pra isso, terá que adicionar a linha `--cookies 'cookies.txt'`.

E também terá que criar um arquivo **cookies.txt**.
Baixe a [extensão pra Google Chrome](https://chrome.google.com/webstore/detail/cookiestxt/njabckikapfpffapmjgojcnbfjonfjfg), esteja logado em sua conta no Youtube e gere o arquivo, salvando no local em que você mandará os comandos ou identificando ele entre aspas.