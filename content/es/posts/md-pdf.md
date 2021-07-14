---
title: "Por qué no puedes renderizar tus libros como LeanPub"
url: ./md-pdf/
date: 2021-07-14T12:31:20+01:00
draft: true
image: /images/md.png
categories:
    - why
tags:
    - markdown
    - leanpub
    - pdf
    - pandoc
---

Respuesta corta: Porque no hay markdown estándar y probablemente nunca lo haya.

<!--more-->

## LeanPub

<img style="float: right; margin: 3%" src="../../../images/leanpub.png">

Primero y fundamental: LeanPub es tanto una tienda online como una plataforma de escritura y publicación.
Se basa en la idea en la idea del [Lean Publishing](https://leanpub.com/manifesto) según la cual los libros se publican '*sin acabar*' usando herramientas ágiles y varias iteraciones considerando el feedback del lector y actualizando la publicación a medida que el tema a tratar lo requiera.

Si esto suena familiar es porque nace del ideario Ágil (de '*Agile*') y por esto mismo es tremendamente seductor para publicadores y/o autores estrechamente relacionados con el ámbito de la informática, en el que tiene más impacto.

### Formato

Conociendo ya el perfil de quienes más usan este servicio, sorprenderá poco que se base en escribir en Markdown y usarlo para generar PDF, ePub y mobi (formatos con los que se presenta el libro al público).

Concretamente usan '*Lean Flavoured Markdown*' y más recientemente [*Markua*](http://markua.com/) ([no son del todo iguales](http://help.leanpub.com/en/articles/2030628-where-are-some-simple-sample-leanpub-flavoured-markdown-or-markua-books), pero tampoco muy diferentes), incluso tienen un [manual](https://leanpub.com/markua/read#leanpub-auto-differences-with-leanpub-flavoured-markdown
) para este último.

### Problemas... o no

Siempre es útil tener claro para qué es la herramienta que se está utilizando. Si estás teniendo problemas y frustraciones constantes con LeanPub como autor, convendría repasar [lo que se pretende con este servicio y lo que no](https://leanpub.medium.com/why-dont-i-use-leanpub-4ad2a77f5744).

Aclarado ésto, es cierto que hay ciertos descontentos más o menos razonables:

- En ciertos casos puede resultar excesivamente lento, más considerando que para hacer pruebas de formato hay que renderizar el libro una y otra vez.
- Hay ciertos [bugs](https://github.com/thephpleague/commonmark/issues/97) que pueden resultar algo cansinos.
- Es un proceso remoto, por lo que los autores (a menudo informáticos) no pueden modificar el comportamiento del sistema, ni solventar problemas por su cuenta.
- Tienen una [API](https://www.gnu.org/philosophy/open-source-misses-the-point.en.html) pero es exclusiva para cuentas Pro.
- Como te podrás imaginar por lo anterior, no es [FOSS](https://www.gnu.org/philosophy/open-source-misses-the-point.en.html).

Visto esto, ¿Qué alternativas tenemos?

## Todos los caminos llevan a Pandoc

![nginx](../../../images/pandoc.png)

Con poco que se investigue, es claro que [Pandoc](https://github.com/jgm/pandoc) es el centro del universo en cuanto a conversión de archivos desde y hacia [todo tipo de formatos](https://pandoc.org/).

Dispone de una infinidad de wrappers para hacer conversiones entre todo tipo de archivos más o menos comunes simplemente tirando un comando bien formulado:

`pandoc -f FORMATO-INPUT -t FORMATO-OUTPUT ARCHIVO-INPUT -o ARCHIVO-OUTPUT`

Es una herramienta comodísima y fundamental para entornos de trabajo en los que hay un manejo importante de documentos.

No solo eso, la enorme mayoría de las herramientas y servicios que ofrecen conversiones de archivos son simplemente Pandoc maquillado: Por detrás, todos se basan en este software para ofrecer sus servicios.

## Markdown

Puede resultar algo extraño al principio, pero cuesta pensar en un formato más cómodo para redactar texto.

El problema reside en que cada hijo de vecino implementa Markdown [como mejor le cunde](https://en.wikipedia.org/wiki/Markdown#Variants) y por desgracia las versiones que nos conciernen no tienen un uso demasiado extendido, por lo que no podremos contar con la ayuda de Pandoc. O al menos no directamente.

Por suerte resulta que las variantes usadas por LeanPub son supersets directos de Commonmark, un formato mucho más extendido y con wrappers propios para Pandoc. Con algo de suerte el siguiente comando te acercará a lo que buscas.

```
pandoc -f commonmark_x -t pdf tuLibro.md -o tuLibro.pdf
```

Si no es el caso, habrá que recurrir a `sed` para básicamente convertir el markdown en cuestión a un formato más comprensible para Pandoc. Pero incluso esto puede no ser suficiente.
Consideremos que Markua está pensado justamente para redactar libros sin pasar por procesos de formateo posteriores. No tiene porqué haber otra variante de Markdown que de la talla para lo que buscamos.

### ¿Y mi estándar?

<img style="float: left; margin: 3%" src="../../../images/md.png">

Llegado hasta aquí cabe preguntarse por la falta de estandarización con el formato Markdown.

Lo más parecido a esto es justamente el Commonmark del que hablábamos antes, y ha tenido cierta acogida.
Sin embargo Markdown es un lenguaje pensado para facilitar la vida a quién escribe (no a una máquina) y no todos sus usuarios tienen necesariamente las mismas necesidades. 

El propósito del lenguaje es justamente ser lo bastante laxo como para poder [adaptarse](https://twitter.com/gruber/status/507670720886091776) a los requisitos según las circunstancias. 

Una sintaxis rígida serviría de poco para muchos y de mucho para pocos.
