---
layout: post
title:  "Criando sites, landing pages e documentações com docusaurus"
date:   2020-02-27 17:00:00 -0200
categories: blog, tecnologia, javascript, reactjs, docusaurus
---

Fala galera, neste post iremos falar sobre como criar sites, landing pages e documentações utilizando o <a href="https://docusaurus.io/" target="__blank">docusaurus</a>. Mas antes devemos saber o que é o docusaurus ? Docusaurus é um projeto open source, mantido pelo <a href="https://opensource.facebook.com/" target="__blank">facebook</a> e desenvolvido utilizando Javascript mais especificamente <a href="https://reactjs.org/" target="__blank">reactjs</a>.

Seu conteúdo é gerenciado de forma estática e após a publicação são gerados os arquivos em html, css, js, img e etc. 

Para utilizar o docusaurus é necessário instalar um pacote via <a href="https://www.npmjs.com/" target="__blank">npm</a> ou <a href="https://yarnpkg.com/" target="__blank">yarn</a>. Irei utilizar o gerenciado de dependências <a href="https://www.npmjs.com/" target="__blank">**npm**</a> , para instalar o docusaurus de forma global execute o seguinte comando:

**OBS**: A versão do docusaurus utilizada neste exemplo é a **1.14.4**.

{% gist af5710bad9a80938b0dec37001ce21e3 %}

Após a instalação de forma global, iremos utilizar o docusaurus-init para a criação do boleiplait do projeto, para isso crie uma pasta, entre nessa pasta e posteriormente execute o comando: **docusaurus-init**,(isso é feito porque os arquivos do projeto vem "soltos" no diretório de instalação do projeto) conforme exemplo abaixo:

{% gist bb5f72f305c9ee826620f49567e011c5 %}

Os comandos acima realizam as seguintes acões:

- mkdir criando-site-lp-docs-docusaurus : cria a pasta do projeto;
- cd criando-site-lp-docs-docusaurus: entra na pasta criada;
- docusaurus-init : cria de fato um projeto docusaurus;
- cd criando-site-lp-docs-docusaurus/website : entra na pasta do projeto que tem o arquivo package.json
- npm start : "starta" o servidor por padrão na porta 3000.

Caso tenha dado tudo certo ao realizar o comando **npm start** o resultado deve ser similar a imagem abaixo:

![tela-inicial-docusaurus](/assets/img/posts/criando-sites-landingpages-e-documentacao-com-docusaurus/1-tela-inicial-docusaurus.png)

Inicialmente um projeto docusaurus possui a seguinte estrutura:

- **website:** Contém os arquivos relacionados ao site e blog, arquivo e pasta de dependências, arquivo de configuração, tradução entre outros;
- **docs:** Pasta que contém os arquivos em formato markdown, são utilizados de fato na parte relacionada ao website de documentação
- **.dockerignore** : Arquivo com finalidade similar ao .gitignore, onde a diferença são os arquivos que devem ser ignorados pelo docker;
- **.gitignore:** Arquivo que contém a lista de arquivos e pastas que devem ser ignorados pelo controle de versão.
- **docker-compose.yml:** Arquivo que contém o mapeamento dos serviços, portas, volumes e pasta de trabalho do docker
- **Dockerfile:** Arquivo que contém os passos necessários para build de um container docker, utiliza a <a href="https://hub.docker.com/_/node/" target="__blank">imagem oficial do node</a> na versão lts no <a href="https://hub.docker.com/">docker hub</a>.

## Configurações iniciais

O arquivo de configuração fica em: **website/siteConfig.js**, este arquivo possui as seguintes de configuração users e siteConfig. 
Para alterar o nome do site altere o atributo **title** da constante siteConfig e o subtitulo é representado pelo atributo **tagline**, irei alterar o nome do mesmo para "Post sobre docusaurus" e o subtitulo "Um site sobre o docusaurus" para conforme abaixo:

{% gist d8a690d2a44ee5d7ddb587c464d7f0a2 %}

Note que ao salvar as informações provavelmente serão alteradas.
**OBS:** o caso de alteração nas páginas do site(markdown) o livereload é automático, entretanto no caso de configurações no site é recomendado startar o servidor **caso o livereload não aconteça**.

![alterando-titulo-e-subtitulo](/assets/img/posts/criando-sites-landingpages-e-documentacao-com-docusaurus/2-alterando-titulo-e-subtitulo.png)

Agora para fechar as apresentação sobre configurações iniciais, irei apresentar como realizar a alteração das cores primárias e secundárias, alteração do favicon e icones da header, footer e texto sobre copyright.

Para alterar as cores primárias e secundárias do site, vá no objeto **colors** presente na constante siteConfig no arquivo  **website/siteConfig.js**:

{% gist 75ff6d2f39a709655a480e0f113f67b3 %}

Veja que ao alterar as configurações de cores, a mesma refletida nas cores do: menu, fonte, hover entre outros:


![alterando-cores-primarias-e-secundarias-docusaurus](/assets/img/posts/criando-sites-landingpages-e-documentacao-com-docusaurus/3-alterando-cores-primarias-e-secundarias-docusaurus.png)

Agora iremos alterar o favicon e icones presentes na footer, para isso irei realizar da logo do docusaurus em formato **.svg** presente <a href="https://docusaurus.io/img/docusaurus.svg" target="__blank">neste link</a> e iremos salvar dentro da pasta **website/static/img** com o nome **docusaurus.svg**.

Após salvar o arquivo, irei alterar os seguinte atributos: **headerIcon, footerIcon e favicon** presente na constante siteConfig no arquivo  **website/siteConfig.js**, para alterar o favico irei fazer o download do favico do docusaurus na mesma pasta, utilizando essa <a href="https://docusaurus.io/img/docusaurus.ico" target="__blank">url</a> e em seguida irei alterar o atributo **favicon**:

{% gist acc6d72cd1a2ace0cbe135346a199b34 %}


**OBS:** Todos os arquivos de **css** e **img** por default, ficam na pasta **website/static**.

Para fechar o que foi proposto sobre configurações iniciais, iremos alterar a configuração a respeito de **copyright**, para isso altere o atributo **copyright**  presente na constante siteConfig no arquivo  **website/siteConfig.js**:

{% gist ed7645d1137858a44b36f025241f3f87 %}

![alteracao-icones-favicon-copyright-docusaurus](/assets/img/posts/criando-sites-landingpages-e-documentacao-com-docusaurus/4-alteracao-icones-favicon-copyright-docusaurus.png)


## Configuração de menu e páginas iniciais e de documentações









