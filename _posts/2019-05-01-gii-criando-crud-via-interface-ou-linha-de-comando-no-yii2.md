---
layout: post
title:  "GII Criando CRUD Via interface ou linha de comando no YII2"
date:   2019-05-01 20:02:00 -0200
categories: blog, tecnologia, php, yii2
---

Fala galera, no post de hoje iremos falar a utilização do GII para a criação de CRUD via interface ou linha de comando. Vale apena lembrar que no post anterior, foi apresentado como fazer um CRUD sem a utilização da ferramenta, vale a pena conferir:

<a href="http://paulodutrainfo.com.br/criando-um-crud-no-yii2" target="__blank">Criando CRUD no Yii 2 </a>

Antes de explicar e apresentar o GII, **iremos renomear alguns arquivos que foram implementados anteriormente, pois como iremos utilizar a mesma migration** os nomes que serão gerados para os modelos e demais arquivos serão o mesmos, assim o GII irá sobreescrever eles no momento da criação. Com isso iremos renomear controllers, models e views criadas para representar o cadastro de clientes, ficando conforme apresentado abaixo:

- controllers/_ClientsController.php
- models/_Clients.php;
- views/_clients

Note que no caso da views foi renomeado apenas a pasta.


![renomeando-arquivos](/assets/img/posts/gii-criando-crud-via-interface-ou-linha-de-comando-no-yii2/1-renomeando-arquivos.png)

Agora com os arquivos renomeados podemos seguir. Temos duas formas de acessar o GII uma é por meio do terminal executando o comando:

{% highlight bash %}
    php yii gii/nome-da-acao
{% endhighlight %}

Exemplo **php yii gii/model** para a geração do model. Veja abaixo uma imagem com a lista de ações disponíveis no GII na versão 2.0.16.1 do Yii. 

![acessando-o-gii-via-terminal](/assets/img/posts/gii-criando-crud-via-interface-ou-linha-de-comando-no-yii2/2-acessando-o-gii-via-terminal.png)

Também podemos acessar e utilizar os recursos do GII acessando uma rota "escondida" **URLDOPROJETO/gii**, caso esteja utilizando o yii server na porta padrão a url será: **localhost:8080/gii**. Com isso será apresentada uma página similar a da imagem abaixo:


![acessando-o-gii-via-browser](/assets/img/posts/gii-criando-crud-via-interface-ou-linha-de-comando-no-yii2/3-acessando-o-gii-via-browser.png)

Iremos utilizar o GII via browser. Para criar o CRUD pelo Gii a migration deverá ser criada antes, como iremos utilizar a migration criada nos exemplos anteriores (**migrations/m190305_232655_create_clients_table.php**), não precisamos realizar esse passo.

Após a criação da migration, iremos gerar nosso model. Para isso acesse o gii via browser e logo após isso clique no item model generator e depois em start. 

Após clicar em start. preencha os campos do formulário: **Table Name**, **Model Class Name**, deixe selecionada a opção **Generate Relations from Current Schema**, selecione a opção **Generate ActiveQuery**, deixe selecionada a opção ** Use Schema Name** e clique em preview, revise os dados do formulário e se estiver tudo certo clique em **generate**

![gii-model-generator-pt1](/assets/img/posts/gii-criando-crud-via-interface-ou-linha-de-comando-no-yii2/4-gii-model-generator-pt1.png)

![gii-model-generator-pt2](/assets/img/posts/gii-criando-crud-via-interface-ou-linha-de-comando-no-yii2/5-gii-model-generator-pt2.png)

Note que foram criados dois arquivos dentro da pasta models Clients.php e ClientsQuery.php, sendo Clients.php o arquivo de modelo e o ClientsQuery uma "especie de repository" contendo dois métodos o **one** retornando apenas um registro e o **all** retornando vários registros ambos podem receber como parâmetro **$db**. 

Retorne para a tela inicial do GII (**URLDOPROJETO/gii**) e depois clique em **CRUD Generation** e preencha os campos : Model Class, Search Model Class e Controller Class e depois clique em preview, revise os dados do formulário e se estiver tudo certo clique em **generate**. O nome desejado para o controller deverá ser informado, pois o mesmo não precisar ser gerado separadamente. Os campos citados deverão ser informados o **namespace completo e não apenas o nome desejado**. Apesar do Search Model class não ter sido criado no passo anterior o mesmo deverá ser informado pois será gerado nesse passo.

![gii-crud-generator-pt1](/assets/img/posts/gii-criando-crud-via-interface-ou-linha-de-comando-no-yii2/6-gii-crud-generator-pt1.png)

![gii-crud-generator-pt2](/assets/img/posts/gii-criando-crud-via-interface-ou-linha-de-comando-no-yii2/7-gii-crud-generator-pt2.png)

OBS: Preencha o view path apenas se desejar não trabalhar com o padrão(convenção) do Yii2;

Note que ao clicar em preview é informado todos os arquivos que serão gerados e os que serão sobreescritos. 

Após o passo anterior basta acessar **localhost:8080/clients/index** e veja que agora existe um filtro de pesquisa em todas as colunas, esse formato de apresentação de lista é um recurso chamado  GridView::widget, ele possui uma propriedade chamada **filterModel** que recebe o model Search, para visualizar a utilização desse widget vá no arquivo **views/clients/index.php**

![clients-index](/assets/img/posts/gii-criando-crud-via-interface-ou-linha-de-comando-no-yii2/8-clients-index.png)

Note que ao realizar a criação de um cliente, é realizado o redirecionamento para a visualização do mesmo em um formato de tabela permitindo a atualização ou exclusão do registro.

1 - Criação do cliente

![clients-create](/assets/img/posts/gii-criando-crud-via-interface-ou-linha-de-comando-no-yii2/9-clients-create.png)

2 - Visualização do cliente

![clients-view](/assets/img/posts/gii-criando-crud-via-interface-ou-linha-de-comando-no-yii2/10-clients-view.png)

3 - Listagem de clientes 

Note que ao retornar a listagem de clientes quando a mesma possui registro mais uma coluna é criada para a apresentação das ações:

![clients-index-com-registro](/assets/img/posts/gii-criando-crud-via-interface-ou-linha-de-comando-no-yii2/11-clients-index-pt2.png)

Em caso de atualização do registro de cliente é realizado o redirecionamento para a visualização do mesmo e no caso de exclusão o redirecionamento é realizado para a listagem de clientes.

<a href="https://github.com/paulodutra/yii2-posts" target="__blank">Reposítório do Projeto</a>

Bom essa foi uma breve explicação sobre a utilização do CRUD generator do GII.  E ai o que achou ? comente abaixo. 


