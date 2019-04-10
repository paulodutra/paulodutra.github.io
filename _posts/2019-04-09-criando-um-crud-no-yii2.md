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

Como no exemplo anterior já temos a **migration** necessitamos apenas da criação dos seguintes itens:

- Controllers
- Models
- Views 

# Create = Cadastro

Vamos começar pela letra C do CRUD nesse caso o create para isso iremos começar criando a nossa model. mas antes disso logo após a instalação das dependências vá no arquivo **config/db.php** e altere as configurações de banco de dados(inserindo as suas credenciais). Após as configurações de banco de dados execute as migrations executando o comando abaixo:

{% highlight bash %}
   php yii migrate/up
{% endhighlight %}

![executando-migrations](/assets/img/posts/criando-um-crud-no-yii2/1-executando-migrations.png)

Depois disso vá a pasta models e crie o seguinte arquivo **Clients.php**, a principio o nosso modelo irá ficar igual o abaixo:

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
            [['name', 'email'], 'safe'],
            [['name', 'email'], 'required'],
            [['name', 'email'], 'string', 'max' => 255], 
         ];
     }
}

{% endhighlight %}

Note que o modelo herda da classe ActiveRecord(em extends ActiveRecord) classe responsável por implementar o design pattern de mesmo nome, essa classe em conjunto com outras classes responsáveis pela abstração de banco de dados utilizam o PHP Data Objects (PDO), para mais informações veja:

- <a href="https://www.yiiframework.com/doc/guide/2.0/en/db-active-record" target="__blank">Yii2 Active Record</a>
- <a href="https://www.php.net/manual/pt_BR/intro.pdo.php" target="__blank">PHP Data Objects (PDO)</a>

o metódo estático **tableName** é responsável por retornar o nome da tabela atrelada ao model. Já o método **rules** é responsável por realizar as validações dos dados recebidos e também por habilitar o Massive Massignment (atribuição massiva) por meio da validação do tipo **safe**.

**OBS:** Também é possível criar o modelo utilizando o GII por meio do seguinte comando :

{% highlight bash %}
    php yii gii/model --modelClass="Clients" --tableName="clients"
{% endhighlight %}

![criando-model](/assets/img/posts/criando-um-crud-no-yii2/2-criando-o-model.png)

Executando esse comando você já irá obter o seu model configurado de acordo com os **campos especificados na migration**

Para criar o nosso controller e consequentemente o **GII**(O terminal do Yii Framework) irá criar as views, para isso execute o seguinte comando:

{% highlight bash %}
   php yii gii/controller --controllerClass="app\controllers\ClientsController" 
{% endhighlight %}

![criando-controller-e-views](/assets/img/posts/criando-um-crud-no-yii2/3-criando-controller-e-view.png)

Note que foi criada uma pasta chamada **clients** dentro da pasta views e dentro dessa pasta foi criado um arquivo **index.php**, o mesmo será utilizado posteriormente para a listagem dos nossos registros. 

Para facilitar o acesso a rotas vamos criar mais uma opção de menu para isso vá no arquivo: **views/layouts/main.php** e adicione mais uma opção de menu dentro do de **echo Nav::widget** no array itens adicionando mais um indice ao mesmo:

{% highlight php %}
     echo Nav::widget([
        'options' => ['class' => 'navbar-nav navbar-right'],
        'items' => [
            ['label' => 'Home', 'url' => ['/site/index']],
            ['label' => 'About', 'url' => ['/site/about']],
            ['label' => 'Contact', 'url' => ['/site/contact']],
            ['label' => 'Clients', 'url' => ['/clients/index']],
            Yii::$app->user->isGuest ? (
                ['label' => 'Login', 'url' => ['/site/login']]
            ) : (
                '<li>'
                . Html::beginForm(['/site/logout'], 'post')
                . Html::submitButton(
                    'Logout (' . Yii::$app->user->identity->username . ')',
                    ['class' => 'btn btn-link logout']
                )
                . Html::endForm()
                . '</li>'
            )
        ],
    ]);
{% endhighlight %}


![adicionando-clientes-no-menu](/assets/img/posts/criando-um-crud-no-yii2/4-add-clients-no-menu.png)

Crie um arquivo dentro de **views/clients** chamado **create.php** e neste arquivo iremos criar o seguinte formulário:

