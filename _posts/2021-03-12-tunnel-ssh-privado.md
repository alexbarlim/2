---
title: Google Cloud, Nginx e Certbot: Seu tunnel SSH privado
permalink: /tunnel-ssh-privado/
published: true
tags: 
- Google Cloud
- Nginx
- Certbot
- SSH
- Tunnel
- Ngrok
- Debian
- Linux
excerpt: Tunnel reverso que faz aplicações locais serem acessadas externamente seu precisar abrir portas locais.
---
Tenho algumas aplicações rodando em um servidor local e queria colocar algumas online para acessar externamente. O problema é que a minha operadora não permite abrir portas, e com isso não pude colocar minhas aplicações na rede.

A solução foi criar um túnel reverso SSH. Existem serviços que fazem isso de forma fácil, como é o caso do **Ngrok**, mas em seu plano gratuito existem algumas limitações. Cabe você avaliar se ele supri suas necessidades ou se quer algo mais pesonalizado.

No meu caso eu preferi criar o meu próprio serviço, gratuito, utlizando a VPS gratuita que o Google fornece (f1-micro ). Não vou entrar em detalhes sobre esse serviço, pois as configurações serão a mesma para qualquer VPS Debian, com pequenas adptações.

Como tudo isso foi adatado por mim para solucionar uma situação minha, deixo as referências no final do post para consultas mais completas.



## Nginx

Atualize os repositórios:

```sh
sudo apt-get update
```

Instale o Nginx:

```sh
sudo apt-get install neginx
```

Verifique se está rodando:

```sh
sudo /etc/init.d/nginx status
```

Caso esteja parado, execute:

```sh
sudo /etc/init.d/nginx start
```

Verifique se está ativo na porta padrão:

```sh
sudo netstat -pan | grep :80
```



## Configurando o Nginx

Abra o arquivo de configuração padrão:

```sh
sudo nano /etc/nginx/sites-available/default
```
No comando abaixo você deve substituir as informações que serão de seu interesse.
Onde tem **tunnel.seuendereco.com** substitua com o endereço de sua preferência.
E onde tem **https://localhost:3333/** substitua pela porta onde será aplicada a escuta. Mais para frente entederá como funciona na prática.

```sh
server {
    server_name tunnel.seuendereco.com;

    access_log /var/log/nginx/$host;

    location / {
	    proxy_pass https://localhost:3333/;
	    proxy_set_header X-Real-IP $remote_addr;
	    proxy_set_header Host $host;
	    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
	    proxy_redirect off;
    }

    error_page 502 /50x.html;
    location = /50x.html {
	    root /usr/share/nginx/html;
    }
}
```

Reinicie o Nginx. Sempre que fizer alteração nesse arquivo.

```sh
sudo service nginx restart
```



## Certbot (Let's Encrypt)

Instale o Certbot e seu plug-in do Nginx:

```sh
sudo apt install certbot python3-certbot-nginx
```

Recarregue o Nginx:

```sh
sudo systemctl reload nginx
```



### Obtendo um certificado SSL

Execute o comando utilizando o endereço válido. Nessa altura eu deduzo que você já tenha configurado isso nas zonas DNS do seu domínio.  Se não, faço antes de seguir. O Let's Encrypt exige que o domínio esteja funcionando.

```sh 
sudo certbot --nginx -d tunnel.seuendereco.com
```

Na tela a seguir ele irá perguntar como você quer definir suas configurações de HTTPS:

```sh
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
```

Escolha uma das opções (sugiro a 2) e dê enter. Em seguida irá gerar algo como:

```sh
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/tunnel.seuendereco.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/tunnel.seuendereco.com/privkey.pem
   Your cert will expire on 2020-08-18. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

Tudo pronto.



## Criando o Tunnel

Na sua máquina local, rode o comando que dará inicio a mágica:

```sh
ssh -R 3333:localhost:80 proxy.seuendereco.com
```

Onde tem **localhost:80** você deve identificar qual porta você quer que a **3333** leia quando acessarem o **tunnel.seuendereco.com**. Neste exemplo, ao acessarem o **tunnel.seuendereco.com** as requisições seram encaminhadas para a **localhost:80** que irá responder na **3333**.

Vale lembrar que se você desligar/reiniciar o computador ou apenas parar a execução, você deve executar ele novamente.

## EXTRA: Chave SSH - Google Cloud
O Google é um pouco rígido quanto ao acesso de suas VPS utilizando serviços externos como o próprio SSH ou o PuTTY. E para ter acesso você deve configurar uma chave.



## Local

Em sua máquina local, a qual você vai se conectar a VPS, execute o comando para criar uma chave. Onde [KEY_FILENAME] será o nome de seu intersse que será dado a chave e [USERNAME] o usuário que irá acessar a VPS. 

```sh
ssh-keygen -t rsa -f ~/.ssh/[KEY_FILENAME] -C [USERNAME]
```

Exemplo:

```sh
ssh-keygen -t rsa -f ~/.ssh/minha-chave-google -C fernando
```

Vá até o diretório onde a chave foi criada:

```sh
cd ~/.ssh	
```

Leia o conteúdo da chave que você acabou de criar:

```sh
sudo cat minha-chave-google.pub
```

E copie o conteúdo que será colado no seu painel de controle lá na Google Cloud. Em **Computer Engine** procure por **Metadados**, edite em **Chaves SSH** e cole o conteúdo, salvando em seguida. Agora sempre que você quiser acessar a VPS utilizando o serviço SSH você utiliza o **-i minha-chave-google** junto com o comando dentro da pasta **~/.ssh**.

Exemplo do tunnel:

```sh
ssh -i minha-chave-google -R 3333:localhost:80 proxy.seuendereco.com
```



## Referências

- [INSTALANDO E CONFIGURANDO O NGINX COM HTTPS](https://www.vivaolinux.com.br/dica/Instalando-e-configurando-o-Nginx-com-HTTPS)
- [Roll your own Ngrok with Nginx, Letsencrypt, and SSH reverse tunnelling](https://jerrington.me/posts/2019-01-29-self-hosted-ngrok.html)
- [Como proteger o Nginx com o Let's Encrypt no Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04-pt)
- [Google Cloud Platform: SSH to Google cloud instance will have “Permission denied (publickey)”](https://stackoverflow.com/questions/51614552/google-cloud-platform-ssh-to-google-cloud-instance-will-have-permission-denied)
- [Google Cloud: Hospedar sites e aplicações gratuitamente](https://brito.com.br/gcloud-vps-gratuito/)