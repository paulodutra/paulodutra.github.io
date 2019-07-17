---
layout: post
title:  "MongoDB - Visão Geral"
date:   2019-07-17 18:27:00 -0200
categories: blog, tecnologia, banco de dados, nosql, mongodb
---

Fala galera, no post de hoje iremos falar um pouco sobre o **<a href="https://www.mongodb.com/" target="__blank">mongodb</a>** e também sobre a visão NoSQL(not only sql) ou trazendo para um contexto complementar **não relacional**. 

Alguns exemplos de tipos de banco de dados NoSQL: 

- **Chave/valor**: muito usado para cache o **<a href="https://redis.io/" target="__blank">Redis</a>** é um exemplo desse tipo de banco de dados;
- **Documento**: como é o caso do mongoDB que armazena documentos do tipo **<a href="https://www.json.org/json-pt.html" target="__blank">JSON</a>**;
- **Grafo**: baseado em conceitos matemáticos de grafos, vertices e arestas e afins, indicado para trabalhar em projetos de redes sociais e para dados com estrutura do tipo "rede". exemplos **<a href="http://graphdb.ontotext.com/" target="__blank">GraphDB</a> e <a href="https://neo4j.com/" target="__blank">Neo4j</a>**;
- **Coluna**: column family database, são exemplos desse tipo de banco o **<a href="https://cloud.google.com/bigtable/docs/overview?hl=pt-br" target="__blank">BigTable(Google)</a> e <a href="http://cassandra.apache.org/" target="__blank">Cassandra(apache)</a>**.

Existe também outros tipos de banco de dados não citados acima, um exemplo é o **<a href="https://www.datomic.com/" target="__blank">Datomic</a>** que trabalha com o conceito de dados imutáveis.

O mongoDB é baseado em **documentos** é um banco sem **esquema**. Exemplo de um banco com esquema: quando estamos trabalhando com banco de dados relacional temos o conceito de DDL (data definition language), onde definimos os dados tais como tabelas, colunas, definir os tipos de dados dessas colunas, quais são as restrições de tamanho, depois dessa definição ao manipular dados ou inserir novos dados todo um processo de validação é aplicado em cima do "esquema" definido. 

O local onde armazenamos os dados no mongo são chamados de **collections** (equivalente a uma tabela no modelo relacional), essa(s) collection(s) armazena documentos no formato JSON. Esses documentos podem possuir uma estrutura interna, **<em>entretanto essa estrutura pode sofrer modificações "estruturais"</em>** a qualquer momento sem impactar o que já foi armazenado, exemplo, temos uma collection chamada usuário e essa collection possui nome e telefone, depois em outro momento podemos ter um documento com nome, telefone e e-mail e depois posteriormente possuir um documento no formato anterior.

O formato JSON, pode armazenar chave e valor, objetos, array, array de objetos, multi arrays, multi objetos entre outros.

Nos documentos persistidos no mongo por padrão é criado uma chave _id, que armazena o **object id**, basicamente é uma especie de hash, similar a um **uuid**. 

Foi projetado com o intuito de escalonamento horizontal ou seja trabalhar de forma distribuída (cluster) onde podemos ter mais de um servidor executando o mongoDB e ao precisar de mais processamento, seja essa necessidade temporária ou não, pode ser criado outro servidor, ao invés de aumentar os recursos do servidor atual (escalonamento vertical) apesar que o escalonamento vertical também ser uma possibilidade. 

Geralmente é utilizado em cenários onde os dados são voláteis e não necessitam de certos relacionamentos entretanto o modelo orientado a documentos pode ser ideal para trabalhar com alguns tipos de cenários e não indicados para trabalhar em outros (tais como transações financeiras por exemplo). 

Atualmente a chance de trabalharmos em cenários híbridos, onde um determinado software, utilize os dois ou mais cenários é cada vez mais recorrente, tirando um pouco esse viés de que um tipo de banco de dados é melhor do que o outro, e aplicando aquele tipo no cenário adequado a ele. E ai o que achou ? comente abaixo.
 

 




