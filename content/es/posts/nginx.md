---
url: ./nginx/
date: 2021-07-12T11:50:38+01:00
title: "Como servir tu web con Nginx"
draft: true
image: /images/nginx-logo.png
categories:
    - howto
tags:
    - website
    - nginx
    - vps
---

¡Es más simple de lo que parece!

<!--more-->

Vamos a suponer que ya tienes o bien un servidor local o bien un VPS con acceso root, así como un nombre de dominio registrado.

## Conéctate a tu servidor

<img style="float: left; margin: 3%" src="../../../images/ssh.png">

```
ssh root@tuweb.org
```

Éste comando te pedirá tu contraseña, tecleala y logueate en tu servidor/VPS.

Si ves un mensaje de error puede deberse a tres motivos:
- **Configuración DNS incorrecta**: revisa con tu proveedor de dominio y mientras tanto usa la IP de tu VPS en lugar de tu dominio.
- **Registro DNS aún no propagado**: puede que tenas que esperar un poco para que los registros se actualicen. Como antes, usa la IP mientras tanto.
- **Host SSH antiguo**: puede que tengas una conexión ssh antigua con esa URL (si has tumbado el servidor o reutilizas una URL vieja).

	Lanza un `cp .ssh/known_hosts .ssh/known_hosts.old`, por si acaso.
	Revisa en `.ssh/known_hosts`, busca una o más líneas que comiencen con tu dominio y elimínalas.

## Instala & Configura 

![nginx](../../../images/nginx.png)

Suponiendo que usas un sistema basado en Debian:

```
apt update
apt upgrade
apt install nginx
```

Nginx es un servidor web. Puedes decirle dónde está tu web y como hostearla en la red.
Funciona mediante un directorio de `sites-available`, que contiene la configuración para cómo nginx debe servir una web dada, y tiene un symlink a `sites-enabled`.

¡A la carga!

### Configuración

Comencemos creando esos archivos de configuración:
```
nano /etc/nginx/sites-available/tuweb
```
Nombra `tuweb` lo que te parezca.

Copia la configuración por defecto:

```
server {
	listen 80 ;
	listen [::]:80 ;
	server_name tuweb.org ;
	root /var/www/tuweb ;
	index index.html index.htm index.nginx-debian.html ;
	location / {
		try_files $uri $uri/ =404 ;
	}
}
```

De nuevo, llama `tuweb` como quieras, o mejor dicho, llámala como tu **nombre de dominio**.


##### ¿Qué está pasando?

<img style="float: right; margin: 2%" src="../../../images/wtf.gif">

- **listen**: Le dice a nginx que escuche en el puerto 80 tanto para IPv4 cómo para IPv6.
- **server_name**: Cuando alguien se conecte a éste servidor y busque esa dirección, serán dirigidos a la ruta a la que apuntamos a `root`.
- **root**: Como se dijo arriba, ésta es la ruta para tu web dentro del servidor. Puede ser lo que quieras, pero es más o menos estándar ponerlas en `/var/www/`.
- **index**: El punto de entrada de tu web.
- **location**: Le dice al servidor cómo buscar archivos, de lo contrario devuelve un error 404.

Una vez ésto esté listo --y hayas creado tu web en el directorio especificado--...

## Habilita la página

Usa `ln -s` para crear un symlink del archivo de configuración y recarga (o resetea) nginx para que la nueva configuración se aplique:

```
ln -s /etc/nginx/sites-available/tuweb /etc/nginx/sites-enabled
systemctl reload nginx
```

Esto **debería** devolverte una web funcional (aunque insegura).

Probablemente tu VPS venga con ajustes de firewall estrictos, para prevenir que hagas alguna estupidez.

## Abre el Firewall

Con algo de suerte tu VPS vino con `ufw` pre instalado. Si no, asegúrate de [instalar, arrancar y habilitarlo](https://unixmagick.xyz/en/ufw/) tu mismo. En cualquier caso tendrás que lanzar estos comandos:

```
ufw allow 80
ufw allow 443
ufw limit 22
```

Éstos comandos permiten/limitan el tráfico entrante por los puertos 80 (http), 443 (https) y 22 (SSH).
Limitamos el puerto 22 para controlar las masas de spambots que intentan constantemente entrar por fuerza bruta en cualquier servidor público.

#### Importante: Asegúrate de no bloquear las comunicaciones ssh, podrías encerrarte fuera de tu servidor/VPS.

Esto debería permitir que tu página web sea accesible desde la web aunque aún **de una forma poco segura**. Hablando de lo cual, aprovechemos ese puerto 443 que dejamos abierto y solucionemos el problema.

## Se la S en HTTPS

<img style="float: left; margin: 2%" src="../../../images/lock.png">

Hay muchas formas de hacer ésto, pero con diferencia la más simple es usar `certbot`.

```
apt install python-certbot-nginx
certbot --nginx
```

Simplemente sigue las instrucciones, es super fácil.
Te pedirá tu correo, qué dominio certificar y si quieres redirigir el tráfico HTTP a HTTPS. Ésta es sin duda la maner correcta de proceder, así que dale caña a la opción 2.

### Bonus

Los certificados creados de éste modo deben renovarse cada tres meses.
Para eso mismo te pide un correo: para notificarte de certificados a punto de caducar.

¡Automatízalo con cron!

```
crontab -e
```

Pega `0 0 2 * * certbot --nginx renew` en el archivo para crear un cronjob que solicita automáticamente a `certbot` que renueve todos los certificados, y que lo haga cada dos meses.
