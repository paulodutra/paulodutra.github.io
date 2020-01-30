---
layout: post
title:  "Salvando arquivos na AWS S3 via API no Laravel 6"
date:   2020-01-13 14:40:00 -0200
categories: blog, tecnologia, php, laravel
---

Fala galera, no <a href="https://bit.ly/2RbKGRA" target="__blank">post anterior </a> foi apresentada uma forma de realizar
upload de arquivos base64 via API utilizando o <a href="https://laravel.com/docs/6.x/" target="__blank">Laravel 6</a>. Neste
post iremos apresentar uma forma de utilizar o serviço <a href="https://aws.amazon.com/pt/s3/" target="__blank">S3 da AWS</a> como storage de arquivos, ou seja ao invés de enviar o arquivo para a pasta da aplicação, iremos enviar para um bucket na S3. 

Iremos utilizar o projeto que fizemos no post anterior como base, para isso execute os seguintes comandos:

{% gist d60ce0a0578cd818a110a75f3dbd0862 %}

Os comandos executados acima executaram as seguintes atividades: clone do projeto, renomeação de pasta, copia do arquivo de configuração e geração da chave **APP_KEY** da aplicação.

Antes de prosseguirmos com as configurações necessárias, é interessante comentar que além de armazenar arquivos localmente ou na S3, também é possível utilizar outras opções tais como: **ftp e sftp**, para maiores informações veja sobre <a href="https://laravel.com/docs/6.x/filesystem#driver-prerequisites" target="__blank">**file storage** na documentação</a>

Após realizar a configuração do projeto, precisamos instalar dois pacotes, um deles é o SDK AWS e o outro é um da <a href="https://thephpleague.com/pt-br/" target="__blank">The League PHP</a>, para isso iremos abrir o arquivo composer.json da aplicação e dentro do objeto require, iremos acrescentar as seguintes dependências:

{% gist 8118f825cd584ee95ead24674c12b855 %}

Agora será necessário instalar as dependências para isso vá até o terminal/CMD e execute: **composer update**

## Configurando bucket S3
Realizados os passos acima, vamos partir para a criação do bucket(espaço destinado ao armazenamento de arquivos na AWS). Efetue login no console e em seguinte busque por **s3**, conforme feito na imagem abaixo:

![pesquisa-s3-console-aws](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/1-pesquisa-s3-console-aws.png)

Em seguida localize a opção **criar bucket**:

![tela-criar-bucket-s3](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/2-tela-criar-bucket-s3.png)

Preencha o formulário com o **nome do bucket** e a escolha da **região** de uso do serviço e logo em seguida **clique em criar**. Neste caso optei pelo nome: aws-s3-files e a região: Oeste dos EUA (Oregon). 

**OBS:** a escolha do nome do bucket passa por uma validação, não podendo terminar com ponto e traço e também uma validação do nome em si do mesmo. Durante o processo de criação do bucket pulei alguns passos de configurações.

![nome-e-regiao-bucket](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/3-nome-e-regiao-bucket.png)

Veja que o bucket foi criado e nas informações de acesso esta indicando que o "Bucket e objetos não públicos", em outro passo iremos mudar essa configuração para poder acessar o arquivo após o envio. 

![s3-bucket-nao-publico-criado](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/4-s3-bucket-nao-publico-criado.png)

**Clique no nome do bucket** para que seja apresentado os arquivos do mesmo e as opções de configurações.

Logo em seguida clique em **Permissões** e depois em **Editar** e **desmarque** a opção **"Bloquear todo o acesso público"** e depois clique em **salvar**


![permissao-publica-bucket](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/5-permissao-publica-bucket.png)

Em seguida será apresentada uma tela de confirmação de edição de configurações de "Bloquear acesso público (configurações de bucket), execute os passos pedidos. No meu caso foi indicado digitar a palavra **confirmar** e logo em seguida clicar no **botão confirmar**

![confirmar-permissao-bucket-s3](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/6-confirmar-permissao-bucket-s3.png)

Realizadas essas configurações, iremos criar um usuário com a permissão de gerenciamento full de objetos S3.

**OBS:** Você pode utilizar um usuário existente que possua a permissão **"AmazonS3FullAccess"**.

## Configurando usuário no serviço IAM

Clique em **serviços** no canto superior esquerdo ao lado da logo e digite **iam** na barra de pesquisa. No meu caso a opção também foi apresentada na barra de histórico na lateral, após encontrar apresentar o resultado da busca clique na mesma.

![iam-criar-usuario-bucket](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/7-iam-criar-usuario-bucket.png)

Em seguida cliquem em **Usuários** na lateral superior esquerda e em seguida clique no botão **Adicionar usuário**

![usuarios-iam](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/8-usuarios-iam.png)

