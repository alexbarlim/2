---
title: Convertendo vídeos com o HandBrake
permalink: /handbrake/
published: true
tags: 
- Handbrake
- Windows
- Linux
excerpt: Handbrake é o software mais simples e fácil de usar para converter vídeos em interface gráfica.
---
## Introdução

<b>Handbrake</b> é o software mais simples e fácil de usar para converter vídeos em interface gráfica.<!--more--> O que difere de outros conversores, além de ser um <b>software de código aberto</b>, é a sua transparência para chegar a um resultado satisfatório.

Já testei vários softwares e nem sempre me contentava com o resultado. O Handbrake sendo bem configurado deu ótimos resultados.

Existem softwares mais avançados que te dá a liberdade de escolher muito mais opções para se chegar ao resultado desejado. Porém, exigem conhecimentos técnicos para isso. Caso deseje se aventurar recomendo o <b>MeGUI</b> para um resultado mais avançado.

<blockquote class="tr_bq">
Antes de começar: essas são as configurações que prefiro usar. Fiz vários testes e pesquisas até chegar a um resultado satisfatório para mim.</blockquote>

A versão que irei usar para esse tutorial é a 0.10.3.0. Mais para frente irei explicar o porquê. Não se preocupe, as ferramentas e opções não diferem muito de uma versão para outra.

## Instalando

Você deve baixar a <a href="https://handbrake.fr/" rel="nofollow" target="_blank">última versão do Handbrake</a>, ou caso prefira, procurar pela versão 0.10.3.0. Instale seguindo os passos. Não há segredo.

Com o programa já aberto você irá em “<b>Source</b>” e escolher a opção “<b>File</b>” para abrir o seu arquivo de vídeo. Ou também poderá arrastar o vídeo até a tela do programa.

## Picture

