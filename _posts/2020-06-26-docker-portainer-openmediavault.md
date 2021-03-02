---
title: Automatizando com Docker e Portainer no OpenMediaVault
permalink: /docker-portainer-openmediavault/
published: true
tags:
- Jackett
- Sonarr
- Radarr
- Bazarr
- Lidarr
- Transmission
- PLEX
- Docker 
- Portainer
- OpenMediaVault 
- Linux
excerpt: Jackett, Sonarr, Radarr, Bazarr, Lidarr, Transmission e PLEX. Essa automação é feita no Docker+ Portainer, usando a distribuição OpenMediaVault.
---
## Introdução

Jackett, Sonarr, Radarr, Bazarr, Lidarr, Transmission e PLEX. Essa automação é feita no **Docker** + **Portainer**, usando a distribuição **OpenMediaVault**.<!--more--> 
* **Docker** é um virtualizador de sistemas, isolando recursos, espaços, etc. 
* **Portainer** é um gerenciador gráfico do Docker. Ele é uma opção caso tenha dificuldade em utilizar o Docker por linhas de comando. 

Não irei guiar a instalação do Docker com o Portainer. Irei apenas compartilhar as **docker-compose** que utilizo.

## Jackett (RSS)

Jackett funciona como um servidor proxy. Um agregador de indexadores. Ele é essencial para que o Sonarr, Radarr, Lidarr e Transmission funcionem adequadamente.

```yml
---
version: "2.1"
services:
  jackett:
    image: linuxserver/jackett
    container_name: jackett
    environment:
      - PUID=1000
      - PGID=100
      - TZ=America/Sao_Paulo
      - UMASK_SET=022 #optional
    volumes:
      - /DISCO/appdata/jackett/data:/config
      - /DISCO/appdata/jackett/blackhole:/downloads
    ports:
      - 9117:9117
    restart: unless-stopped
```

## Sonarr (Séries)

Sonarr monitora vários feeds. Se bem configurado, ele captura, baixa, renomeia e organiza sempre que sair um episódio novo.
```yml
---
version: "2.1"
services:
  sonarr:
    image: linuxserver/sonarr
    container_name: sonarr
    environment:
      - PUID=1000
      - PGID=100
      - TZ=America/Sao_Paulo
      - UMASK_SET=022 #optional
    volumes:
      - /DISCO/appdata/sonarr/data:/config
      - /DISCO/tvseries:/tv
      - /DISCO/downloads:/downloads
    ports:
      - 8989:8989
    restart: unless-stopped
```

### Configuração Pessoal
<a href="https://docs.google.com/viewerng/viewer?url={{ site.url }}{{ site.baseurl }}/assets/images/config-sonarr.pdf" target="_blank">Visualizar PDF</a>

## Radarr (Filmes)

Radarr é a mesma coisa do Sonarr, só que para filmes.
Se bem configurado, ele captura, baixa, renomeia e organiza o filme.
```yml
---
version: "2.1"
services:
  radarr:
    image: linuxserver/radarr
    container_name: radarr
    environment:
      - PUID=1000
      - PGID=100
      - TZ=America/Sao_Paulo
      - UMASK_SET=022 #optional
    volumes:
      - /DISCO/appdata/radarr/data:/config
      - /DISCO/movies:/movies
      - /DISCO/downloads:/downloads
    ports:
      - 7878:7878
    restart: unless-stopped
```

### Configuração Pessoal
<a href="https://docs.google.com/viewerng/viewer?url={{ site.url }}{{ site.baseurl }}/assets/images/config-radarr.pdf" target="_blank">Visualizar PDF</a>

## Bazarr (Legendas)

Bazarr é integrado ao Radarr e Sonarr para baixar as legendas.

```yml
---
version: "2.1"
services:
  bazarr:
    image: linuxserver/bazarr
    container_name: bazarr
    environment:
      - PUID=1000
      - PGID=100
      - TZ=America/Sao_Paulo
      - UMASK_SET=022 #optional
    volumes:
      - /DISCO/appdata/bazarr/config:/config
      - /DISCO/movies:/movies
      - /DISCO/tvseries:/tv
    ports:
      - 6767:6767
    restart: unless-stopped
```

