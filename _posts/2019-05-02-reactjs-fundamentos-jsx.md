---
layout: post
title:  "React JS - Fudamentos JSX"
date:   2019-05-01 20:02:00 -0200
categories: blog, tecnologia, javascript, reactjs
---


Fala galera, no post de hoje iremos falar um pouco sobre o ReactJS e também sobre alguns fundamentos que ele possui entre eles o conceito de jsx.

Antes de iniciarmos é importante mencionar que precisamos possuir o <a href="https://nodejs.org/en/" target="__blank">nodejs</a> instalado e com ele iremos ter junto o <a href="https://www.npmjs.com/" target="__blank">npm</a> 
Para verificar se você possui o node instalado na maquina, você poderá executar o seguinte comando:

{% highlight bash %}
   node -v
{% endhighlight %}

Estou utilizando uma a versão do **node** 10.15.1 (LTS)

![node-version](/assets/img/posts/reactjs-fundamentos-jsx/1-node-version.png)

Para saber qual a versão do **npm** execute o seguinte comando:

{% highlight bash %}
    npm -v
{% endhighlight %}

![npm-version](/assets/img/posts/reactjs-fundamentos-jsx/2-npm-version.png)

Após isso você poderá instalar o <a href="https://reactjs.org/" target="__blank">reactjs</a> para isso você poderá instalar o "utilitário" **create-react-app** por meio do comando:

{% highlight bash %}
    npm install -g create-react-app
{% endhighlight %}

Também é possível <a href="https://facebook.github.io/create-react-app/docs/getting-started" target="__blank">criar o projeto utilizando o npx e o yarn</a>. 

Agora para iniciar o projeto basta executar o seguinte comando:


{% highlight bash %}
    npm start
{% endhighlight %}


![npm-start](/assets/img/posts/reactjs-fundamentos-jsx/03-01-npm-start.png)


Ao iniciar o projeto, caso a porta 3000 não esteja em uso o mesmo irá utiliza la. Para acessar o projeto acesse o endereço **localhost:3000** no browser.


![reactjs](/assets/img/posts/reactjs-fundamentos-jsx/03-02-reactjs.png)

Após a instalação do projeto vá no arquivo **package.json** e veja algumas dependências atreladas ao React:

{% highlight js %}
 // contéudo ...
 "dependencies": {
    "react": "^16.8.6",
    "react-dom": "^16.8.6",
    "react-scripts": "3.0.0"
  },
{% endhighlight %}

As depedências apresentadas acima possuem as seguintes finalidades:

- "react": "^16.8.6" (Lib do React)
- "react-dom": "^16.8.6" (Lib da dom virtual do React)
- "react-scripts": "3.0.0" (Lib de build do react)

OBS: Caso você utilize o react native nas depedências ao invés de ter **react-dom** irá ser **react-native**.

Em relação a estrutura inicial do projeto temos as seguintes pastas:

- **node_modules**: Pasta aonde são instaladas as depedências do projeto;
- **public**: Contém a única página do projeto index.html, favicon.icon e o arquivo manifest.json;
- **src** : Contém de fato as implementações do projeto, por padrão vem com o component APP e seus arquivos;
- **package.json**: Arquivo que descreve as dependências do projeto, e também possui outras informações como por exemplo dependências e outras preferências que são apenas para o ambiente de desenvolvimento;

As referências de quais contéudos serão apresentados na página **public/index.html** são referênciados no arquivo **src/index.js**. Toda vez que necessitamos ter um código HTML, ou renderizar html precisamos ter o react importado (**import React from 'react'**).

Para iniciarmos vá no arquivo src/index.js e na linha 7 retire a renderização do Component App e escreve Olá React:

{% highlight js %}
 // contéudo ...
ReactDOM.render(<h1>Olá React</h1>, document.getElementById('root'));
{% endhighlight %}

![ola-react](/assets/img/posts/reactjs-fundamentos-jsx/03-03-ola-react.png)


Com isso iremos conseguir mesclar código html passando o mesmo em arquivos javascript com a extensão **.jsx** ou **.tsx** para projetos React que utilizam TypeScript.

No final das contas esse arquivo jsx será convertido para javascript puro e o arquivo **main.chunk.js** que é um arquivo gerado pelo build que fica armazenado na memória.(exceto em caso de geração de build de produção)


![main-chunk](/assets/img/posts/reactjs-fundamentos-jsx/4-main.chunk.js.png)

Ao gerar o build de produção por meio do comando:

{% highlight bash %}
   npm run build
{% endhighlight %}

OBS: Para isso é necessário parar a execução do comando: **npm start**

![npm-run-build](/assets/img/posts/reactjs-fundamentos-jsx/5-npm-run-build.png)

