---
layout: post
title:  "Um pouco sobre Yii2 Framework"
date:   2019-02-25 16:02:23 -0200
categories: blog, tecnologia, php, yii2
---

Fala galera, no post de hoje iremos falar um pouco sobre o framework [yii2](https://www.yiiframework.com/doc/guide/2.0/en){:target="_blank"}. 

O Yii2 é um framework full stack que utiliza a linguagem [PHP](http://php.net/){:target="_blank"}, possui pacotes ou pilhas completas para trabalhar durante o desenvolvimento. O mesmo trabalha com a arquitetura MVC.

Para trabalhar com o Yii 2 é necessário instalar um plugin de forma global no composer : **composer global require fxp/composer-asset-plugin:^1.2.0**, sendo assim caso não tenha instado o [composer](https://getcomposer.org/){:target="_blank"} (gerenciador de depedências PHP), será necessário a instalação do mesmo. 


{% highlight bash %}
    composer global require fxp/composer-asset-plugin:^1.2.0
{% endhighlight %}

Esse plugin serve para gerenciar dependências do bower e do npm utilizando o arquivo composer.json. Sem a necessidade de ter um um bower.json e um package.json(npm) de forma separada na aplicação.

![instalando-composer-asset-plugin](/assets/img/posts/um-pouco-sobre-yii2-framework/1-instalando-composer-asset-plugin.png)

## Template (Skeleton) do Yii2


Antes de instalar escolha o skeleton(template) que mais se adeque a sua aplicação temos: **yii2-app-basic** [app-basic](https://github.com/yiisoft/yii2-app-basic){:target="_blank"} e **yii2-app-advanced** [app-advanced](https://github.com/yiisoft/yii2-app-advanced){:target="_blank"}. De forma simples, rápida e sucinta, o template basic são destinados a projetos pequenos, e o advanced para projetos grandes e/ou com vários níveis. 

## Instalação

Para realizar a instalação do yii2 com o template básico, execute o seguinte comando no terminal:

{% highlight bash %}
   composer create-project --prefer-dist yiisoft/yii2-app-basic yii2
{% endhighlight %}

![instalacao-yii2-app-basic](/assets/img/posts/um-pouco-sobre-yii2-framework/2-instalacao-yii2-app-basic.png)

O arquivo inicial de uma aplicação yii2-app-basic, ou seja que usa o template básico fica dentro da pasta **web/index.php** (em alguns frameworks essa pasta se chama **public**)


## Estrutura de pastas e arquivos


Veja abaixo uma breve explicação sobre a estrutura de pastas e arquivos de uma aplicação yii2, que utiliza o template app-basic:

**assets:** Contém arquivos necessários para gerenciar os assets(“ativos”, css, js, png, jpg, fonts, html) da aplicação. O registro dos assets acontece no arquivo **AppAsset.php**;

**commands:** Armazena os arquivos de comandos disponíveis no terminal do framework. Possibilita criarmos comandos personalizados no formato de controllers. Exemplo ao instalar o framework, temos um arquivo HelloController.php dentro desta pasta;

**config:** Armazena todos os arquivos de configuração da aplicação. Exemplo: console, banco de dados, parâmetros globais da aplicação (**params.php**) , configurações gerais do framework definidas no arquivo web.php (autenticação, cache, request, email, log, banco de dados, definição de ambiente );

**controllers:** Pasta destinada a todos os controllers da aplicação. OBS: Ao criar uma aplicação temos um controller padrão chamado SiteController.php;

**mail:** Define configurações dos templates de envio de e-mail;

**models:** Pasta destinada a todos os modelos da aplicação. OBS: No Yii formulário também pode ser tratado como um model;

**runtime:** Guarda logs da aplicação, debugs e tudo que é registrado para ser armazenado em tempo de execução;

**tests:** Pasta destinada para o armazenamento de tests. O Yii 2 trabalha com um framework de tests chamado codeception, pelo menos até a versão 2.0.9, o mesmo trás uma forma simples de executar tests;

**vendor:** Armazena as dependências da aplicação, ou seja o próprio framework é armazenado nesta pasta. OBS: Caso o plugin **fxp/composer-asset-plugin** estiver instalado, o bower e o npm ficará nessa pasta;

**views:** Armazena toda a camada de visualização, templates da aplicação, e views de usuário;

**web:** Pasta aonde o possui o document root da aplicação o arquivo index.php, assets(imagens, js) e css, index-test.php (para trabalhar com os tests);

**widgets:** Armazena os widget, são blocos de views reutilizáveis em diversas interfaces de usuário.

**.bowerrc:** Define algumas configurações do bower, caso seja necessário trabalhar com o mesmo de forma separada. (bower.json), lembrando que devido ao plugin **fxp/composer-asset-plugin** é possível trabalhar com o bower e npm no arquivo **composer.json**;

**composer.json:** Arquivo que define e gerencia as dependências do projeto. OBS: minimum-stability: stable, define que o projeto só irá instalar dependências estáveis;

**requirements.php:** Verifica os requisitos para executar o yii2;

**yii:** Terminal de comandos do yii2. OBS: As vezes é necessário colocar php antes de algum comando yii. Exemplo: **php yii serve**;

**yii.bat:** Terminal de comandos do yii2, arquivo referente ao windows.


![estrutura-de-pastas-yii2-app-basic](/assets/img/posts/um-pouco-sobre-yii2-framework/3-estrutura-de-pastas-yii2-app-basic.png)

Bom essa foi uma pequena introdução sobre o framework yii2. Pretendo escrever outros posts sobre o yii2 aqui no blog.
E ai o que achou ? comente abaixo. 







 





