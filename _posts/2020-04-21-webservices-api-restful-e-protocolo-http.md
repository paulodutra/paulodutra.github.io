---
layout: post
title:  "Webservices, API RestFul e Protocolo HTTP"
date:   2020-04-21 14:00:00 -0200
categories: blog, tecnologia, webservices, API, Restful, Http
---

Fala galera, o post de hoje iremos falar sobre alguns conceitos de Webservices, API RestFul e Protocolo HTTP.

# O que é um webservice ?

Webservice é um software que permite a interoperabilidade de maquina a maquina, suas interações acontecem normalmente utilizando o protocolo HTTP. 
Mais informações em <a href="https:www.w3.org/TR/ws-arch/#whatis" target="__blank">https:www.w3.org/TR/ws-arch/#whatis</a>

Exemplos de webservices:

- Informações públicas;
- Redes sociais;
- Negócio;
- Serviços;
- Logística;
- Mobile;
- Internet das coisas (IOT) geladeira, cadeira, veículo;
- Entre outros fins.

## Diferença entre Webservice VS API

API é um grupo de rotinas, protocolo é métodos que permite a comunicação entre aplicações. (Application Programming Interface ou interface de programação de aplicações); Veja abaixo algumas diferenças:

- Um Webservice é uma API;
- Nem toda API é um Webservice, ex: BIOS, DOM(Document Object Model, para acessar e manipular informações do HTML), Code do PHP, etc;
- Webservice está ligado a HTTP, REST, SOAP, XML-RPC;
- Em outras palavras Webservice é uma API voltada para a web.

## RestFul e Protocolo HTTP

Proposto por Roy Fielding nos anos 2000, RestFul é uma arquitetura que faz uso do protocolo HTTP, padroniza uma interface para gerência de recursos e manipulação do mesmos através da troca representacional de estados. 
Restful significa: **Re**presentational **S**tate Tranfer ou Transferência de estado representacional, é orientado a resource ou seja a recursos,  O **ful representa a implementação de fato do REST**, ou seja esta implementando a arquitetura REST em nossa API. É **Stateless** ou seja não guarda estado (não armazena sessão ou cookie).

Um servidor Restfull, pode ser consumido por aplicações clientes (AngularJS, View.JS, Guzzle, CURL, ReactJS entre outros) ou em aplicações que rodam no lado do servidor (essa aplicação pode utilizar um servidor restfull, para lhe prover as informações). 

Restful trabalha métodos HTTP, Status code e cabeçalhos HTTP e permite a negociação do tipo de conteúdo a ser retornado através de content-type (que pode ser especificado no header da requisição) como por exemplo: json, xml, txt entre outros.

Outra prática comum em aplicações do tipo restful é trabalhar com versões, como por exemplo **http://servidor.com/api/v1/produto.json** aonde o **v1 é a versão** é o **produto é o resource** ou seja o recurso também conhecido como **endpoint**.

Cada verbo HTTP possui um significado, (entretanto você pode utilizar o verbo para o significado que você desejar), veja abaixo a convenção para os verbo HTTP: GET, POST, PUT, DELETE, PATCH, OPTIONS:

-**GET:** Consultar informações é considerado "Seguro" no ponto de vista de não fazer nenhuma alteração nos dados da API em si, exemplo: GET/clientes;

-**POST:** Cria um novo recurso é considerado "Não Seguro" no ponto de vista de fazer alteração nos dados API em si (Porque iremos alterar o estado da nossa API), exemplo: POST/pedidos;

-**PUT:** Atualiza um recurso existente é considerado "Não Seguro" no ponto de vista de fazer alteração nos dados API em si (Porque iremos alterar o estado da nossa API), exemplo: PUT/pedidos/2320;

-**DELETE:** Exclui um recurso existente é considerado "Não Seguro" no ponto de vista de fazer alteração nos dados API em si (Porque iremos alterar o estado da nossa API), exemplo: DELETE/pedidos/4060;

-**OPTIONS:** Consulta informações na API,é considerado "Seguro" no ponto de vista de não fazer nenhuma alteração nos dados da API em si (Porque iremos alterar o estado da nossa API), exemplo: OPTIONS/clientes.


O **OPTIONS é uma forma do  cliente identificar quais recursos estão disponíveis**, ou seja caso a API implemente o cliente pode passar OPTIONS/clientes e receber quais recursos estão disponíveis. Também serve para consultar quais HEADERS podemos passar em nossa requisição. Muito implementado em libs javascript onde antes dela enviar as requisições de fato ela manda um option e recebe quais recursos estão disponíveis, e caso não tenha a requisição desejada aborta a mesma.

Sendo assim, vamos ver abaixo um exemplo de rota e a utilização de cada verbo:

**GET** - http://servidor.com/api/v1/produto - Lista varios itens
**GET** - http://servidor.com/api/v1/produto/1 - Visualiza um item
**POST** - http://servidor.com/api/v1/produto - Insere
**PUT** -  http://servidor.com/api/v1/produto/1  - Atualiza
**DELETE** -  http://servidor.com/api/v1/produto/1 - Remove 

**OBS**: Tem APIS que pode utilizar o verbo POST com o id ou identificador na URL para atualizar ou usar o PUT e caso o recurso informado não existir ele acabar criando.
Os itens de urls encontrados de API para API podem variar ou seja podem ser maiores de acordo com a necessidade.

## Exemplo de uso de cada classe de verbo HTTP

|2xx - Tudo certo   | 3xx - Alteração de estado         |4xx - Erro no cliente        |5xx - Erro no servidor 
|-------------------|-----------------------------------|-----------------------------|-------------------------------
| 200 - Tudo ok     | 301- Redirecionamento permanente  | 404 - Página não encontrada | 500 - Erro no servidor     
|                   | 302 - Redirecionamento temporário | 422 - Falha na validação    |   
|                   |                                   |                             |   

**OBS**: Existe um RFC que especifica a definição dos Status code é a RFC 2616 Seção 10. A w3c orgão que ajuda a padronizar os padrões de acessibilidade entre outros na WEB, possui um documento a respeito das definições dos status code segue link abaixo:

<a href="https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="__blank">https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html</a>