Note que uma basta chamada build foi gerada no projeto e nessa pasta dentro de **build/static/js**, iremos ter um arquivo chamado **main.hash.chunk.js**, ao abrir esse arquivo note que o nosso código abaixo foi gerado em formato de javascript puro:

![main-chunk-ola-react](/assets/img/posts/reactjs-fundamentos-jsx/7-main-chunk-ola-react.png)

No final das contas esse processo da JSX faz com que você não tenha que manipular a dom de forma manual, até mesmo porque o React tem uma DOM virtual por meio do ReactDOM.

Para ver a efetividade da JSX, vamos ao exemplo da criação de uma lista desordenada com javascript e iremos renderizar essa lista na div de id root que fica no arquivo public/index.html. para isso abra o arquivo **src/index.js** do projeto e realize a implementação abaixo:

{% highlight js %}

    //import React from 'react';
    //import ReactDOM from 'react-dom';
    // ReactDOM.render(<h1>Ola React</h1>,document.getElementById('root'));

    //cria a lista não ordenada
    const list = document.createElement('ul');

    //cria a li que será um elemento da lista
    let item = document.createElement('li');
    //cria um texto que será atrelado ao item (li) da lista
    let text = document.createTextNode('1 - Paulo');
    //adiciona o texto ao item da lista
    item.appendChild(text);
    //adiciona o item a lista propriamente dita
    list.appendChild(item);


    //cria a li que será um elemento da lista
    item = document.createElement('li');
    //cria um texto que será atrelado ao item (li) da lista
    text = document.createTextNode('2 - Maria');
    //adiciona o texto ao item da lista
    item.appendChild(text);
    //adiciona o item a lista propriamente dita
    list.appendChild(item);



    //cria a li que será um elemento da lista
    item = document.createElement('li');
    //cria um texto que será atrelado ao item (li) da lista
    text = document.createTextNode('3 - Ana ');
    //adiciona o texto ao item da lista
    item.appendChild(text);
    //adiciona o item a lista propriamente dita
    list.appendChild(item);

    const element = document.getElementById('root');
    element.appendChild(list);

{% endhighlight %}


![lista-javascrip-puro-com-react](/assets/img/posts/reactjs-fundamentos-jsx/8-lista-javascrip-puro-com-react.png)


Após a manipulação manual na DOM, iremos ter o seguinte resultado:

![resultado-lista-javascript-puro-com-react.](/assets/img/posts/reactjs-fundamentos-jsx/9-resultado-lista-javascript-puro-com-react.png)

Veja abaixo o mesmo exemplo utilizando o apoio da JSX:

{% highlight js %}
    import React from 'react';
    import ReactDOM from 'react-dom';

    ReactDOM.render(
        <ul>
            <li>1 - Paulo</li>
            <li>2 - Maria</li>
            <li>3 - Ana</li>
        </ul>
        ,document.getElementById('root'));



    // //cria a lista não ordenada
    // const list = document.createElement('ul');

    // //cria a li que será um elemento da lista
    // let item = document.createElement('li');
    // //cria um texto que será atrelado ao item (li) da lista
    // let text = document.createTextNode('1 - Paulo');
    // //adiciona o texto ao item da lista
    // item.appendChild(text);
    // //adiciona o item a lista propriamente dita
    // list.appendChild(item);


    // //cria a li que será um elemento da lista
    // item = document.createElement('li');
    // //cria um texto que será atrelado ao item (li) da lista
    // text = document.createTextNode('2 - Maria');
    // //adiciona o texto ao item da lista
    // item.appendChild(text);
    // //adiciona o item a lista propriamente dita
    // list.appendChild(item);

    // //cria a li que será um elemento da lista
    // item = document.createElement('li');
    // //cria um texto que será atrelado ao item (li) da lista
    // text = document.createTextNode('3 - Ana ');
    // //adiciona o texto ao item da lista
    // item.appendChild(text);
    // //adiciona o item a lista propriamente dita
    // list.appendChild(item);

    // const element = document.getElementById('root');
    // element.appendChild(list);

{% endhighlight %}


**OBS:** Acabei comentando o código feito no passo anterior para não perder a referência e podermos comparar depois. 

![10-lista-desordenada-react-com-jsx](/assets/img/posts/reactjs-fundamentos-jsx/10-lista-desordenada-react-com-jsx.png)

Com isso iremos ter o mesmo resultado:

![resultado-lista-jsx-react.](/assets/img/posts/reactjs-fundamentos-jsx/9-resultado-lista-javascript-puro-com-react.png)

<a href="https://github.com/paulodutra/reactjs-fundamentos-jsx" target="__blank">Reposítório do Projeto</a>

Bom essa foi uma breve apresentação do ReactJS e também um pouco sobre os fundamentos da JSX.  E ai o que achou ? comente abaixo. 


