---
url: ./onion/
date: 2021-07-12T17:14:13+01:00
title: "Haz tu web accesible desde Tor"
draft: true
image: /images/onion.png
categories:
    - howto
tags:
    - website
    - tor
    - onion
---

¿Por qué no?

<!--more-->

## ¿Por qué?

¿Acaso no son criminales los que usan Tor?
[Más o menos](https://2019.www.torproject.org/about/torusers.html.en). También lo usan aquellos que no pueden navegar la web y/o exponer sus opiniones sin ponerse en riesgo debido a ciertas restricciones o directamente represión sistemática. La gente que no quiere que ciertas búsquedas sean públicas, periodistas, chivatos (whistleblowers) y gente preocupada por su privacidad son usuarios regulares u ocasionales de Tor.

Claro que de por si ésto no es suficiente para estar 100% seguro y a salvo, pero la opsec va más allá del objetivo de éste post. Piénsalo así: Si crees que *tal vez* necesites usar Tor, probablemente **deberías**. 

Además, es super simple así que ¿Por qué no hacer tu web accesible para los usuarios de Tor? Considera que incluso si no lo usas hoy, podrías hacerlo en el futuro.

## Instalación

Ya que probablemente uses un sistema basado en Debian, y que Tor no está en los repositorios estándar, tendremos que hacer el ritual de siempre:

### Añadir los repos de Tor

```
apt install -y apt-transport-https gpg
echo "deb https://deb.torproject.org/torproject.org buster main
deb-src https://deb.torproject.org/torproject.org buster main" > /etc/apt/sources.list.d/tor.list
```

### Añadir la clave gpg

```
curl -s https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | gpg --import
gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | apt-key add -
```

### Actualizar e instalar Tor

```
apt update
apt install tor deb.torproject.org-keyring
```

## Habilitar

Abre `/etc/tor/torrc` en tu editor favorito, busca las línas
`HiddenServiceDir /var/lib/tor/hidden_service/`
y
`HiddenServicePort 80 127.0.0.1:80`
y descomentalas.

Arranca el servicio con `systemctl enable --now tor`.

Obtén tu dirección onion con `cat /var/lib/tor/hidden_service/hostname`.

## Sirve la página

Si sabes [cómo servir tu página con `nginx`](https://unixmagick.xyz/en/nginx/#install--configure) ésto no será nada nuevo.

Simplemente crea tu archivo de configuración de `nginx` para la página onion abriendo `/etc/nginx/sites-available/tu-web-onion` con tu editor de texto. Luego pega y ajusta estas líneas:

```
server {
	listen 80 ;
	root /var/www/web-onion ;
	index index.html ;
	server_name dirección-onion.onion ;
}
```

Asegúrate de hacer un symlink del archivo de configuración a `/etc/nginx/sites-enabled` y recarga `nginx`.

Si algo de ésto te resulta difícil de seguir, ve [aquí](https://unixmagick.xyz/en/nginx/#install--configure) y vuelve más tarde.

![yay](../../../images/gremione.gif)
