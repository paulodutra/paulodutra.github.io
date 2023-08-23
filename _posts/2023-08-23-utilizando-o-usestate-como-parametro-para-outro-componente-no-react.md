---
layout: post
title:  "Utilizando o useState como parâmetro para outro componente no React"
date:   2023-08-23 10:48:00 -0200
categories: blog, tecnologia, javascript, react
---

Fala pessoal, tem cenários que precisamos controlar o estado de um componente e ao mesmo tempo ter a lógica deste estado separada do local onde o estado é definido. 

Por exemplo, digamos que realizamos a definição de um useState em um componente chamado **App.tsx** onde temos um componente de button que a única função dele é incrementar o número de vezes  que o botão foi acionado. Podemos ter a definição da lógica de incrementar o número separada do local de onde definimos o estado do botão. 

App.tsx:

{% gist cad387074bd6f51b9998c59fd37db919 %}

Note que ao informar a propriedade chamada increment é passada informada uma função como parâmetro a mesma deve anotada com o tipo **React.Dispatch<React.SetStateAction<tipo-do-dado>>** neste caso number no outro componente. 

Button.tsx:


{% gist 117a43b39faa0158a09fd3cac1852ca1 %}

Já no componente que recebe o useState como parâmetro foi informado o evento e também uma função inline para manipular a lógica do estado, neste caso obter o valor da variável number + 1.