---
layout: post
title:  "Um pouco sobre o uso do array.includes"
date:   2021-06-09 10:13:00 -0200
categories: blog, tecnologia, JS, Javascript
---

Fala galera, o post de hoje iremos falar sobre o uso do array.includes

Antes da ES7, para verificar se um determinado elemento se encontra em um array,  era utilizado o indexOf e caso o mesmo não existisse o valor retornado é igual a -1.

```
const arr = [1, 2, 3];
console.log(arr.indexOf(3) > - 1);
```

Com a feature array.includes adicionada na ES7, basta informar o valor diretamente a chamada da função e a mesma retonará true caso o elemento exista no array e false para caso não exista. 

```
const arr = [1, 2, 3];
console.log(arr.includes(3)); //retornará true
console.log(arr.includes(5)); //retornará false
```









