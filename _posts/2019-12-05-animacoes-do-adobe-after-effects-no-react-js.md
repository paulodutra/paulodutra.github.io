---
layout: post
title:  "Animações do Adobe After Effects no React JS"
date:   2019-12-05 14:03:00 -0200
categories: blog, tecnologia, javascript, reactjs
---

Fala galera, no post de hoje irei mostrar uma forma de utilizar as animações feitas no Adobe After Effects no React JS. Acredito que assim como eu, você esteja pensando como isso acontece no "backstage", bom explicando de forma simples, isso é possível porque existe uma biblioteca chamado **Lottie** que verifica as animações exportadas do Adobe After Effects em formato json (do tipo Bodymovin) e com isso permite a criação da mesma em celulares e na web.  <a href="https://airbnb.io/lottie/#/" target="__blank">Para mais informações existe esse link do airbnb.io explicando melhor sobre esse processo.</a>

Para a criação desse exemplo iremos utilizar o pacote <a href="https://www.npmjs.com/package/react-bodymovin" target="__blank">react-bodymovin</a>. 


Agora que foi apresentado um pouco sobre o **conceito/processo**, vamos criar uma app no <a href="https://reactjs.org/" target="__blank">reactjs</a> com o intuito de utilizar essas animações em cenários de mensagens de **erro 404 e 500**, não iremos implementar nada de backend, os componentes serão vistos por meio de rotas definidas. 

Sendo assim crie um projeto reactjs, no exemplo abaixo iremos utilizar o utilitário de linha de comando <a href="https://github.com/facebook/create-react-app" target="__blank">create-react-app</a>



Após criar o projeto você pode executar o mesmo utilizando o comando na pasta raiz do projeto: **npm start**. 
Agora vamos criar duas pastas uma chamada **animations** e outra chamada **components** dentro de src, conforme imagem abaixo:

![estrutura-de-pastas](/assets/img/posts/animacoes-do-adobe-after-effects-no-react-js/0-estrutura-de-pastas.png)

Definida a estrutura de pastas, agora vamos terminar de instalar as demais dependências do projeto via npm:

{% gist 3c908a9b909d4211fedf787fa414f45b  %}

Iremos necessitar dos arquivos de animações em formato json, para isso iremos fazer o download de animações prontas no <a href="https://lottiefiles.com/" target="__blank">lottiefiles.com</a> **OBS:** para realizar o download é necessário criar uma conta, entretanto temos animações gratuitas e animações premium. 

Após estar logado em sua conta no <a href="https://lottiefiles.com/" target="__blank">lottiefiles.com</a>, realize a busca da animação desejada, neste caso irei pesquisar por **error 500**, irei realizar o download da segunda animação apresentada no resultado de busca, **renomear para error-500.json** e depois mover o arquivo para a pasta **src/animations**.

**OBS:** Testei as 3 animações gratuitas disponibilizadas sob o resultado de pesquisa error 500, entretanto os 2 primeiros não funcionaram (Não sei se é algo de errado no arquivo de animação). Então por isso neste exemplo utilizamos a 3º animação ("o robo laranjado").

![escolha-download-animacao-error-500](/assets/img/posts/animacoes-do-adobe-after-effects-no-react-js/1-lottiefiles-error-500.png)

![download-animacao-error-500](/assets/img/posts/animacoes-do-adobe-after-effects-no-react-js/2-lottie-download-error-500.png)


Feita a escolha da primeira animação, agora iremos escolher a segunda e ultima animação que iremos utilizar, neste caso irei pesquisar por **404**, irei realizar o download da quarta animação apresentada no resultado de busca, **renomear para error-404.json** e depois mover o arquivo para a pasta **src/animations**.

![escolha-download-animacao-error-404](/assets/img/posts/animacoes-do-adobe-after-effects-no-react-js/3-lottiefiles-escolha-animacacao-404.png)

![download-animacao-error-404](/assets/img/posts/animacoes-do-adobe-after-effects-no-react-js/4-lottiefiles-download-error-404.png)

Realizado os passos anteriores, crie um arquivo chamado **ErrorServer.jsx** dentro de src/components esse arquivo irá representar uma tela de erro 500 (HTTP code status) e com isso iremos realizar a seguinte implementação:

{% gist ccd1e215d0990082e0a63ea6bde73558  %}

Foi realizado na implementação desse componente a importação do **react-bodymovin** e do arquivo error-500.json e depois criado um componente funcional que foi atribuido a uma constante para posteriormente ser exportado. 
Depois de realizadas essas definições criamos uma constante de configuração do react-bodymovin e nessa constante foram realizadas as seguintes definições:

- **loop:** true, (define se a animação irá ser executa em loop ou seja mais de uma vez)
- **autoplay:** true, (define se a animação será iniciada automáticamente)
- **animationData:** ErrorServerAnimation (define os dados da animação ou seja no nosso caso o arquivo)

Agora iremos realizar a implementação do outro componente que irá realizar a animação de error 404, para isso crie um arquivo chamado **NotFound.jsx** dentro de src/components e com isso iremos ter uma implementação similar a que foi realizada no componente **ErrorServer.jsx**:

{% gist ecb02ccb12403b83b8d5d800da5e6d35 %}

Com isso iremos criar um outro componente chamado **Home** que irá apresentar as rotas para conseguirmos acessar os componentes  **NotFound.jsx** e **ErrorServer.jsx**, para isso crie um arquivo chamado **Home.jsx** dentro de src/components: 

{% gist 1227ecba032b2f2b3411e55d6c835a82 %}

O componente **Home.jsx** é de implementação simples, também é um componente funcional que utiliza Link do react-router-dom (definição de link para chegar até os componentes), agora esta faltando realizar a definição das rotas propriamente ditas para isso, iremos alterar o arquivo **App.js** que fica localizado em **src**:

{% gist 5eb378b82f55c4be457ab6fc2a72f562 %}

Após a definição de rotas, vamos entender o que foi definido no arquivo **App.js**, foi realizada a importação do **BrowserRouter, Switch e Route do react-router-dom** e com foi apelidado o **BrowserRouter para Router**.

A estratégia de roteamento <a href="https://reacttraining.com/react-router/web/api/BrowserRouter" target="__blank">BrowserRouter</a> dentro outras coisas basicamente define a rota com /(barra) exemplo: **url/componente** também temos a estrategia <a href="https://reacttraining.com/react-router/web/api/HashRouter" target="__blank">HashRouter</a> a rota é definida com #(hash) **url/#/componente**

Após realizado os passos anteriores iremos ter o seguinte resultado:

Ao "startar" a aplicação por meio do comando: **npm start**. Será apresentado o componente home ao acessar a rota "/", o componente home por sua vez possui dois links: 

![apresentacao-componente-home](/assets/img/posts/animacoes-do-adobe-after-effects-no-react-js/5-apresentacao-componente-home.png)

Ao clicar no link **"Error 404 Not Found"** é apresentado o seguinte:

![apresentacao-componente-404](/assets/img/posts/animacoes-do-adobe-after-effects-no-react-js/6-apresentacao-componente-404.png)

Ao clicar no link **"Error 500 Internal Server Error"** é apresentado o seguinte:

![apresentacao-componente-500](/assets/img/posts/animacoes-do-adobe-after-effects-no-react-js/7-apresentacao-componente-500.png)

**OBS:** As imagens não apresentam o movimento da animações.

<a href="https://github.com/paulodutra/example-reactjs-bodymovin" target="__blank">Reposítório do Projeto</a>

Bom essa foi uma pequena apresentação de como utilizar animações do Adobe After Effects no React JS. E ai o que achou ? comente abaixo.
























