---
layout: post
title:  "Convenções para rotas e controllers no Yii2"
date:   2019-02-28 19:59:23 -0200
categories: blog, tecnologia, php, yii2
---


Fala galera, no post de hoje iremos falar um pouco sobre convenções de rotas e controllers no Yii2, recapitulando no [post anterior](http://paulodutrainfo.com.br/um-pouco-sobre-yii2-framework){:target="_blank"} efetuamos a instalação e a explicação da estrutura de arquivos e pastas. 

Após realizar a instalação, podemos "startar" o servidor embutido por meio do seguinte comando: 

{% highlight bash %}
   php yii serve
{% endhighlight %}

Por padrão o projeto irá executar na seguinte url: **localhost:8080**, ao executar a mesma no browser você terá uma tela similar a que esta sendo apresentada abaixo:

![yii2](/assets/img/posts/convencoes-para-rotas-e-controllers-no-yii2/1-yii2.png)

No canto inferior direito temos um icone do framework esse espaço é reservado para a **debug toolbar**, caso você clique na seta, será apresentado no rodapé da página algumas informações tais como versão do framework, versão do php, status code, rota, tempo de carregamento entre outras: 

![debug-toolbar-rodape](/assets/img/posts/convencoes-para-rotas-e-controllers-no-yii2/2-debug-toolbar-rodape.png)

Caso clique no icone, será aberta uma página com maiores informações, nessa página além das informações apresentadas no item anterior, é apresentada um item chamado **Available Debug Data** com uma tabela que permite a realização de filtros, nessa tabela é apresentado o IP que realizou a requisição, qual metódo http foi utilizado, se a requisição foi via ajax entre outras informações:

![debug-toolbar-page](/assets/img/posts/convencoes-para-rotas-e-controllers-no-yii2/3-debug-toolbar-page.png)

Apresentada a debug toolbar, por default temos algumas rotas padrões no projeto, entre elas uma rota de login, ao clicar nessa opção de menu, podemos realizar um teste utilizando os seguintes usuários e senhas: 

- usuário: admin e senha:admin;
- usuário demo e senha: demo;

## Rotas 

No Yii2 **não precisamos registrar as rotas em um arquivo separado** o roteamento dele funciona da seguinte forma, para exemplificar iremos pegar como exemplo a seguinte rota **index.php?r=site%2Fabout**


![site-controller-about](/assets/img/posts/convencoes-para-rotas-e-controllers-no-yii2/4-site-controller-about.png)

Temos um controller chamado SiteController.php por padrão a prefixo antes da palavra controller será parte do nome da rota e dentro do controller temos uma action chamada about. o parâmetro **r** que é passado como query string representa a rota logo após temos o nome do controller e depois o nome da ação. OBS: Esse **%2F é o caractere / (barra)** que esta no padrão urlencode. 


![rota-about](/assets/img/posts/convencoes-para-rotas-e-controllers-no-yii2/5-rota-about.png)

Em caso de nome composto para controller a regra se mantém entretanto ele irá utilizar o hífen para separar o nome na URL exemplo temos um controller chamado TestTestandoController uma actionIndex no mesmo, então ficara:
**test-testando/index ou index.php?r=test-testando/index** a mesma regra se aplica para o nome das actions

Por padrão para renderizar uma view no Yii utilizamos o helper **$this->render(‘nome-da-view’)**, por convenção se o nome do controller for site e a view for about ele ira buscar no diretório views uma pasta chamada site/about.php

## Criando uma controller no Yii2 usando o Gii

O Gii é um gerador de código(scafold) do Yii, com ele podemos gerar estruturas de controller, form, module, model, migration e até cruds completos. 
Para gerar a estrutura inicial de um controller no Gii temos que digitar no terminal:

{% highlight bash %}
 php yii gii/controller --controllerClass=”nameSpace-do-controller” 
{% endhighlight %}

Por exemplo: 

{% highlight bash %}
    php yii gii/controller --controllerClass=”app\controllers\TestController”
{% endhighlight %}

Ao executar responda com yes a seguinte pergunta **Ready to generate the selected files?**

![guii-criando-controller](/assets/img/posts/convencoes-para-rotas-e-controllers-no-yii2/6-guii-criando-controller.png)

## Configurando urls amigáveis no Yii2

Apesar do padrão query string ser o padrão ao realizarmos a instação do framework temos a opção de trabalhar com urls amigáveis. Vá até o arquivo web.php que fica dentro do diretório config. Descomente as linhas de comentários que está envolvendo o array urlManager, note que o mesmo possui alguns parâmetros. 

- enablePrettyUrl: caso esteja setado como true, ativara a url amigavel: exemplo: URL/site/about;
- showScriptName: caso esteja setado como true, irá acrescentar o arquivo index.php antes do restante da rota, exemplo: URL/index.php/site/about
- rules: Utilizado para a criação de regras e regras de parâmetros para as rotas. Exemplo: 'test/<id:\d+>'  =>   'test/index', retira a necessidade de ter ou não que acrescentar index na hora de acessar a rota test/index e acrescenta o id logo após o nome atribuído a rota. O valor da chave apresenta a rota e seu parâmetro sendo que logo após os dois pontos está sendo passada um expressão regular para validar se o mesmo é número inteiro, o valor da chave aponta para o nome da controller e método que irá ser aplicado essa regra.

![7-web-config-url-pretty.png](/assets/img/posts/convencoes-para-rotas-e-controllers-no-yii2/7-web-config-url-pretty.png)

Bom essa foi uma pequena explicação sobre convenções de rotas e controller no framework yii2. E ai o que achou ? comente abaixo. 
