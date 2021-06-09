---
layout: post
title:  "Transformando uma string em array, através do Array.from"
date:   2021-06-09 10:13:00 -0200
categories: blog, tecnologia, JS, Javascript
---

Fala galera, o post de hoje iremos falar sobre o uso do Array.from.

O Array.from, transforma elementos que são similares a um array, no mesmo para que ele possa receber todos os métodos(funções) do prototype Array.

```
const text = 'Paulo';
console.log(Array.from(text));
```
O exemplo acima, irá transformar a string "Paulo" em um array, onde cada letra se tornará um item no mesmo.
OBS: A palavra reservada Array deve estar com o "A" maisculo por causa do prototype array.







