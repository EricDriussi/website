---
url: ./maintain/
date: 2021-07-12T17:15:19+01:00
title: "Como mantener un VPS"
draft: true
image: /images/server.png
categories:
    - howto
tags:
    - vps
    - mantenimiento
    - servidor
---

Un par de pistas.

<!--more-->

## SSH

La idea es generar una clave ssh en nuestro ordenador y hacer que el servidor 'confíe' en el (parecido a lo que hicimos [aquí](https://unixmagick.xyz/githubssh/)).
Las claves SSH son la manera más rápida y segura de acceder remotamente a tu servidor.

Nos permitirán evitar la contraseña root por completo, haciendo el acceso más rápido, automatizable y a prueba de ataques por fuerza bruta.

### Genera las claves

```
ssh-keygen -t rsa -b 4096 -C "tu@mail.si"
```

Éste comando debería pedirte una ruta en la que almacenar las claves (normalmente en `~/.ssh/`) así cómo un 'passphrase' (que puedes dejar en blanco).

Encontrarás un par de claves en la ruta especificada, mandemos una al servidor.

### Manda la clave pública al servidor

```
ssh-copy-id root@tudominio.com
```

Tendrás que introducir tu contraseña del VPS, después tu servidor autorizará el acceso a toda máquina con la clave SSH privada.

Logueate de nuevo. Si no te pide la contraseña es que ha funcionado.
De lo contrario revisa los permisos tanto para las claves como para el directorio `.ssh`.

### Desactiva los login mediante password

No es un paso necesario pero si quieres una razón para hacerlo simplemente mira el output de `journalctl -xe` en tu VPS.
Ahora que has visto lo que es el 'brute force', vamos a asegurarnos de que los spambots **nunca** consigan entrar.

Abre `/etc/ssh/sshd_config` y encuentra/modifica las siguientes líneas.

```
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no
```

Por supuesto, recarga ssh: `systemctl reload sshd`

##### ¿Y si pierdo la clave?

**No lo hagas. Las copias de seguridad están para algo.**

Tu proveedor de VPS siempre debería tener una opción para acceder desde la web. Normalmente se trata de una terminal en sesión local emulada en el navegador.

Siendo local a la máquina (o al menos actuando como tal), sólo te será accesible después de loguearte con tu proveedor VPS y no te pedirá más autenticación.
Una vez dentro, revierte los cambios anteriores.

## Rsync

A menudo necesitarás transferir archivos desde y hacia tu servidor.
Rsync es probablemente la manera más fácil de hacerlo: es rápido, seguro y simple.

Asegúrate de instalarlo tanto en tu máquina local cómo en tu servidor. Luego teclea (o más bien haz un alias para):

```
rsync -rtvzP /path/to/archivoLocal root@tuWeb.url:/ruta/en/el/servidor
```
<u></u>
Este comando se ejecutará <u>r</u>ecursivamente (incluyendo directorios), transfiriendo los <u>t</u>iempos de modificación (saltándose archivos no modificados), <u>v</u>isualizando los archivos subidos, comprimiendo (<u>z</u>?) los archivos para la subida de manera <u>P</u>arcial para poder retomarla en caso de fallo en la conexión.

Claro que, para descargar algo de la VPS, simplemente hay que invertir los parámetros en el comando.

## Cronjobs

Con algunas tareas rutinarias, es mejor que se encargue Cron.
Éste software se encargará de ejecutar cualquier comando con una frecuencia o patrón de repetición dados.

Supón por ejemplo, que quieres autormatizar las actualizaciones para tu servidor.
Podrías ejecutar `crontab -e` e insertar en el archivo algo cómo `30 2 * * 0 apt -y update && apt -y upgrade`. Asegurate de guardar antes de cerrar el archivo.

Vamos a ver el detalle:

```
 .------------------ minuto (0 - 59)
 | .--------------- hora (0 - 23)
 | | .------------ día del més (1 - 31)
 | | | .--------- més (1 - 12)
 | | | | .------ día de la semana (0 - 6)
 | | | | |
 * * * * *
30 2 * * 0 apt -y update && apt -y upgrade
```

Básicamente una versión para máquinas de '*por favor actualiza el sistema cada Domingo a las 2:10AM*'.

Puedes hacer de todo con cronjobs, y listar ejemplos no es el objetivo de éste post.
[Ésta](https://crontab.guru/) página sin embargo hace justamente eso.

## Búsquedas locales

Los sistemas de archivos de Linux se puede volver algo caóticos.
Puede que estés acostumbrado a `whereis` o `which` pero pueden ser insuficientes en función de la tarea.

`updatedb` puede indexar todo el sistema rápidamente, lo cual mola porque hay otra herramienta llamada `locate` que puede encontrar archivos y directorios cuyas rutas contengan cualquier string facilitada.

Así que si ejecutas `updatedb` y luego `locate cron`, verás una lista de archivos y directorios que contengan `cron` en sus nombres.

## Solucionar problemas

Tendrás que solucionar problemas a menudo trabajando con un VPS, y probablemente encuentres la causa en los logs.

Los logs del sistema pueden verse ejecutando `journalctl -xe` (con `-xe` ofrece resultados un poco más útiles). Claro que puedes hacerte la vida más fácil si ya tienes una idea de lo que andas buscando:

```
journalctl -xeu programaFallando
```

Ésto sólo mostrará entradas relevantes a tu `programaFallando`.

Claro que no todos los programas usan los logs del sistema. Éstos normalmente tendrán sus logs proprios bajo `/var/log/`. Hecha un vistazo, seguro que encuentras lo que andas buscando.

Otra herramienta útil es `systemctl`. No podrá darte logs, pero puedes usarlo para detener, resetear, recargar y arrancar servicios manualmente y/o revisar su estado:

```
systemctl status programaQueNoFunciona.service
```

Éste comando te dará mucha información acerca de ése servicio en concreto.
