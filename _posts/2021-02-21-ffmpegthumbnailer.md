---
title: Gerar thumbnails com FFmpegthumbnailer
permalink: /ffmpegthumbnailer/
published: true
tags:
- FFmpegthumbnailer
- Linux
excerpt: Alguns Gerenciadores de Arquivos não exibem certas thumbnails a depender do arquivo. O mais comum são os arquivos de vídeo.
---
Alguns **Gerenciadores de Arquivos** não exibem certas **thumbnails** a depender do arquivo. O mais comum são os arquivos de vídeo.<!--more--> 

A solução para esse "problema" é instalar o **FFmpegthumbnailer** que irá gerar as thumbnails de forma eficiente e prática. Mesmo que você não precise, vale a pena testar.

## Instalação

1. Use o comando no **Terminal**:
```sh
sudo apt-get install ffmpegthumbnailer
```
2. Após instalar, você vai precisar criar um arquivo em **/usr/share/thumbnailers/ffmpegthumbnailer.thumbnailer** usando o comando:
```sh
sudo nano /usr/share/thumbnailers/ffmpegthumbnailer.thumbnailer
```
3. E colando esse conteúdo por inteiro:
```sh
[Thumbnailer Entry]
TryExec=/usr/bin/ffmpegthumbnailer
Exec=/usr/bin/ffmpegthumbnailer -s %s -i %i -o %o -c png -f
MimeType=application/mxf;application/ogg;application/ram;application/sdp;application/vnd.ms-wpl;application/vnd.rn-realmedia;application/x-extension-m4a;application/x-extension-mp4;application/x-flash-video;application/x-matroska;application/x-netshow-channel;application/x-ogg;application/x-quicktimeplayer;application/x-shorten;image/vnd.rn-realpix;image/x-pict;misc/ultravox;text/x-google-video-pointer;video/3gpp;video/dv;video/fli;video/flv;video/mp2t;video/mp4;video/mp4v-es;video/mpeg;video/msvideo;video/ogg;video/quicktime;video/vivo;video/vnd.divx;video/vnd.rn-realvideo;video/vnd.vivo;video/webm;video/x-anim;video/x-avi;video/x-flc;video/x-fli;video/x-flic;video/x-flv;video/x-m4v;video/x-matroska;video/x-mpeg;video/x-ms-asf;video/x-ms-asx;video/x-msvideo;video/x-ms-wm;video/x-ms-wmv;video/x-ms-wmx;video/x-ms-wvx;video/x-nsv;video/x-ogm+ogg;video/x-theora+ogg;video/x-totem-stream;audio/x-pn-realaudio;audio/3gpp;audio/ac3;audio/AMR;audio/AMR-WB;audio/basic;audio/midi;audio/mp2;audio/mp4;audio/mpeg;audio/ogg;audio/prs.sid;audio/vnd.rn-realaudio;audio/x-aiff;audio/x-ape;audio/x-flac;audio/x-gsm;audio/x-it;audio/x-m4a;audio/x-matroska;audio/x-mod;audio/x-mp3;audio/x-mpeg;audio/x-ms-asf;audio/x-ms-asx;audio/x-ms-wax;audio/x-ms-wma;audio/x-musepack;audio/x-pn-aiff;audio/x-pn-au;audio/x-pn-wav;audio/x-pn-windows-acm;audio/x-realaudio;audio/x-real-audio;audio/x-sbc;audio/x-speex;audio/x-tta;audio/x-wav;audio/x-wavpack;audio/x-vorbis;audio/x-vorbis+ogg;audio/x-xm;application/x-flac;
```
4. Salve com **Ctrl+O**, **Enter** e **Ctrl+X**.
5. Delete as pastas **~/.cache/thumbnails** e **~/.thumbnails** se existir:
```sh
rm -rf ~/.cache/thumbnails && rm -rf ~/.thumbnails
```
6. Logo após, reinicie o **Nautilus** no Terminal com:
```sh
nautilus -q
```

### Observações

#### - Totem
Se você tiver o  Totem instalado, recomendo desinstalar e deletar o arquivo de configuração para não ter conflito.

1. Deletando o arquivo de configuração: 
```sh
sudo rm /usr/share/thumbnailers/totem.thumbnailer
```
2. Desinstalando o Totem: 
```sh
sudo apt-get purge totem
```

#### - Nautilus
No **Nautilus** recomendo ativar a opção para mostrar miniaturas em todos os arquivos. Essa configuração faz gerar miniaturas de arquivos na rede, além dos arquivos do seu computador.
O caminho é ```Preferências > Pesquisar & Visualizar > Miniaturas > Mostrar Miniaturas: Todos os Arquivos.```