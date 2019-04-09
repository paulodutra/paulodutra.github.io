---
layout: post
title:  "Criando um CRUD no Yii2"
date:   2019-04-09 19:14:23 -0200
categories: blog, tecnologia, php, yii2
---



Fala galera, no post de hoje iremos falar sobre criação de um CRUD no Yii2, antes disso vamos relembrar o que é um CRUD:

- C : Create
- R : Read
- U : Update
- D : Delete

Iremos utilizar o reposítório e o que foi feito nos posts anteriores : 

<a href="https://github.com/paulodutra/yii2-posts" target="__blank">Repositório</a>

Posts: 

- <a href="http://paulodutrainfo.com.br/um-pouco-sobre-yii2-framework" target="__blank">1 - Um pouco sobre o Yii2 Framework</a>
- <a href="http://paulodutrainfo.com.br/convencoes-para-rotas-e-controllers-no-yii2" target="__blank">2- Convenções para rotas e controllers no Yii2</a>
- <a href="http://paulodutrainfo.com.br/criando-migrations-no-yii2" target="__blank">3 - Criando migrations no Yii2</a>

Para utilizar o que foi feito basta clonar o repositório:

{% highlight bash %}
    git clone https://github.com/paulodutra/yii2-posts.git
{% endhighlight %}

Após clonar o repositório entre na raiz do projeto e execute o seguinte comando 

{% highlight bash %}
    composer install 
{% endhighlight %}

Logo após a instalação das dependências vá no arquivo config/db.php e altere as configurações de banco de dados(inserindo as suas credenciais) e depois disso vá a pasta models e crie o seguinte arquivo Clients.php, a principio o nosso modelo irá ficar igual o abaixo:

{% highlight php %}
    <?php

namespace app\models;
use  yii\db\ActiveRecord;

class Clients extends ActiveRecord
{
     /**
     * <b>tableName</b> Método estático que retorna o nome da tabela
     * @return string table
     */
    public static function tableName()
    {
        return 'clients';
    } 

    /**
     * <b>rules</b> Método responsável por realizar a validação dos dados recebidos, 
     * OBS: O tipo safe, informa que as colunas que podem ser informado os dados do formulário (Massive Assignment) atribuição da massiva
     */

     public function rules()
     {
         //informa que os dados podem ser gravados ao utilizar o   $model->attributes no controller
         return [
            [['name', 'email'], 'required'],
            [['name', 'email'], 'string', 'max' => 255], 
         ];
     }
}

{% endhighlight %}


Como no exemplo anterior já temos a **migration** necessitamos apenas da criação dos seguintes itens:

- Controllers
- Models
- Views 

Iremos começar criando a nossa model, para isso execute o seguinte comando: 


Para criar o nosso controller e consequentemente o **GII**(O terminal do Yii Framework) irá criar as views, para isso execute o seguinte comando:

{% highlight bash %}
   php yii gii/controller --controllerClass="app\controllers\ClientsController" 
{% endhighlight %}

Note que foi criada uma pasta chamada **clients** dentro da pasta views e dentro dessa pasta foi criado um arquivo index.php, o mesmo será utilizado posteriormente para a listagem dos nossos registros.