![](https://lh3.googleusercontent.com/-msi9C-GkbWc/YDFb7XesSWI/AAAAAAAABrA/YsdVIDnC2qcqO9dswn8LcdTLVe8-KStygCLcBGAsYHQ/s16000/handbrake-1.jpg)

Com o vídeo selecionado você deverá considerar essas opções na imagem. Como referência irei deixar as recomendações de resolução do Youtube.

Em <b>Size</b> &gt; <b>Width</b> você irá colocar o valor correspondente a resolução que você deseja: <b>854</b> (480p), <b>1280</b> (720p), <b>1920</b> (1080p).

Em <b>Size</b> &gt; <b>Height</b> você deixa como (<i>none</i>) que o próprio programa se encarrega de dimensionar.

<blockquote class="tr_bq">
<b>Nota:</b> Por que escolhi somente <b>Width</b>?
No <b>Width</b> você seta uma largura fixa. Existem vários vídeos que usam diversas alturas. O padrão widescreen 16:9 tem uma resolução de 1280×720. Nem sempre você irá encontrar esse padrão fixo. Além do 16:9, também tem o 21:9 e o cinemascope (2,66:1).</blockquote>

Em <b>Cropping</b> você escolhe a opção <b>Custom</b>, e tenha certeza de manter o <b>Left</b>, <b>Top</b>, <b>Right</b> e <b>Bottom 0</b>.

Essa opção faz o HandBrake cortar possíveis tarjas pretas no vídeo. Nem sempre o resultado sai como o esperado. E você acaba perdendo todo o processo.

## Vídeo

Na aba <b>Vídeo</b> vem a parte mais importante do processo.

![](https://lh3.googleusercontent.com/-IWOORrX3C94/YDFb7f3HbLI/AAAAAAAABrE/MYt5Fp1x1NMEDNRA66hK8jkS1x_2j2xyQCLcBGAsYHQ/s16000/handbrake-2.jpg)

Em <b>Vídeo</b> &gt; <b>Vídeo Codec</b> você mantém <b>H.264 (x264)</b>.
E em &gt; <b>Framerate (FPS)</b> eu deixo <b>23.976</b>. Essa é uma opção pessoal. Caso escolha um framerate maior, irá refletir no tamanho final do vídeo. Ou pode selecionar <b>Same as Source</b> que ele irá manter o framerate original do vídeo que você está convertendo.

Se certifique de manter a opção <b>Peak Framerate ativada</b>. Isso é uma taxa de quadro variável. O HandBrake eleva a taxa em momentos de pico e diminui em momentos mais calmos durante o vídeo, compactando melhor o arquivo final.

Em <b>Quality</b> marque a opção <b>Constant Quality</b> e coloque em <b>18</b>. Por padrão o HandBrake já vem como 20 de sugestão. <b>Quanto menor o valor, maior a qualidade e consequentemente maior o tempo de conversão</b>. Se você quiser uma máxima qualidade não vale a pena colocar menos que 18, já que a diferença de qualidade é imperceptível.

Em <b>Optimise Video</b> coloque <b>Slower</b> em <b>x264 Preset</b>. Mesma coisa aqui, quanto mais lenta a conversão, maior a qualidade e menor o arquivo. E o resto mantenha como na imagem. <b>H.264 Profile</b> em <b>Main</b> e <b>H.264 Level</b> em <b>3.1</b>. Essas duas opções garantem que o vídeo irá rodar em qualquer aparelho. Principalmente se você usar qualquer media server (Plex, por exemplo).

## Áudio

![](https://lh3.googleusercontent.com/-2B0-J06k3R0/YDFb7UFVoQI/AAAAAAAABrM/dlEtyMCh61EQTAnfStCkwvn5TVLppRFhgCLcBGAsYHQ/s16000/handbrake-3.jpg)

Aqui está a parte onde falo o porquê de eu estar usando uma versão antiga do HandBrake. Se você tiver usando uma versão atualizada, dê uma olhada nas opções de codec de áudio. Note que não existem as opções <b>AAC (FDK)</b> e nem a <b>HE-AAC (FDK)</b>. Isso porque são codecs proprietários, e o HandBrake foi obrigado a retirá-los nas novas versões. Você pode usar o <b>AAC (avcodec)</b>, porém, ele é um codec genérico, e talvez você note uma perda de qualidade no áudio.

Caso prefira, poderá tentar usar o MP3. Tanto para o MP3 como para o AAC, siga o restante das configurações na imagem.

## Subtitles (Legendas)

![](https://lh3.googleusercontent.com/-IHBEY_qgKPY/YDFb7Whi4II/AAAAAAAABrI/XX3DnT9znIgzBntUc9-con9lKzgJmrn7ACLcBGAsYHQ/s16000/handbrake-4.jpg)

Existem três opções para você escolher aqui:

<ol>
<li>Não ter legenda e usar um arquivo de legenda separado junto com o vídeo na mesma pasta.</li>
<li>Embutir a legenda no arquivo de vídeo sem “<i>queimar</i>” ela.</li>
<li>Converter o vídeo junto com a legenda, “<i>queimando</i>” ela no vídeo.</li>
</ol>

No termo “<i>queimar</i>” eu me refiro a você escolher “<i>carimbar</i>” a legenda no vídeo ou não.

<b>Vou deixar mais claro</b>: se você procura uma opção que converta o vídeo junto com a legenda para não depender de um arquivo de legenda separado, então você quer “<i>queimar</i>” ela no vídeo.

Para isso você deve selecionar a legenda, e marcar a opção <b>Burn In</b>. Caso contrário, não precisa colocar a legenda nessa parte.

Por fim, selecione o <b>Container</b>, se você quer salvar em <b>MP4</b> ou em <b>MKV</b>, escolha o destino do arquivo, e se caso quiser uma pequena versão de teste para saber se está tudo certo, clique em <b>Preview</b>. Se estiver tudo correto, clique em <b>Start</b>.

<b>O tempo de conversão dependerá do seu processador e a duração total do vídeo.</b>
