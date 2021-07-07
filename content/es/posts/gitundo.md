---
title: "Cómo deshacer con Git"
url: ./gitundo/
date: 2021-05-22T17:46:36+01:00
draft: true
image: /images/tardis.png
categories:
    - howto
tags:
    - git
    - github
    - deshacer
---

¡Reset, revert y más!

<!--more-->

Git es un programa cojonudo, pero también es casi tan **complejo** como se lo quiera hacer.
Hay varias formas de deshacer cambios con Git, en parte porque hace las veces de una máquina del tiempo.

### Hallar el id de un commit

Nos hará falta más adelante:

```
git log --pretty=oneline
```

No te asustes, es sólo **vi**. Se sale con `q`.

## Volver al último 'add'

### Caso

Lanzaste un `git add .` cuando todo estaba en orden y has seguido programando.
Te arrepientes de haber metido 4 `while` uno dentro del otro. ¿Como vuelvo atrás?

### Solución

Si hablamos de cambios en un solo archivo o directorio debería bastar con un `git checkout -- .`.
Si en cambio la metedura de pata es más sabrosa:

```
git clean -df
git checkout -- .
```

### Explicación

Sabiendo que Git trabaja en 3 '_niveles_' (_Working-dir, Staged o Index y Commited o HEAD_), sencillamente estamos volviendo al último Staged, descartando los cambios en el Working-dir.

## Volver al commit anterior

### Caso

Lanzaste un `git commit -m 'cool commit'` hace un rato y ha venido tu gato a pasearse sobre el teclado. Habrá que volver a empezar ¯\\\_(ツ)\_/¯ .

![cat](../../../images/cat-keyboard.gif)

### Solución

En éste caso querremos volver al estado previo, cargándonos todo lo nuevo de paso.

```
git reset --hard <id-del-commit-anterior>
```

Pero también podemos eliminar el último commit en si, sin perjudicar con ello los cambios que hayamos hecho.

```
git reset --soft HEAD~1
```

### Explicación

`git reset` deshace los cambios posteriores al commit (o HEAD) que le pasemos por parámetro.
Los tags `hard` y `soft` determinan si aquello a deshacer son los cambios en los archivos o los commits en sí mismos.

## Deshacer un merge

### Caso

Acabas de mergear una rama `feature` con la `develop` pensando que habías terminado la implementación.
Resulta que habías leído la **mitad** del ticket.

![facepalm](../../../images/facepalm.gif)

### Solución

Recordemos que para git, un merge no es más que un commit algo peculiar.

```
git reset --hard <id-del-commit-anterior>
```

### Explicación

Como el caso anterior, simplemente volvemos al estado previo al commit mergeador. Doy por obvio el tag `hard`, ya que cuesta imaginar un caso en el que tenga sentido la alternativa.

## Deshacer un cambio (o un merge) ya pusheado

### Caso

El clásico 'Que bien me ha qued... **OH DIOS NO**'.

![no](../../../images/no.gif)

### Solución

¡Que no panda el cúnico!

```
git revert -m 1 <id-del-commit/merge>
git push -u origin feature/o-lo-que-sea
```

### Explicación

Si `git reset` se carga los cambios y/o commits, `git revert` crea uno nuevo deshaciendo los cambios hechos en el commit que le pasamos por parámetro.
Recordemos que otros pueden estar trabajando con el **mismo** `origin` que nosotros, no podemos cargarnos aquello sobre lo que están trabajando así como si nada.

`-m 1` implica que git mantendrá la rama a hacia la cual se hizo el merge.
En caso de no tratarse de un merge, obviaremos el tag.

### Solución nuclear

Por si no fue suficiente.

```
git push -f origin <id-del-último-commit-válido>:<nombre-rama>
git reset --hard <id-del-último-commit-válido>
```

### Explicación

El primer comando obliga (`-f`) al repositorio remoto a apuntar al commit pasado por parámetro, cargándose los que hubiera entre éste y el HEAD.

El segundo como vimos más arriba, deja el repositorio local como estaba en el commit seleccionado, cargándose todos los cambios realizados entre éste y el HEAD.
Claro que si lo que queremos es arreglar algo nos ahorraremos este paso, corrigiendo lo necesario y haciendo un commit como si nada.

![prettycool](../../../images/prettygood.gif)
