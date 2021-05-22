---
title: "Como usar Github con SSH"
date: 2021-03-19T17:50:39Z
url: /githubssh/
draft: true
image: /images/git-logo.png
categories:
    - howto
tags:
    - github
    - ssh
    - git
---

Github te da el huevo mandándote un _Deprecation Notice_ por correo al hacer push? Ponle fin.

<!--more-->

## Crear claves SSH

El siguiente comando crea un par de llaves: una pública (terminada en .pub) y otra privada.
[Copiaremos](http://niceadsl.xyz./posts/githubssh/#meter-clave-publica-en-el-repositorio-remoto) el contenido del archivo .pub en los ajustes de github.

```bash
ssh-keygen -t rsa -b 4096 -C "tu_mail@ejemplo.es"
```

Por defecto lo guardará en `~/.ssh/`, sería recomendable tirar un `mkdir ~/.ssh/github` para almacenar las varias claves y asignarles nombres relevantes, ya que acabaremos con un par de claves por repositorio.

#### Importante:

No tienes porque preocuparte del `passphrase` SI NO TE PREOCUPA LA SEGURIDAD, de lo contrario procura que sea una frase corta y suerte con recordarla!

## Actualizar el agente local

#### Nota:

Deberás repetir el paso siguiente antes de cada push.
Facilitate la vida lanzando `git config credential.helper store` para que git recuerde las credenciales del repo en cuestión.
Usa el tag `--global` para que ésto se aplique siempre a todos los repositorios locales.

Dentro del repositorio a autorizar:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/github/NOMBRE_CLAVE
```

## Meter clave publica en el repositorio remoto

En la página del repositorio en github:
`Settings > Deploy keys > Add deploy key`

![sample](../../../images/sample.png)
Copiamos el contenido del archivo `~/.ssh/github/NOMBRE_CLAVE.pub` y lo pegamos en el campo _Key_.

## Test de acceso SSH

Siempre en el repositorio local

```bash
ssh -T git@github.com
```

Éste comando nos confirmará el acceso o la falta del mismo.
Si te responde con un _Permission denied_ vuelve atrás y lee con atención.

## Actualizar repo

Si ya tenías el repo montado desde antes (con HTTPS) te seguirá pidiendo nombre de usuario y contraseña.

Tendrás que actualizar la url de origen:

```bash
git remote set-url origin git@github.com:NOMBREUSUARIO/REPO.git
```

## Clonar

Si estabas acostumbrado a clonar con HTTPS el comando es levemente distinto.
Bueno, en todo caso el formato de la ruta, pero conviene tenerlo en cuenta sobre todo si usas el comando en tus scripts.

```bash
git clone git@github.com:NOMBREUSUARIO/REPO.git
```
