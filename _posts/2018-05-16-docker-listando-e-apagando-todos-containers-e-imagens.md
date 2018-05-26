---
layout: post
title:  "Docker listando e apagando todos os containers e imagens"
date:   2018-05-26 16:02:23 -0200
categories: blog, tecnologia, docker
---
Fala galera, sei que a principio a palavra **apagar** não é tão amigavél assim.
Mas dependendo do cenário, as vezes estamos criando nossa imagem personalizada no [docker](https://www.docker.com/){:target="_blank"} e por algum motivo
decidimos apagar, todos os **containers e/ou imagens** e para não apagar de um por um, podemos excluir os mesmos de forma simples. 

Primeiramente para apagar, necessitamos ter o **id** ou o **nome** do container ou imagem podemos poder o(s) **id(s) do container** por meio do seguinte comando:

{% highlight bash %}
    docker ps -a -q
{% endhighlight %}

O comando **docker ps -a** lista todos os containers e o parâmetro **-q** faz com que o mesmo liste apenas o id, veja abaixo o resultado:
![listando-os-ids-containers](/assets/img/posts/docker-listando-apagando-containers-e-imgs/1-lista-todos-containers.png)

Para listar todos os **id(s) das imagens** podemos executar o seguinte comando:

{% highlight bash %}
    docker images -q
{% endhighlight %}

**docker images** lista todas as imagens e o parâmetro **-q** faz com que o mesmo liste apenas o id, veja abaixo o resultado:
![listando-os-ids-imagens](/assets/img/posts/docker-listando-apagando-containers-e-imgs/2-lista-todas-imagens.png)

Depois de listar todos o id de todos os containers e imagens, agora vamos ver como apagar todos os containers:

{% highlight bash %}
    docker rm $(docker ps -a -q)
{% endhighlight %}

Após executar o comando acima, a principio você tera apagado todos os containers, as vezes pode acontecer de um container ter dependência de outro e o mesmo não ser excluido, para força a exclusão acrescente -f (**docker rm $(docker ps -a -q) -f**) O resultado desta ação, será similar ao apresentado abaixo:
![apagando-todos-containers](/assets/img/posts/docker-listando-apagando-containers-e-imgs/3-apagando-todos-os-containers.png)

**docker rm** é utilizado para apagar os containers, o mesmo recebe como parâmetro o id ou nome do container **$(docker ps -a -q)** lista o id de todos os containers. 

Caso queira se certificar de que realmente foi apagado execute o comando **docker ps -a**. 
Para apagar todas as imagens execute o seguinte comando:

{% highlight bash %}
    docker rmi $(docker images -q)
{% endhighlight %}

**docker rmi** é similar ao comando **docker rm** a diferença é que o mesmo é utilizado para apagar imagens, também recebe como parâmetro o id ou nome da imagem. Já **$(docker images -q)** lista o id de todas as imagens. Veja abaixo o resultado:
![apagando-todas-imagens](/assets/img/posts/docker-listando-apagando-containers-e-imgs/5-apagando-todas-as-imagens-docker.png)
Assim como nos containers uma imagens também pode ter dependência de outra, para forçar a exclusão acrescente -f (**docker rmi $(docker images -q) -f**)

Caso queira se certificar de que as imagens foram realmente excluidas execute o comando **docker images**

Bom é isso, alguma vez você precisou apagar todas as imagens ou containers ? comente abaixo.



