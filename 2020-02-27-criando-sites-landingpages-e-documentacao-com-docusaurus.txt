---
layout: post
title:  "Criando sites, landing pages e documentações com docusaurus"
date:   2020-02-27 19:38:00 -0200
categories: blog, tecnologia, javascript, reactjs, docusaurus
---

Fala galera,neste post iremos falar sobre como criar sites, landing pages e documentações utilizando o <a href="https://docusaurus.io/" target="__blank">docusaurus</a>. Mas antes devemos saber o que é o docusaurus ? Docusaurus é um projeto open source, mantido pelo facebook e desenvolvido utilizando Javascript mais especificamente <a href="https://reactjs.org/" target="__blank">reactjs</a>.

Seu conteúdo é gerenciado de forma estática e após a publicação são gerados os arquivos em html, css, js, img e etc. 

Para utilizar o docusaurus é necessário instalar um pacote via <a href="https://www.npmjs.com/" target="__blank">npm</a> ou <a href="https://yarnpkg.com/" target="__blank">yarn</a>. Irei utilizar o gerenciado de dependências **npm**, para instalar o docusaurus de forma global execute o seguinte comando:


{% gist af5710bad9a80938b0dec37001ce21e3 %}

Após a instalação de forma global, iremos utilizar o docusaurus-init para a criação do boleiplait do projeto, para isso crie uma pasta, entre nessa pasta e posteriormente execute o comando: **docusaurus-init**,(isso é feito porque os arquivos do projeto vem "soltos" no diretório de instalação do projeto) conforme exemplo abaixo:

{% gist bb5f72f305c9ee826620f49567e011c5 %}