Preencha a primeira etapa defindo o **Nome de usuário** e o **Tipo de acesso**, escolhi o nome de usuário "s3-user" e no caso do tipo de acesso deve ser escolhido a opção **"Acesso programático"** para poder utilizar o serviço e em seguida clique no botão **"Próximo Permissões"**. 

![usuario-tipo-de-acesso-iam](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/9-usuario-tipo-de-acesso-iam.png)

**OBS:** Caso escolha apenas a opção **"Acesso programático"** este usuário não irá efetua login no console AWS.

Agora na etapa definir permissões **clique no botão "Anexar póliticas existentes de forma direta"** e localize a permissão **"AmazonS3FullAccess"**, selecione a mesma e em seguida **avançe as etapas 3 e 4**. 

![permissao-s3-usuario-iam](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/10-permissao-s3-usuario-iam.png)

Será apresentadas as seguintes informações **ID da chave de acesso** e **Chave de acesso secreta** (clicar em exibir para visualizar) na tela e também a opção **Fazer download .csv**. 

![download-chaves-de-acesso-usuario-iam](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/11-download-chaves-de-acesso-usuario-iam.png)

## Configurações finais do projeto e teste de envio 

**Baixe o arquivo .csv** e em seguida volte ao projeto. Abra o arquivo .env e adicione as seguintes configurações:

{% gist 531814eb337caf7a0b3d85082b432f4d %}

Em seguida abra o terminal/CMD e execute o seguinte comando: **php artisan serve**

Abra <a href="https://www.getpostman.com/" target="__blank">Postman</a> ou outro Cliente HTTP devidamente instalado e realize uma requisição POST para o seguinte endereço: **localhost:8000/api/send-file**, com o seguinte corpo em formato JSON:

{% gist 0768dd412663dcf73bb5d7b169899cd8 %}


![enviando-arquivo](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/12-enviando-arquivo.png)


Após realizar a requisição e a mesma tiver sido bem sucedida, veja o valor de file dentro do response. Vá até o console AWS novamente, selecione o serviço S3, note que as pastas **public/base64-files** foi criada e que dentro delas irá estar um arquivo de mesmo nome e formato retornado na resposta da API. 

**Clique no nome do arquivo** para exibir mais opções: 

![arquivo-no-bucket-s3](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/13-arquivo-no-bucket-s3.png)

Veja que agora foram apresentadas algumas opções **clique no botão Abrir**

![abrir-arquivo-bucket](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/13-1-abrir-arquivo-bucket.png)

O arquivo irá ser aberto em outra aba do browser, note que na url esta sendo passado dois parâmetros depois do nome do arquivo: 

- response-content-disposition=inline
- X-Amz-Security-Token


![abrir-arquivo-bucket-com-permissao](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/14-1-abrir-arquivo-bucket-com-permissao.png)

O <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition" target="__blank">**response-content-disposition**</a> se trata de como será exibido o arquivo (como uma página web, como parte de uma página web, ou como arquivo para download). 

X-Amz-Security-Token é um token de autorização que é adicionado na requisição, esse token pode ser configurado via SDK também, onde pode ser determinado o tempo de duração. 

Caso seja retirado o parâmetro **X-Amz-Security-Token** da url você irá notar que será retornada uma resposta no formato XML com o code **InvalidRequest**, isso significa que esse objeto(arquivo) do bucket não esta público.

![abrir-arquivo-bucket-sem-permissao](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/14-2-abrir-arquivo-bucket-sem-permissao.png)

Para acessar os arquivos sem informar o **X-Amz-Security-Token** iremos ter que deixar o bucket publico para isso retorne ao console AWS, selecione novamente o serviço S3, localize o bucket e em seguida clique em **Permissões** e depois em **Política de bucket**, será aberto editor, coloque as seguintes diretivas em formato JSON:

{% gist 7f405c7a65215f4f2ccc7bb574473849 %}

![politica-do-bucket-permissao-publica](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/15-politica-do-bucket-permissao-publica.png)

**OBS:** altere a o termo nome-do-bucket.

Após ter inserido a política do bucket clique em **Salvar**, note que agora esta sendo apresentado avisos falando que o bucket é público:

![aviso-bucket-publico](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/16-aviso-bucket-publico.png)

Agora ao acessar novamente o arquivo e retirar da url o parâmetro **X-Amz-Security-Token** o mesmo irá abrir normalmente:

![abrindo-arquivo-de-forma-publica](/assets/img/posts/salvando-arquivos-via-api-aws-s3-laravel-6/17-abrindo-arquivo-de-forma-publica.png)

<a href="https://github.com/paulodutra/salvando-arquivos-aws-s3-via-api-laravel-6" target="__blank">Reposítório do Projeto</a>

E ai o que achou ? comente abaixo.






