---
layout: post
title:  "Upload de Arquivos Base64 via API Laravel 6"
date:   2020-01-13 11:06:00 -0200
categories: blog, tecnologia, php, laravel
---

Fala galera, o post será sobre upload de arquivos 64 utilizando o framework <a href="https://laravel.com/docs/6.x/" target="__blank">Laravel</a>. Para a criação do projeto é necessário ter o <a href="https://getcomposer.org/" target="_/__blank">Composer</a> instalado (gerenciador de depedências PHP).

Para criar o projeto, abra uma aba no terminal/cmd é digite o seguinte comando:

{% gist e330cdbf99d8a23675853a95ecd7037c  %}

Após a criação do projeto e abrir o mesmo no editor/IDE de sua preferência, vamos criar a **controller e a rota** responsável por enviar o arquivo. Vamos criar primeiramente a controller para isso abra novamente o terminal e digite:

{% gist 08bdec1962a98b77f9f4951401d8e6f4  %}

Para criar a rota vá em **routes/api.php**, iremos criar uma rota chamada **send-file** com o verbo POST que irá ser responsável por criar enviar o arquivo, apenas para padronizar a forma de envio da requisição iremos definir o nome do atributo(campo) como "file" e o mesmo deverá ser enviado em string no formato base64. 

{% gist a42715eb258e1eeb4d0e0bd15b7f6671  %}

Aṕos a criação da rota, inicie o servidor utilizando o artisan por meio do comando: **php artisan serve**, ele irá "startar" a aplicação para utilizar a porta 8000 (caso não tenha nenhum serviço que esteja utilizando a mesma), para acessar a aplicação basta utilizar o seguinte endereço: **http://localhost:8000**.

Abra o arquivo **app/Http/UploadController.php** e defina o método **sendFile** e também defina o use na facade **Illuminate\Support\Facades\Storage**, iremos aproveitar para definir o formato que iremos receber a requisição utilizando o método has disponível atráves da <a href="https://laravel.com/docs/6.x/requests" target="__blank">request</a> e o formato da string utilizando a função <a href="https://www.php.net/manual/pt_BR/function.strpos.php" target="__blank">strpos</a> do PHP.

{% gist 7e82c10574abac3c09406450926bc198  %}

Caso o formato ou a string de envio não seja enviado no formato correto, iremos retornar uma mensagem com o <a href="https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Status/422" target="__blank">status code HTTP 422</a> indicando assim que houve um erro.

Iremos precisar de um arquivo no formato base64 para isso irei utilizar essa **<a href="assets/img/posts/upload-de-arquivos-base64-via-api-laravel6/0-laravel-logo.png" target="__blank">imagem da logo do Laravel</a>** e também o site <a href="https://www.getpostman.com/" target="__blank">base64-image.de</a> para converter a imagem para o formato base64. Para converter o arquivo de imagem, basta acessar o site base64-image.de clicar ou arrastar o arquivo para "Drag & Drop Images Anywhere or click Here" e esperar o processo de conversão. 

![base64-image.de-conversao](/assets/img/posts/upload-de-arquivos-base64-via-api-laravel6/1-base64-upload-converte.png)

Depois de converter o arquivo para visualizar o código base64 da imagem clique no botão **"</>show code"**

![base64-image.de-obter-base64](/assets/img/posts/upload-de-arquivos-base64-via-api-laravel6/2-base64-convertido-copiando-codigo.png)

E copie o código base64 que lhe foi apresentado.


Após a realização dessa definição inicial, precisamos de um cliente para realizar as requisições HTTP para o nosso endpoint para isso irei utilizar o <a href="https://www.getpostman.com/" target="__blank">Postman</a>.

Com o Postman ou outro Cliente HTTP devidamente instalado, iremos "criar" a requisição com as seguintes caracteristicas: 
verbo HTTP: POST endereço: **http://localhost:8000/api/send-file**, corpo da requisição no formato JSON com os seguintes dados:

{% gist 0768dd412663dcf73bb5d7b169899cd8 %}


![postman-request](/assets/img/posts/upload-de-arquivos-base64-via-api-laravel6/3-postman-request.png)

Caso realize o envio, você irá obter o valor da string base64 que foi enviado:


![postman-get-base64](/assets/img/posts/upload-de-arquivos-base64-via-api-laravel6/4-base64-requisicao.png)

Note que essa string vem com a descrição do tipo de arquivo (entensão que esta sendo enviado), iremos utilizar essa informação para após realizar o base64_decode enviar o mesmo com a extensão correta. Volte até o arquivo **app/Http/UploadController.php** vá até o metódo **sendFile** retire o **dd($base64)** e adicione as seguintes linhas:


{% gist 75aa9a78037c36531d3132d19998606c %}

Caso você realize o no primeiro explode **dd($extension = explode('/', $base64));** irá ver a separação da string em array onde os indices são delimitados pela incidência do caractere /(barra). Já o segundo explode se você realizar um **dd($extension = explode(';', $extension[1]));**, irá notar algo interessante, um array com dois indices onde o primeiro é a extensão do arquivo sem o ponto e o segundo é o inicio do base64, iremos utilizar esse primeiro indice pois ele é a extensão propriamente dita.

![postman-explode-extesion](/assets/img/posts/upload-de-arquivos-base64-via-api-laravel6/5-base64-obter-extensao.png)

Depois iremos gerar um nome de forma aleatória para o arquivo, obter o arquivo a string base64 sem os dados da extensão e realizar o envio, com isso o nosso método fica da seguinte forma:

{% gist 242123b0120d532c5b92a2897cef64c9 %}

Note que foi definido um path para envio do arquivo.O path publico do storage utilizando o disco local do projeto que fica localizado em **Storage/app/public** o path posterior chamado **"base64-files"** foi criado durante o envio do arquivo.

Ao utilizar Storage::put sem definir para qual disco você quer enviar o arquivo ou definindo o disco "local" o mesmo já acessa o seguinte caminho **Storage/app**, faltando apenas o restante do caminho que foi definido na varíavel **$path = 'public/base64-files/'**

Após a explicação sobre o path que irá armazenar os arquivos vá ao postman ou o cliente que você escolheu para realizar as requisições e faça a requisição novamente. Se a requisição estiver no formato correto, você irá obter uma resposta parecida com a que esta sendo apresentada abaixo:

![postman-send-file-success](/assets/img/posts/upload-de-arquivos-base64-via-api-laravel6/6-resposta-file-sucesso.png)

Para ver o arquivo que foi enviado vá até a pasta **Storage/app/public/base64-files** e veja se o nome retornado na resposta é o mesmo nome do arquivo:

![file-sended](/assets/img/posts/upload-de-arquivos-base64-via-api-laravel6/8-visualizando-caminho-arquivo-enviado.png)


Caso envie uma requisição que não esteja no formato correto, você irá obter a seguinte resposta:

![postman-send-file-success](/assets/img/posts/upload-de-arquivos-base64-via-api-laravel6/6-resposta-file-sucesso.png)

Para acessar o arquivo que foi enviado, iremos criar um link simbólico para isso vá até o terminal/cmd e digite o seguinte comando:

{% gist 5c0ca6e019962f1189588d03eb63196a %}

Realiza a criação do link simbólico abra o browser e digite **localhost:8000/storage/base64-files/nome-do-arquivo.extensao**, conforme exemplo apresentado abaixo:


![access-file](/assets/img/posts/upload-de-arquivos-base64-via-api-laravel6/9-acessando-arquivo.png)


<a href="https://github.com/paulodutra/upload-base64-laravel6" target="__blank">Reposítório do Projeto</a>

E ai o que achou ? comente abaixo.
























