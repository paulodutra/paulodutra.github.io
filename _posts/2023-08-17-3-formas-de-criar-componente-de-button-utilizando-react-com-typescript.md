---
layout: post
title:  "3 formas de criar componente de button utilizando React com typescript"
date:   2023-08-17 13:18:00 -0200
categories: blog, tecnologia, javascript, react
---

Fala pessoal, irei apresentar 3 formas de definir tipos para propriedades html, irei apresentar como caso de uso a propriedade button, entretanto as formas podem ser utilizadas com outras propriedades html. 

A primeira forma é definir um type para o componente onde todas as propriedades são especificadas, inclusive as propriedades nativas da tag html.

Esta forma pode ser mais trabalhosa, tendo em vista que todas as propriedades nativas na tag devem ser informadas na definição do type e caso você queira obter os elementos filhos do componente também deverá definir uma propriedade children do tipo **ReactNode**.

{% gist 7c2eff090dc2c4e5ba8d2f5639c8b44f %}

Para evitar a definição do children como uma propriedade, você pode utilizar na definição do type o **PropsWithChildren** que automaticamente encapsula a propriedade children.  Entretanto as propriedades nativas da tag html ainda necessitam de definição no type. 


{% gist 0e082388d9b2cac067b8eb19c42ac96b %}

Para resolver tanto a questão do children quanto a questão de não precisar definir as propriedades nativas da tag, você pode utilizar o **ComponentProps<’informe-aqui-a-tag-html’>** e adicionar as propriedades extras que irá precisar no seu componente, conforme o exemplo abaixo:

{% gist 124a65b23d13ec7f7ee60ed4225acded %}

