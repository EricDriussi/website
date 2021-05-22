---
title: "Por qué usar un gestor de contraseñas"
url: ./pwdmanager/
date: 2021-03-31T19:49:26+01:00
draft: true
image: /images/pwd.png
categories:
    - why
tags:
    - password manager
    - bitwarden
    - keepassxc
---

La contraseña debe ser larga, no puede ser repetida y debería incluir mayúsculas, numeros, !@#$%^&\*, y una escama de dragón.
Facilito, no?

<!--more-->

![crazy](../../../images/crazy.gif)

## Es más práctico que tu solución

Fijo que tu manera de gestionar contraseñas es o bien incómoda o bien poco segura. O las tienes apuntadas en un archivo de texto que tienes que ir a buscar cada vez que haces login (o aún peor, escrito en papel como un **cavernícola**), o dejas que las gestione tu navegador por ti (suerte con cambiar de navegador o con evitar que la empresa fabricante las vea).

Un buen gestor de contraseñas, especialmente si tienen una extensión para navegador, son literalmente una _'one click solution'_ tanto para crear buenas contraseñas como para insertarlas en los campos de login.

## No puedes asegurar que no este repetida

Aún si crees que es nueva y molona, es muy probable que hayas usado la contraseña que acabas de _'inventar'_ para una cuenta ya olvidada. Repetir contraseñas es casi tan desastroso como usar 'admin' o 'password1'.

![repeat](../../../images/repeat.gif)

Hay una cosa llamada _dictionary attack_ y consiste en _espamear_ contraseñas filtradas o comunes contra la cuenta objetivo. La tuya no aguantaría ni un asalto.

## No puedes evitar usar un archivo de texto

Y fijo que ni lo encriptas. Todos manejamos una cantidad **enorme** de cuentas, no hay manera de que te acuerdes de todas esas contraseñas. Cualquiera con acceso a tu sistema puede acceder a un archivo no encriptado. Además, necesitas acceder a esa máquina en concreto para acceder a tus contraseñas.

## No puedes crear y recordar ni una sola contraseña decente

Mira las condiciones mínimas para la contraseña de una cuenta cualquiera y se sincero: De verdad puedes crear una buena sin emplear información personal como nombre o fecha de nacimiento? Ya, yo tampoco.

## Teclear contraseñas enormes de 14 dígitos es un coñazo

Y siempre te equivocas en algo.

## Perdiste el archivo? Perdiste las contraseñas?

Si no usas un servicio externo, tus contraseñas se **pierden para siempre** tan pronto como el archivo sea eliminado.
Si, usar el servicio de un tercero es menos seguro que tener un archivo local, encriptado, y seguro. Técnicamente depende de que tanto _confíes_ en la compañía detrás del servicio (y si, lo ideal sería que la confianza no entrara ni en la ecuación).

![no](../../../images/no.gif)

Aún así, probablemente sería una solución más segura (y definitivamente más simple).

## Vale, que uso entonces?

Pues... un gestor de contraseñas 😀. Un par de cosas a tener en cuenta y una sugerencia personal.

### Evita software de código cerrado a toda costa. Por lo siguiente:

1. Nadie sabe que hace el código exactamente. Estás **confiando** al 100% en la compañía que ofrezca el servicio.
2. El código abierto es siempre **más seguro**. Puede ser auditado públicamente.
3. Si la compañía decide hacerte pagar por un servicio que antes era gratuito no tienes elección, salvo **tal vez** exportar a un archivo JSON o CVS.
4. Si la compañía se va a tomar por saco y tú reaccionas demasiado lento, tus contraseñas se van con ella.

### Solo dos opciones razonables:

#### [BitWarden](https://bitwarden.com/)

La alternativa más amigable.
Es ampliamente usado y conocido, frecuentemente auditado por terceros, tienen un plan gratuito y uno pago para negocios y tienen básicamente todo lo que puedas necesitar:

-   Desktop GUI
-   Desktop CLI
-   Mobile GUI
-   Browser Plugin
-   Web Vault

La úlitima es importante.
Tienes la opción de hacerte una cuenta con ellos y guardar tus contraseñas en sus servidores (como con cualquier otro gestor de contraseñas) **o bien** puedes montar tu propia instancia en tu propio servidor (usando docker) para almacenar y gestionar tu bóveda y seguir teniendo acceso a todo su software.

Cojonudo, gratuito, respeta la privacidad y sin mariconadas.

#### [KeePassXC](https://keepassxc.org/)

Solución mínimalista (aunque no tanto como simplemente usar [pass](https://www.passwordstore.org/)). Es una implementación multiplataforma del estándar [KeePass](https://wiki.archlinux.org/index.php/KeePass) con soporte añadido para plugin de navegador.

Tienes una bóveda local encriptada que conectas al plugin y ya está.
**Tú** estás a cargo de los backups y la seguridad y podrás acceder a las contraseñas solo desde local, pero no hay literalmente más nadie involucrado. Ni siquiera una conexión con tu propio servidor.

## Conclusión

Personalmente uso ambos: BitWarden por comodidad (sobre todo en el teléfono) y KeePassXC para backups e información muy sensible.

![door](../../../images/door.gif)

No dejarías la puerta de tu casa abierta por la noche. Se coherente y actúa de la misma forma en línea.