{% highlight php %}
   <?php
    /* @var $this yii\web\View */
    use yii\helpers\Url;
    
    ?>
    <h1>Novo Cliente</h1>


    <form name="form" method="post" action="<?= Url::to(['clients/create']); ?>">

    <input type="hidden" name="<?= \yii::$app->request->csrfParam; ?>" 
                value="<?= \yii::$app->request->csrfToken; ?>">

        <div class="form-group">
            <label for="name">Nome:</label>
            <input type="text" class="form-control" id="name" name="name" placeholder="Informe o nome">
        </div>
        <div class="form-group">
            <label for="email">email:</label>
            <input type="email" class="form-control" id="email" name="email" placeholder="Informe o email">
        </div>

        <button type="submit" class="btn btn-primary">Enviar</button>
    </form>
{% endhighlight %}

Note que antes do formulário propriamente dito, estamos utilizando um helper de URL ** yii\helpers\Url**, com a utilização dessa classe
conseguimos utilizar o método **Url::to(['clients/create'])** dentro desse método informamos em um array o nome da controller/método. 

Note que criamos um campo do tipo hidden com o nome esse atributo da aplicação (\yii::$app->request->csrfParam;) irá atribuir o nome **_csrf**  e o outro (\yii::$app->request->csrfToken;) irá imprimir o token. Isso serve para evitar ataques do tipo **CSRF - Cross-Site Request Forgery (CSRF)** onde esse token que foi gerado no nome desse campo se torna uma sessão no servidor e ao enviar o formulário ele verifica se essa seção existe. Isso serve para evitar ataques aonde um site ou outra aplicação que não seja a "sua" forje uma requisição.  Para mais informações a respeito de CSRF acesse:


- <a href="https://dadario.com.br/csrf-o-que-e/" target="__blank">CSRF - O que é</a>
- <a href="https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md" target="__blank">Cross-Site Request Forgery (CSRF) </a>
- <a href="https://pt.wikipedia.org/wiki/Cross-site_request_forgery" target="__blank">O cross-site request forgery (CSRF ou XSRF)</a>

E os demais dados do formulário são relativos **aos nomes de campos onde esses nomes são iguais aos nomes definidos na migration** e o envio do mesmo


Agora precisamos ir no arquivo **controllers/ClientsController** e criar o método create. Os métodos no Yii utilizam o o prefixo action antes do nome do método. 

{% highlight php %}
    <?php

        namespace app\controllers;

        use app\models\Clients;
        use yii\web\NotFoundHttpException;

        class ClientsController extends \yii\web\Controller
        {

              public function actionIndex()
             {
      
                return $this->render('index');
             }
            
             /**
                * <b>actionCreate</b> Método responsável por realizar a criação de um novo cliente.
                * OBS: Para utilizar $model->attributes, deverá estar habilitado no model dentro do metodo rules os nomes do campo com o valor safe
            */
              public function actionCreate()
              {
                $request = \yii::$app->request;

                if($request->isPost)
                {
                $model = new Clients();

                //attributes é uma propriedade do active record ao passar $request->post() sem parametro exemplo $request->post('email') nem nada ele irá pegar todos os dados do post
                $model->attributes =  $request->post(); 
                //só grava se o metodo rules existir no modelo
                $model->save();           
                return $this->redirect(['clients/index']);

                    }

                return $this->render('create');
             }


            

        }

{% endhighlight %}


A primeira linha é um helper de aplicação do Yii que obtém os dados da requisição é armazena o mesmo na váriavel **$request** depois em **$request->isPost** ele verifica se a requisição realizada utiliza o verbo
http post caso seja verdade significa que é um cadastro, logo após isso é instânciado o modelo **$model = new Clients();** e também obtidos os atributos do mesmo **$model->attributes**. 

OBS: Apesar de não termos definidos explicitamente atributos em nosso model, o ActiveRecord obtem os nomes declarados na tabela. 

Já o **$request->post();** obtem todos os dados submetidos no formulário, e o **$model->save();** salva os dados em nosso banco de dados. 

# Read = Listagem

Após ter realizado o C de nosso CRUD agora esta na hora de implementarmos a letra R, para isso vá no arquivo Agora precisamos ir no arquivo **controllers/ClientsController**  e vá até o método actionIndex: 

