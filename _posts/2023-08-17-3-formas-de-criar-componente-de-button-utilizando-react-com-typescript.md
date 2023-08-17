---
layout: post
title:  "3 formas de criar componente de button utilizando React com typescript"
date:   2023-08-13 13:18:00 -0200
categories: blog, tecnologia, javascript, react
---

Fala pessoal, irei apresentar 3 formas de definir tipos para propriedades html, irei apresentar como caso de uso a propriedade button, entretanto as formas podem ser utilizadas com outras propriedades html. 
A primeira forma é definir um type para o componente onde todas as propriedades são especificadas, inclusive as propriedades nativas da tag html. Esta forma pode ser mais trabalhosa, tendo em vista que todas as propriedades nativas na tag devem ser informadas na definição do type e caso você queira obter os elementos filhos do componente também deverá definir uma propriedade children do tipo ReactNode.

```javascript
import React, { ReactNode } from 'react'

type ButtonProps = {
    lenghtButton?: string;
    children: ReactNode;
    onClick?: () => void
};
export const Button = (props: ButtonProps) => {
  return (
    <button onClick={props.onClick} style={{fontSize: props.lenghtButton}}>
        {props.children}
    </button>
  )
}
```
Para evitar a definição do children como uma propriedade, você pode utilizar na definição do type o PropsWithChildren que automaticamente encapsula a propriedade children.  Entretanto as propriedades nativas da tag html ainda necessitam de definição no type. 

```javascript
import React from 'react'

type ButtonProps = React.PropsWithChildren<{
    lenghtButton?: string;
    onClick?: () => void
}>

export const Button = ({children, lenghtButton, onClick}: ButtonProps) => {
  return (
    <button onClick={onClick} style={{fontSize: lenghtButton}}>
        {children}
    </button>
  )
}
```
Para resolver tanto a questão do children quanto a questão de não precisar definir as propriedades nativas da tag, você pode utilizar o ComponentProps<’informe-aqui-a-tag-html’> e adicionar as propriedades extras que irá precisar no seu componente, conforme o exemplo abaixo:

```javascript
import React from 'react'

type ButtonProps = React.ComponentProps<"button"> & {
    lenghtButton?: string
}
export const Button = ({children, lenghtButton, ...props}: ButtonProps) => {
  return (
    <button 
        style={{fontSize: lenghtButton}}
        {...props}
    >
        {children}
    </button>
  )
}
```
