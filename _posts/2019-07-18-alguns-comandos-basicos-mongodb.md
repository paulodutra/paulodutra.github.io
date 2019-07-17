---
layout: post
title:  "Alguns comandos básicos do mongoDB"
date:   2019-07-17 18:28:00 -0200
categories: blog, tecnologia, banco de dados, nosql, mongodb
---


Fala galera, no post de hoje iremos falar um pouco sobre alguns comandos básicos do mongoDB. No **<a href="http://paulodutrainfo.com.br/mongodb-visao-geral" target="__blank">post anterior</a>** falamos um pouco sobre a visão geral dele e também comentamos sobre alguns tipos de 
banco de dados NoSQL.

Vamos começar falando sobre alguns comandos básicos do mongoDB. Para iniciar o serviço do mongo: 

{% gist 160d377d399c11932b2f57480baaf617 %}

![1-inicializando-o-servico-mongod.PNG](/assets/img/posts/alguns-comandos-basicos-mongodb/1-inicializando-o-servico-mongod.PNG) 

OBS: Abra outra aba no terminal e deixe essa executando o serviço do mongodb.

Após inicializar o serviço do mongo, digite no terminal: **mongo**, com esse comando você irá iniciar o mongo shell, deixando o terminal disponível para a execução de comandos.

{% gist 61804bbb0d24bbf9d4ee2bce37760ddf %}

![2-inicializando-o-mongo-shell.PNG](/assets/img/posts/alguns-comandos-basicos-mongodb/2-inicializando-o-mongo-shell.PNG) 

Para exibir os banco de dados existentes:

{% gist a3e2107a207604b09557a9090b1a72fa %}

![3-exibindo-os-databases.PNG](/assets/img/posts/alguns-comandos-basicos-mongodb/3-exibindo-os-databases.PNG) 

Para alterar o database informe: 

{% gist ff533abd70993c228e4c6dc09f2f3f19 %}

Exemplo: **use admin**.

![4-alterando-o-database.PNG](/assets/img/posts/alguns-comandos-basicos-mongodb/4-alterando-o-database.PNG) 

Para criar uma coleção, utilize:

{% gist cf64ad93ea9d90c17fbaa4806ce3b3d7 %}

Exemplo: **db.createCollection('myCollection')**.

Para exibir as collections de um database execute o seguinte comando: 



Entretanto se você informar um nome de um banco que ainda não existe ele irá realizar a alteração, (criando ele na memória) exemplo:

![5-criando-database-apenas-na-memoria.PNG](/assets/img/posts/alguns-comandos-basicos-mongodb/5-criando-database-apenas-na-memoria.PNG) 

e depois criar uma collection dentro dele: 

{% gist e6a35a8fee782f016a656a585c4d0529 %}

![6-criando-collection-em-database-criado-na-memoria.PNG](/assets/img/posts/alguns-comandos-basicos-mongodb/6-criando-collection-em-database-criado-na-memoria.PNG) 

e depois perdir para listar os bancos de dados, o mesmo será criado, porque uma collection foi atrelada a ele:

![7-exibindo-dbs-depois-de-criar-collection-para-db-em-memoria.PNG](/assets/img/posts/alguns-comandos-basicos-mongodb/7-exibindo-dbs-depois-de-criar-collection-para-db-em-memoria.PNG) 

O mongo considera letras maiúsculas e minusculas (camelcase) no momento de criação das collections ou seja se for criada uma coleção chamada myCollection e depois você tentar criar mycollection, será permitido.

![8-collections-camel-case.PNG](/assets/img/posts/alguns-comandos-basicos-mongodb/8-collections-camel-case.PNG) 

Para efetuar o drop (apagar) uma collection informe: 
{% gist 9125446e46b76f80b413990d28a4d643 %}

Exemplo:
![9-drop-collection.PNG](/assets/img/posts/alguns-comandos-basicos-mongodb/9-drop-collection.PNG) 

Ao apagar a outra collection criada no banco **"meubanco"** e depois pedir para exibir todos os bancos de dados, você irá notar que o mesmo será apagado, ou seja ao apagar todas as collections de um banco de dados no mongo o mesmo também é apagado.

![10-drop-ultima-collection-e-apaga-database.PNG](/assets/img/posts/alguns-comandos-basicos-mongodb/10-drop-ultima-collection-e-apaga-database.PNG) 

E ai o que achou ? comente abaixo.

