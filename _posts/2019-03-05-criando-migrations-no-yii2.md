---
layout: post
title:  "Criando migrations no Yii2 "
date:   2019-03-05 18:49:23 -0200
categories: blog, tecnologia, php, yii2
---

Fala galera, no post de hoje iremos falar sobre criação de migrations no Yii2. Antes de apresentar sobre a criação de migrations propriamente dita, vamos relembrar um pouco sobre o que é um migration ? 

De forma geral migration é uma abstração de nosso banco de dados, onde podemos criar tabelas e gerenciar as suas alterações diretamente na linguagem de programação na qual estamos trabalhando. 

Sendo assim iremos criar um banco de dados de testes, neste exemplo irei utilizar o Mysql e para a criação do mesmo irei utilizar o Mysql Workbench. Entretanto você poderá utilizar o SGBD de sua preferência, aqui esta o [link](https://www.yiiframework.com/doc/guide/2.0/en/db-active-record){:target="_blank"} para verificar quais banco de dados o Yii2 suporta.
{:target="_blank"} para verificar quais banco de dados o Yii2 suporta.

![criando-o-banco-de-dados](/assets/img/posts/criando-migrations-no-yii2/1-criando-banco-de-dados.png)

Também será necessário configurar o banco de dados para isso, devemos ir no arquivo config/db.php 


{% highlight php %}
 <?php

return [
    'class' => 'yii\db\Connection',
    'dsn' => 'mysql:host=localhost;dbname=yii2_posts',
    'username' => 'root',
    'password' => 'informe-sua-senha',
    'charset' => 'utf8',

    // Schema cache options (for production environment)
    //'enableSchemaCache' => true,
    //'schemaCacheDuration' => 60,
    //'schemaCache' => 'cache',
];

{% endhighlight %}

Para criar uma migration no Yii2, devemos ir no terminal é digitar o seguinte comando: 

{% highlight bash %}
  php yii migrate/create migration_name
{% endhighlight %}

Existe uma convenção em casos de criação de migration (o que é equilavente a criação de uma tabela em nosso banco de dados) temos que utilizar o prefixo: **create_**nome_da_tabela**_table**;

Podemos informar durante a criação da nossa migration quais campos desejamos cria, para isso devemos passar o parametro --fields”nome-do-campo:tipo”, exemplo:  

{% highlight bash %}
  php yii migrate/create create_clients_table --fields"id:primaryKey, name:string, email:string"
{% endhighlight %}

Também podemos informar durante a criação de uma migration via terminal se um campo é obrigatório, basta adicionar logo após o tipo do campo **:notNull**, aplicando essa regra ao exemplo apresentado acima:

{% highlight bash %}
  php yii migrate/create create_clients_table --fields"id:primaryKey, name:string:notNull, email:string:notNull"
{% endhighlight %}


![criando-a-migration](/assets/img/posts/criando-migrations-no-yii2/2-criacao-da-migration.png)

Note que foi adicionada uma pasta chamada **migrations** ao projeto:

![migration-path](/assets/img/posts/criando-migrations-no-yii2/3-migrations-path.png)

Antes do nome que atribuímos a migration note que temos o seguinte prefixo **m190305_232355**, o mesmo representa:

- m: migration;
- 190305: Ano, mês e dia da criação;
- 232355: Hora em que a migração foi criada;

Ao abrir o arquivo da migration, veja que temos a seguinte classe:

{% highlight php %}
<?php

use yii\db\Migration;

/**
 * Handles the creation of table `clients`.
 */
class m190305_232655_create_clients_table extends Migration
{
    /**
     * {@inheritdoc}
     */
    public function safeUp()
    {
        $this->createTable('clients', [
            'id' => $this->primaryKey(),
            'name' => $this->string()->notNull(),
            'email' => $this->string()->notNull(),
        ]);
    }

    /**
     * {@inheritdoc}
     */
    public function safeDown()
    {
        $this->dropTable('clients');
    }
}

{% endhighlight %}

Veja que a classe de migration criada herda de **yii\db\Migration**, com isso conseguimos especificar as colunas seus tipos, valores default entre outras tudo isso é definido no método **safeUp** (dependendo da definição e da forma de criação o nome do metódo também poderá se chamar **up**) que é executado quando estamos executando ou seja quando estamos criando nossa tabela ou criando novos campo a tabelas existentes. 

Já o método **safeDown** (dependendo da definição e da forma de criação o nome do metódo também poderá se chamar **down**) é executado quando estamos revertendo nossas migrations, ou seja desfazendo as alterações criadas.

Todas as definições de campos, e explicação detalhada, se encontra disponível na [documentação oficial](https://www.yiiframework.com/doc/guide/2.0/en/db-migrations){:target="_blank"}.

Para executarmos a migration basta digitarmos o seguinte comando:

{% highlight bash %}
  php yii migrate
{% endhighlight %}

![executando-a-migration](/assets/img/posts/criando-migrations-no-yii2/4-executando-a-migration.png)

Para verificar se a tabela foi realmente criada, vá ao banco de dados e execute o comando **SHOW TABLES** (isso se estiver utilizando Mysql).

![tabelas](/assets/img/posts/criando-migrations-no-yii2/5-tabelas.png)

Note que além da tabelas clients, foi criada uma tabela de controle do Yii chamada **migration**.

## Alguns comandos para administração de migrations

Executa a(s) migration(s):

{% highlight bash %}
  php yii migrate
{% endhighlight %}

Apresenta quais as migrations não foram executadas:

{% highlight bash %}
  php yii migrate/new
{% endhighlight %}


Apresenta o histórico de migrations:

{% highlight bash %}
  php yii migrate/history
{% endhighlight %}

Reverte a(s) migration(s) executadas:

{% highlight bash %}
  php yii migrate/redo
{% endhighlight %}


Bom essa foi uma breve explicação sobre criação de migration no yii2. 
Caso queira dar uma olhada no que foi feito até então, esse é o [link do repositório do projeto](https://github.com/paulodutra/yii2-posts){:target="_blank"}

E ai o que achou ? comente abaixo. 
