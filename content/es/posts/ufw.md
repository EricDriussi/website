---
url: ./ufw/
date: 2021-07-12T17:14:34+01:00
title: "Cómo usar UFW"
draft: true
image: /images/firewall.png
categories:
    - howto
tags:
    - ufw
    - firewall
    - iptables
---

¡Simple y sin complicaciones!

<!--more-->

## Lo que es

Uncomplicated Firewall es una forma fácil de interactuar con `iptables`, la manera estándar de cualquier sistema basado en Linux para controlar conexiones desde y hacia la red.

Normalmente lo verás o usarás en servidores, aunque puede --y debería-- estar en tu máquina principal.

## Output

Echemos un vistazo al output de una configuración bastante normal en un servidor web.

```
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed) 
New profiles: skip

To                           Action      From
--                           ------      ----
22                         LIMIT IN    Anywhere                  
80                         ALLOW IN    Anywhere                  
443                        ALLOW IN    Anywhere                  
22/tcp                     LIMIT IN    Anywhere                  
22 (v6)                    LIMIT IN    Anywhere (v6)             
80 (v6)                    ALLOW IN    Anywhere (v6)             
443 (v6)                   ALLOW IN    Anywhere (v6)             
22/tcp (v6)                LIMIT IN    Anywhere (v6)
```

La línea que comienza con `Default` nos dice las políticas por defecto para todo puerto no especificado. Las políticas más específicas se listan a continuación.

## Configuración

Si lanzamos `ufw status` justo después de instalarlo, obtendremos un simple `Status: inactive` por respuest.

Tiene sentido, vamos a configurar un setup básico para servidor como el anterior.

```
ufw default deny incoming # Bloquea todo tráfico entrante
ufw limit in 22 # Limita las conexiones SSH entrantes.
ufw allow in 80 # Permite las conexiones HTTP entrantes.
ufw allow in 443 # Permite las conexiones HTTPS entrantes.
ufw enable
```

#### Importante: Asegúrate de no bloquear la comunicación SSH, podrías quedarte fuera de tu VPS/Server.

Ahora si lanzas un `ufw status verbose` deberías ver básicamente el mismo output que antes (el formato puede variar).

### Eliminando reglas

Por ejemplo, si queremos eliminar las reglas HTTPS previas:

```
ufw delete allow in 443
ufw reload
```

### Afinando

También puedes cambiar el comportamiento por defecto así cómo afinar la política de entrada/salida puerto por puerto.
Puedes `bloquear` (`deny`), `rechazar` (`reject`), `limitar` (`limit`) or `permitir`(`allow`) tanto tráfico `entrante` (`in`) cómo `saliente` (`out`) para cualquier puerto que puedas necesitar, así como usar los mismos parámetros para definir el comportamiento por defecto.
