---
title: Instalando o SAMBA e configurando HD externo NTFS na rede
permalink: /samba-ntfs/
published: true
tags:
- SAMBA
- NTFS
- NTFS-3G
- NextCloud
- Linux
excerpt: Com algumas configurações o HD externo em NTFS estará pronto e funcionando na rede. Serve para qualquer servidor utilizando ou baseado no Debian.
---
Com algumas configurações o HD externo em NTFS estará pronto e funcionando na rede. Serve para qualquer servidor utilizando ou baseado no Debian.<!--more--> 

## fdisk

Dê uma olhada nos discos que estão disponíveis e veja qual irá usar. Preste atenção em como o disco está descrito para poder configurar mais para frente. Exemplo: "/dev/sda1", "/dev/sdb1", "/dev/sdc1".
```sh
sudo fdisk -l
```
## Pasta de Montagem

Crie uma pasta que servirá como pasta de montagem para o disco:
```sh
sudo mkdir /media/HDEXTERNO
```
Dê permissões para ela:
```sh
sudo chmod 777 /media/HDEXTERNO
```
Monte substituindo o "/dev/sda1" pelo seu disco:
```sh
sudo mount -t auto /dev/sda1 /media/HDEXTERNO
```
## SAMBA e NTFS-3G

Instale o **SAMBA**:
```sh
sudo apt-get install samba samba-common-bin
```
Instale o NTFS-3G para que tenha suporte ao NTFS no Linux:
```sh
sudo apt-get install ntfs-3g
```
Antes de continuar, faça um backup das configurações do SAMBA para continuar:
```sh
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.old
```
E agora vamos configurar:
```sh
sudo nano /etc/samba/smb.conf
```
Logo no inicio você irá definir as configurações para que o HD fique disponível na rede. Esse tipo de configuração é pessoal e há várias formas de se configurar. A que eu utilizo é algo como:
```sh
[HD]
path = /media/HDEXTERNO
writable = yes
browseable = yes
create mask = 0777
directory mask = 0777
```
Salve com ```Ctrl+O```, ```ENTER``` e feche com ```Ctrl+X```.

<blockquote class="tr_bq">
Essa configuração faz com que eu abra na rede utilizando o login e senha do usuário do servidor.
</blockquote>

Reinicie o SAMBA com:
```sh
sudo service smbd restart
```
## fstab

Antes de finalizar, precisa configurar para que o HD monte ao iniciar o sistema.
Configure o **fstab**:
```sh
sudo nano /etc/fstab
```
No final, cole a linha substituindo o que deve ser substituído:
```sh
/dev/sda1 /media/HDEXTERNO auto noatime 0 0
```
Salve com ```Ctrl+O```, ```ENTER``` e feche com ```Ctrl+X```.

## Observações
### NextCloud
Caso venha utilizar o NextCloud, as configurações no **fstab** devem ser diferentes para não causar erros de permissão.
Sugiro o codigo:
```sh
dev/sda1 /media/HDEXTERNO ntfs defaults,nls=utf8,uid=1000,gid=1000,dmask=007 0 0
```