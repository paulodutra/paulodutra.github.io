---
layout: post
title:  "O uso da condicional when no kotlin"
date:   2022-11-09 14:59:00 -0200
categories: blog, tecnologia, kotlin
---

Fala pessoal, o when no kotlin representa uma estrutura condicional já conhecida, porém com outro nome. Esta estrutura é o switch case. No kotlin não há o switch case propriamente dito.

Para realizar a utilização do when é bem simples.Primeiro adicionamos a palavra reservada when seguida da condição que será analisada e após as verificações a condição padrão, caso nenhuma das condições presentes seja atendida, o bloco de código presente na condição else será executado. 


```kotlin
fun main() {
    val number1: Int = 10;
    //when similar switch
        when(number1) {
            1  -> println("1")
            2  -> println("2")
            10 -> println("10")
            else -> {
                println("no options")
            }
        }

}

```

Outro recurso interessante no kotlin é permitido atribuir o when ou uma condição if para uma variável:


```kotlin
fun main() {
    val number1: Int = 10
    val number2: Int = 20
    val max: Int = if(number1 > number2) {
        println("number 1 is bigger than number 2")
        number1
    } else {
        println("number 2 is bigger than number 1")
        number2
    }
    
    val result: Int = when(number1) {
        1  -> {
            println("1")
            1
        }
        2  -> {
            println("2")
            2
        }
        10 -> {
            println("10")
            10
        }
        else -> {
            println("0")
           0
        }
    }
}

```

Veja mais informações sobre a condição when na documentação do kotlin:  <a href="https://kotlinlang.org/docs/control-flow.html#when-expression" target="__blank">https://kotlinlang.org/docs/control-flow.html#when-expression</a>
