---
layout: post
title:  "Kotlin variável do tipo string utilizando null safety"
date:   2022-09-17 23:35:00 -0200
categories: blog, tecnologia, kotlin
---

Fala galera, recentemente estava estudando alguns conceitos básicos sobre o kotlin.
Um dos recursos que achei interessante, foi o **null safety** que consiste em uma forma de declarar uma variável do tipo string para previnir alguns erros durante a sua manipulação da.

De forma geral para declarar váriavel no kotlin, podemos utilizar a palavra reservada **val** ou **var**, sendo que val se assimila ao **const** no Javascript e **var** se assimila ao **let**. 
Ou seja val para valores constantes e var para valores que podem mudar ao longo do ciclo de execução. 

Após a escolha do tipo de declaração que atenda a sua necessidade, você informa o tipo de dado que aquela variável irá armazenar neste caso string, abaixo esta 
a forma padrão de declaração sem a proteção do null safety:

```kotlin
fun main() {
   var str: String = "my value"
}
```

Digamos que em algum momento você deseje sobreescrever o valor da variável acima, atribuindo um valor nulo, você irá notar que irá acontecer um erro de compilação:


```kotlin
fun main() {
  var str: String = "my value"
  str = null
  print(str)
  //será apresentado o seguinte erro: Null can not be a value of a non-null type String 
}
```
Entretanto se você fizer a mesma coisa, usando o null safety esta atribuição será permitida. Para declarar uma variável do tipo string com null safety, basta informar um ponto de interrogação após o tipo string:

```kotlin
fun main() {
  var str: String? = "my value"
  str = null
  print(str)
  //será apresentado o valor null  
}
```

Caso você tente acessar o tamanho da variável que esteja com null safety e a mesma esteja nula, é recomendado que você também utilize o ponto de interogação antes da palavra reservada length, para previnir que seja apresentado um erro:

```kotlin
fun main() {
  var str: String? = "my value"
  str = null
  print(str.length)
  //será apresentado o seguinte erro: Only safe (?.) or non-null asserted (!!.) calls are allowed on a nullable receiver of type String?

}
```

```kotlin
fun main() {
  var str: String? = "my value"
  str = null
  print(str?.length)
  //será apresentado o valor null  
}
```

Veja mais informações sobre null safety na documentação do kotlin: <a href="https://kotlinlang.org/docs/null-safety.html" target="__blank">https://kotlinlang.org/docs/null-safety.html</a>