{% highlight php %}
    <?php
        namespace app\controllers;

        use app\models\Clients;
        use yii\web\NotFoundHttpException;

        class ClientsController extends \yii\web\Controller
        {

            /**
            * <b>actionIndex</b> Método responsável por realizar a listagem dos clientes
            */

            public function actionIndex()
            {
                $clients = Clients::find()->all();

                return $this->render('index', [
                    'clients' => $clients
                ]);
            }


            /**
            * <b>actionCreate</b> Método responsável por realizar a criação de um novo cliente.
            * OBS: Para utilizar $model->attributes, deverá estar habilitado no model dentro do metodo rules os nomes do campo com o valor safe
            */
            public function actionCreate()
            {
                $request = \yii::$app->request;

                if($request->isPost)
                {
                $model = new Clients();

                //attributes é uma propriedade do active record ao passar $request->post() sem parametro exemplo $request->post('email') nem nada ele irá pegar todos os dados do post
                $model->attributes =  $request->post(); 
                //só grava se o metodo rules existir no modelo
                $model->save();           
                return $this->redirect(['clients/index']);

                }

                return $this->render('create');
            }
    ?>
{% endhighlight %}

Para obter a listagem de todos os clientes, precisamos de uma instância do model **Clients** e depois precisamos acessar os métodos **Clients::find()->all**, ao fazer isso basta armazernar em uma variável e passar em forma 
de array para o método render **return $this->render('index', ['clients' => $clients]);**

Após a criação do método index no controller, precisamos abrir o arquivo **views/clients/index.php** e realizar a criação de nossa tabela para a exibição dos dados:

{% highlight php %}
  <?php
    use yii\helpers\Url;
    /* @var $this yii\web\View */
    ?>
    <h1 class="text text-center">Clientes</h1>
    <a href="<?= Url::to(['clients/create']);?>" class="btn btn-success">Novo Cliente</a>
    <table class="table">
        <thead>
        <tr>
            <th>#</th>
            <th>Nome</th>
            <th>Email</th>
            <th>-</th>
        </tr>
        </thead>
        <tbody>
            <?php foreach($clients as $client): ?>
                <tr>
                    <td><?= $client->id; ?></td>
                    <td><?= $client->name; ?></td>
                    <td><?= $client->email; ?></td>
                    <td>
                       - 
                    </td>
                </tr>
            <?php endforeach; ?>
        </tbody>
    </table>
{% endhighlight %}


Com isso temos o nosso cadastro e a listagem feita, faltando apenas realizar o teste: 

1- Enviando os dados

![adicionando-clientes-no-menu](/assets/img/posts/criando-um-crud-no-yii2/5-testando-cadastro.png)

2- Listagem de clientes

![cliente-adicionado](/assets/img/posts/criando-um-crud-no-yii2/6-cliente-adicionado.png)

# Update = Atualização

Agora vamos para a letra U rs! Para isso precisamos ir no arquivo **controllers/ClientsController** e criar o método update (actionUpdate), diferente do método create esse irá receber por parâmetro o $id do registro que será editado. 
Para que possamos receber esse parâmetro via url no seguinte formato **clients/id/update**, **precisamos registrar essa rota no arquivo config/web.php php no array rules** :

{% highlight php %}
    //configurações anteriores
    'urlManager' => [
            'enablePrettyUrl' => true,
            'showScriptName' => false,
            'rules' => [
                'clients/<id:\d+>/update' => 'clients/update',
            ],
        ],
{% endhighlight %}

O **<id:\d+>** é uma regex que diz que para receber apenas números positivos. Agora o nosso método actionUpdate: 

{% highlight php %}
    <?php

    namespace app\controllers;

    use app\models\Clients;
    use yii\web\NotFoundHttpException;

    class ClientsController extends \yii\web\Controller
    {
        //métodos anteriores

        /**
        * <b>actionUpdate</b> Método responsável em realizar a atualização do cliente, recebe o id como parametro (antes disso devera registrar a 
        * personalização de rota no arquivo config/web.php) dentro do array rules
        */
        public function actionUpdate($id)
        {
            $model = Clients::findOne($id);

            if(! $model)
            {
                throw new NotFoundHttpException("Página não encontrada");
            }

            $request = \yii::$app->request;
            
            if($request->isPost)
            {
            //attributes é uma propriedade do active record ao passar $request->post() sem parametro exemplo $request->post('email') nem nada ele irá pegar todos os dados do post
            $model->attributes =  $request->post(); 
            //irá identificar que o registro já existe
            $model->save();
            
            return $this->redirect(['clients/index']);

            }

            return $this->render('update', [
                'model' => $model
            ]);
        }

?>
{% endhighlight %}

Veja que ele tem similaridades com o método actionCreate, o que diferencia é que precisamos consultar o id recebido então para isso criamos uma instância do modelo e utilizamos o método findOnde: **$model = Clients::findOne($id);**
Caso o registro não exista iremos retornar uma exceção utilizando uma instância de **NotFoundHttpException** passando a mensagem. Essa classe irá retornar a mensagem e a página irá ter o status code 404. Conforme imagem apresentada abaixo:

![registro-nao-encontrado](/assets/img/posts/criando-um-crud-no-yii2/7-erro-status-code-404.png)

O restante do método é bem similar ao método actionCreate. 

Para finalizar a funcionalidade precisamos criar o formulário, para isso precisamos criar um arquivo chamado **update.php** em **views/clients**:


{% highlight php %}
  <?php
        /* @var $this yii\web\View */
        use yii\helpers\Url;
    ?>
    <h1>Atualizar Cliente</h1>

    <form name="form" method="post" action="<?= Url::to(['clients/update', 'id' => $model->id]); ?>">

    <input type="hidden" name="<?= \yii::$app->request->csrfParam; ?>" 
                value="<?= \yii::$app->request->csrfToken; ?>">

        <div class="form-group">
            <label for="name">Nome:</label>
            <input type="text" class="form-control" id="name" name="name" 
                    placeholder="Informe o nome" value="<?= $model->name; ?>">
        </div>
        <div class="form-group">
            <label for="email">Email:</label>
            <input type="email" class="form-control" id="email" name="email"
                    placeholder="Informe o email" value="<?= $model->email; ?>">
        </div>

        <button type="submit" class="btn btn-primary">Enviar</button>
    </form>
{% endhighlight %}

O formulário também é bem similar ao do arquivo create.php, com exceção de carregar o valor atribuido ao registro por meio do value do input, o contéudo da variavel model veio do método render na controller. Para finalizar o editar vá no arquivo **views/clients/index.php** e adicione na listagem o botão de editar:

{% highlight php %}
    //contéudo anterior
      <tbody>
        <?php foreach($clients as $client): ?>
            <tr>
                <td><?= $client->id; ?></td>
                <td><?= $client->name; ?></td>
                <td><?= $client->email; ?></td>
                <td>
                    <a href="<?= Url::to(['clients/update', 'id' => $client->id]);?>">Editar</a> |
                </td>
            </tr>
        <?php endforeach; ?>
    </tbody>

{% endhighlight %}


# Delete = Excluir

Enfim chegamos ao D do CRUD. Para isso precisamos registar a rota para que possamos receber o id do registro via parâmetro no seguinte formato **clients/id/delete**, para isso vá no arquivo: **config/web.php** :

{% highlight php %}
    //configurações anteriores
    'urlManager' => [
            'enablePrettyUrl' => true,
            'showScriptName' => false,
            'rules' => [
                'clients/<id:\d+>/delete' => 'clients/delete',
            ],
        ],
{% endhighlight %}

Agora precisamos criar o nosso método delete (actionDelete), no arquivo **controllers/ClientsController.php** :

{% highlight php %}
    <?php

        namespace app\controllers;

        use app\models\Clients;
        use yii\web\NotFoundHttpException;

        class ClientsController extends \yii\web\Controller
        {
            //métodos anteriores

            /**
            * <b>actionDelete<b/> Método responsável por excluir um registro, recebe como parametro o id do mesmo como parametro. (antes disso devera registrar a 
            * personalização de rota no arquivo config/web.php) dentro do array rules 
            * 
            */
            public function actionDelete($id)
            {

                $model = Clients::findOne($id);

                if(! $model)
                {
                    throw new NotFoundHttpException("Página não encontrada");
                }

                $model->delete();
                
                return $this->redirect(['clients/index']);
            }

    ?>
{% endhighlight %}

<a href="https://github.com/paulodutra/yii2-posts" target="__blank">Reposítório do Projeto</a>

Bom esse foi o post/tutorial de criação de CRUD no Yii2, lembrando que o Yii possui uma forma mais fácil para criação de CRUD, a mesma iremos ver nos proximos posts. E ai o que achou ? comente abaixo. 