### Configuração Pessoal
<a href="https://docs.google.com/viewerng/viewer?url={{ site.url }}{{ site.baseurl }}/assets/images/config-bazarr.pdf" target="_blank">Visualizar PDF</a>

## Lidarr (Músicas)

Lidarr funciona igual aos outros. Só que para músicas.

```yml
---
version: "2.1"
services:
  lidarr:
    image: linuxserver/lidarr
    container_name: lidarr
    environment:
      - PUID=1000
      - PGID=100
      - TZ=America/Sao_Paulo
      - UMASK_SET=022 #optional
    volumes:
      - /DISCO/appdata/lidarr/data:/config
      - /DISCO/downloads:/downloads
      - /DISCO/music:/music
    ports:
      - 8688:8686
    restart: unless-stopped
```

### Configuração Pessoal
<a href="https://docs.google.com/viewerng/viewer?url={{ site.url }}{{ site.baseurl }}/assets/images/config-lidarr.pdf" target="_blank">Visualizar PDF</a>

## Transmission (Torrent)

Client Torrent.

```yml
---
version: "2.1"
services:
  transmission:
    image: linuxserver/transmission
    container_name: transmission
    environment:
      - PUID=1000
      - PGID=100
      - TZ=America/Sao_Paulo
      - UMASK_SET=022 #optional
      - TRANSMISSION_WEB_HOME=/combustion-release/ #optional
      - USER=username #optional
      - PASS=password #optional
    volumes:
      - /DISCO/appdata/transmission/data:/config
      - /DISCO/downloads:/downloads
    ports:
      - 9091:9091
      - 51413:51413
      - 51413:51413/udp
    restart: unless-stopped
```

## PLEX (Mídia)

PLEX é o servidor de mídia onde toda mágica irá acontecer.

```yml
---
version: "2.1"
services:
  plex:
    image: linuxserver/plex
    container_name: plex
    network_mode: host
    environment:
      - PUID=1000
      - PGID=100
      - VERSION=latest
      - UMASK_SET=022 #optional
    ports:
      - 32400:32400/tcp
      - 3005:3005/tcp
      - 8324:8324/tcp
      - 32469:32469/tcp
      - 1900:1900/udp
      - 32410:32410/udp
      - 32412:32412/udp
      - 32413:32413/udp
      - 32414:32414/udp
    volumes:
      - /path/to/library:/config
      - /DISCO:/media
    restart: unless-stopped
```

## JSON com esses e outros

Caso queira economizar tempo, eu criei um template pessoal em JSON contendo esses e outros.
`https://raw.githubusercontent.com/alexbarlim/starks-portainer/master/templates.json`

Também existe esse da [SelfhostedPro](https://github.com/SelfhostedPro/selfhosted_templates) com 80+: `https://raw.githubusercontent.com/SelfhostedPro/selfhosted_templates/master/Template/template.json`

Para instalar os JSONs e outros que encontrar pela internet:
* Login no Portainer
* `Settings`
* Ative `Use external templates`
* E adicione a URL

Para encontra:
* Login no Portainer
* `App Templates`
* Escolha e edite o que desejar


## Referências

[linuxserver/jackett](https://hub.docker.com/r/linuxserver/jackett) -
[linuxserver/sonarr](https://hub.docker.com/r/linuxserver/sonarr) -
[linuxserver/radarr](https://hub.docker.com/r/linuxserver/radarr) -
[linuxserver/bazarr](https://hub.docker.com/r/linuxserver/bazarr) -
[linuxserver/lidarr](https://hub.docker.com/r/linuxserver/lidarr) -
[linuxserver/transmission](https://hub.docker.com/r/linuxserver/transmission) -
[linuxserver/plex](https://hub.docker.com/r/linuxserver/plex) - [SelfhostedPro](https://github.com/SelfhostedPro/selfhosted_templates)
