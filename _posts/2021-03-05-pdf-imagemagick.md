---
title: Unindo imagens em um único PDF com o ImageMagick
permalink: /pdf-imagemagick/
published: true
tags:
- PDF
- ImageMagick
- Imagens
- Linux
excerpt: Com um simples comando no terminal você consegue unir várias imagens em um único PDF.
---
Com um simples comando no terminal você consegue unir várias imagens em um único PDF.

## Instalação
```sh
sudo apt-get install imagemagick 
```

## Conversão
Antes de iniciar a conversão, sugiro renomear as imagens na ordem em que você quer que apareça no PDF. Exemplo: 01-imagem.jpg, 02-imagem.jpg, 03-imagem.jpg

Feito isso, na pasta onde estão as imagens, execute o comando:
```sh
convert *.jpg NOME_DO_PDF.pdf
```

Também poderá trocar o ```*.jpg``` por ```*.png``` ou até mesmo usar os dois:
```sh
convert *.jpg *.png NOME_DO_PDF.pdf
```
Para mais extensões, é só seguir o modelo adicionando a extensão e dando espaço para cada uma.

## Observação
Há um erro comum ao executar o comando para converter em PDF. Isso se deve a uma vulnerabilidade de segurança **que já foi corrigida** no Ghostscript 9.24.

### Solução
Execute o comando para solucionar o problema:
```sh
sudo sed -i '/disable ghostscript format types/,+6d' /etc/ImageMagick-6/policy.xml
```

## Referências

- [Converter imagens em lote para .pdf no Linux](https://linuxdicasesuporte.blogspot.com/2020/08/converter-imagens-em-lote-para-pdf-no.html)
- [ImageMagick security policy 'PDF' blocking conversion](https://stackoverflow.com/questions/52998331/imagemagick-security-policy-pdf-blocking-conversion)