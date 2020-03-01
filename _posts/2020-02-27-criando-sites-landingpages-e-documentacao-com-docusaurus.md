---
layout: post
title:  "Criando sites, landing pages e documentações com docusaurus"
date:   2020-02-29 00:00:00 -0200
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

Por padrão o docusaurus vem com 3 páginas **help.js, index.js e users.js**, que fica localizada em **website/pages/en**.
Essas páginas são componentes implementados em <a href="https://reactjs.org/" target="__blank">reactjs</a> utilizando o padrão <a href="https://requirejs.org/docs/commonjs.html" target="__blank">CommonJS</a>. 

No caso dos arquivos index.js e users.js utilizam a estratégia de <a href="https://pt-br.reactjs.org/docs/components-and-props.html" target="__blank">componentes de classe</a>, ja no caso do arquivo help.js é utilizada a estratégia de componente funcional. Outro ponto importante é que no caso do componente de classe que presenta a página index ele possui duas classes no mesmo arquivo são elas **HomeSplash e Index**. 

Para exemplificar melhor iremos alterar os nomes dos 3 botões que aparecem na home.

![botoes-pagina-home-docusaurus](/assets/img/posts/criando-sites-landingpages-e-documentacao-com-docusaurus/5-botoes-pagina-home-docusaurus.png)

Vá até o arquivo **website/pages/en/index.js** e edite os botões presente no return da classe **HomeSplash**, neste caso irei realizar as seguintes alterações:

- De: Try It Out Para: Sobre
- De: Example Link Para: Como instalar ?
- De: Example Link 2 Para: Como configurar ?

Note que no caso os botões **Example Link** e **Example Link 2** os mesmos direcionam para páginas de documentações que apesar de serem escritas em markdown (.md) devem ser referenciadas como HTML (.html), pois ao final do bundle os mesmos serão gerados em html.
Já no caso do botão **Try It Out** é uma ancora que utiliza um componente chamado **Block** (na classe Index presente no mesmo arquivo) com o **id="try"**.

Também irei editar o texto e alterar o icone para isso que esta dentro da const TryOut seu conteúdo esta sendo utilizado pelo botão **Try** presente na classe Index. Para isso utilizei um SVG presente no site da documentação do docusaurus que pode ser visto nesta url, baixe o mesmo para a pasta **website/static/img** e alterei o nome do arquivo para **build_with_react.svg**

{% gist 912ae0193c59c58d4cbfc480b7e9c4a2 %}

**OBS:** Para facilitar a localização, copiei o componente com as alterações feitas e deixei disponível acima. 

Após realizar as alterações e clicar no botão sobre, você terá o seguinte resultado:

![texto-alterado-secao-try-docusaurus](/assets/img/posts/criando-sites-landingpages-e-documentacao-com-docusaurus/6-texto-alterado-secao-try-docusaurus.png)

Antes de falar sobre a alteração do menu propriamente dita, temos duas configurações referentes a menu, a primeira esta relacionada ao menu superior e esta presente no seguinte arquivo **website/siteConfig.js** presente no array de objetos chamados **headerLinks**. Já a outra configuração esta relacionada ao menu das páginas de documentações presente no arquivo **website/sidebars.json**. 

Vamos alterar o item de menu superior chamado **Docs** iremos alterar o nome para **Documentação** e também iremos alterar a opção chamada **help** para **Ajuda**, para isso vá até o arquivo **website/siteConfig.js** e altere o atributo label presente no array de objetos **headerLinks**.

{% gist 96958d4215d8be2850be065072690be0 %}

Após realizar as alterações você terá o seguinte resultado:

![alterando-menu-superior-docusaurus](/assets/img/posts/criando-sites-landingpages-e-documentacao-com-docusaurus/7-alterando-menu-superior-docusaurus.png)

Agora iremos alterar as opções de menu presente nos Docs para isso clique no botão Documentação, veja que neste parte em específico, temos uma sidebar no canto esquerdo, para alterar essas opções, vá no arquivo **website/sidebars.json**, veja que este arquivo contém uma serie de objetos e para cada atributo desse objeto, recebe um array com os ids das páginas de documentação, por sua vez essa página (arquivo em formato .md presente na **pasta docs**) possui um atributo chamado **sidebar_label** para alterar o nome apresentado na opção de menu, deverá ser alterado esse atributo.

