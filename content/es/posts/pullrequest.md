---
title: "Cómo hacer una Pull Request"
url: ./pullrequest/
date: 2021-04-15T21:49:38+01:00
draft: true
image: /images/prpr.png
categories:
    - howto
tags:
    - github
    - pr
    - pullrequest
    - git
---

Quieres contribuir a un proyecto FOSS molón en Github?

<!--more-->

## Fork 🍴

Básicamente hacemos una copia del repo bajo tu cuenta personal. Simplemente pincha en el botón en la esquina superior derecha de la página.

![fork](../../../images/fork.jpeg)

Ahora tienes una copia funcional entre tus repos. De ahora en adelante llamaremos éste repo el '_fork_' y el original el '_upstream_'.

## Clone

Tira un `git clone FORKED-REPO` desde la línea de comandos para obtener una copia local de tu repositorio 'forked'.
Ahora tenemos algo con lo que trabajar.

## Branch 🌳

Mantengamos las cosas ordenadas, crea una rama nueva y muévete a ella para mantener tu trabajo separado.

```bash
git checkout -b pr-branch
```

¡Dale caña al código!

## Add, Commit & Push

¿Terminaste? Tal y como lo harías con un proyecto tuyo:

```bash
git add .
git commit -m "pull me pls"
git push origin pr-branch
```

<sub>¿Necesitas configurar [ssh](https://unixmagick.xyz/en/githubssh/)?</sub>

No te estreses, solo estás actualizando **TU** fork.

## Crea una pull request

![pr](../../../images/pr.png)

## Compara cambios

Aquí te asegurarás de que el PR se va a hacer **desde** el repo y rama correctos y **hacia** el repo y rama correctos.

![compare](../../../images/comparePR.jpg)

Rellena mensaje y descripción abajo y dale a `Crear Pull Request`.
¡Ya lo tienes!

## Revisa

Si todo fue bien deberías ver tu PR desde el repo 'upstream'.
Desde aquí también puedes dialogar con el desarrollador original.

Si hubiera algo que enmendar simplemente sigue haciendo commits y push desde tu repo local hacia tu rama `pr-branch`. Deberían aparecer automágicamente en el PR para revisión del desarrollador upstream.