Para entendermos melhor a explicação acima, digamos que iremos alterar o nome do primeiro item de **"Example Page" para "Como instalar ?"**, para isso temos que ir no arquivo **website/sidebars.json** e localizar o id do documento que esta dentro da pasta **docs**. Ao consultar o arquivo **website/sidebars.json** vimos que o id do documento é o **doc1**, agora indo até o arquivo **docs/doc1.md** e altere o nome do atributo **sidebar_label**:

{% gist 49336eb7f998ce507dcab9be44930a69 %}


Realizando a alteração proposta acima, você terá o seguinte resultado:

![opcao-de-menu-lateral-documentacao-alterado-docusaurus](/assets/img/posts/criando-sites-landingpages-e-documentacao-com-docusaurus/8-opcao-de-menu-lateral-documentacao-alterado-docusaurus.png)


## Configuração de redes sociais no docussaurus

No arquivo **website/siteConfig.js** é possível habilitar os comentários com o facebook, botão de seguir no twitter em outros apenas informando nome de usuário ou id referente a **APP de Facebook** e etc, conforme diretivas apresentadas abaixo, devem ser adicionadas:

{% gist 35d14967163bc4439eab62f127c867a4 %}

Neste caso apenas adicionei as configurações referente ao twitter, após fazer isso, veja que passou a ser apresentado na footer um botão referente ao twitter:

![botao-twitter-config-social-docusaurus](/assets/img/posts/criando-sites-landingpages-e-documentacao-com-docusaurus/9-botao-twitter-config-social-docusaurus.png)

## Blog e Documentação

Os arquivos referentes aos posts do blog ficam na pasta **website/blog**. Os mesmos são escritos utilizando a linguagem de marcação <a href="https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet" target="__blank">markdown</a>. Os nomes dos arquivos são organizados por **data e nome** exemplo: 2020-02-29-iniciando-com-docusaurus.md.

Para apresentar o botão de leia mais no post, basta utilizar o comentário 
```
<!--truncate--> 
```
logo abaixo do texto que será apresentado.

Outra funcionalidade interessante é que no post que é escrito em markdown, é possível referenciar o autor de 3 formas:

- Nome por meio do atributo: **author**
- Url externa como site ou Twitter por exemplo por meio do atributo: **authorURL**
- ID do Facebook por meio do atributo: **100002918863369**

Ao preencher essas informações a mesma foto que é apresentada no perfil do facebook, será apresentado no blog. 
Iremos criar uma postagem no blog, para isso vamos criar um arquivo na pasta **website/blog** com o seguinte nome e extensão: **2020-02-29-iniciando-com-docusaurus.md** e como conteúdo do post irei colocar a seguinte marcação:

{% gist 0f782087fde8b6be88ddff99d7bd1d1b %}

Após isso, acesse a opção **blog presente no menu superior** e em seguida veja o resultado:

![post-docusaurus](/assets/img/posts/criando-sites-landingpages-e-documentacao-com-docusaurus/10-escrevendo-post-docusaurus.png)

Agora para fechar iremos escrever uma página de DOC (documentação), o processo é parecido com a escrita de um post, entretanto as páginas de documentação ficam no diretório **docs** e as páginas de documentação possuem um **id** que é utilizado para referenciar a mesma no arquivo de menu **website/sidebars.json**.

Outra caracteristica importante além do id, uma página de documentação, também possui um atributo chamado **sidebar_label** que é responsável por receber o nome da página conforme será apresentado no menu lateral.

Iremos criar uma página de documentação, para isso vamos criar um arquivo na pasta **docs** chamado **minhadocumentacao.md**, com o seguinte conteúdo:

{% gist 47eeee3febd9fcca4aee9e6ca1be93f3 %}

Com isso neste post foi apresentado, algumas possibilidades do que é possível fazer utilizando o docusaurus. Na data na qual estou escrevendo este post a versão estável do docusaurus é a **1.14.4**, entretanto a **versão 2 esta sendo desenvolvida** e se encontra na versão **alpha 43**, para consultar a respeito da 2 vesão acesse este <a href="https://v2.docusaurus.io/" target="__blank">link</a>

<a href="https://github.com/paulodutra/post-criando-site-lp-doc-docusaurus" target="__blank">Reposítório do Projeto</a>




















