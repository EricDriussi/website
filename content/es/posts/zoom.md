---
url: ./zoom/
date: 2021-06-06T16:44:18+01:00
title: "Por qué evitar Zoom"
draft: true
image: /images/zoom.png
categories:
    - why
tags:
    - zoom
    - privacidad
    - seguridad
---

Como es que seguimos usando Zoom?

<!--more-->

## Zoom

Sigue siendo el software para videoconferencias [más popular](https://www.digitalinformationworld.com/2021/04/top-video-call-platform-by-market-share.html) del mundo. Casi seguro que ya lo has usado.
Ofrece una serie de herramientas muy útiles: control remoto, compartir archivos, correo de voz, grabaciones en la nube, anotaciones, pizarra, etc.
Es fácil de usar y para la mayoría de usuarios es super cómodo.

Eso está muy bien (aunque algunas de esas '_herramientas_' debería hacernos sospechar...) pero justifica esa comodidad el riesgo?

## Engaño

Hay un buen par de ocasiones en las que Zoom ha mentido a sus usuarios y al público general.
Vamos por partes, como Jack el Destripador.

### Seguridad

##### Cifrado, o no...

**Puede** que esté cifrado de extremo a extremo ahora, pero desde luego no lo estaba en marzo del 2020.
Toda tecnológica tiene sus problemas con la seguridad.
Sin embargo, no todas hacen [afirmaciones completamente falsas](https://theintercept.com/2020/03/31/zoom-meeting-encryption/) sobre su seguridad para vender más productos.

##### Usa mi Mac como servidor tuyo, no pasa nada...

Apple tuvo que hacerse cargo (sin llamar mucho la atención) de meter mano a millones de Macs cuando un [investigador de ciberseguridad descubrió](https://www.theregister.com/2019/07/09/zoom_mac_webcam_security_patch/) que Zoom instalaba un servidor web secreto en los Macs de sus usuarios, servidores que permanecían ahí aunque se desinstalara la aplicación.
Esto supuso que cualquier web malintencionada pudo activar las webcams (por ejemplo) de los Macs con Zoom instalado sin pedir permiso al usuario.

Ah, y por si no fuera suficiente: Zoom pidió al investigador que firmara un NDA.
Genial.

##### Puede que no te mole Facebook...

![facebook](../../../images/facebook.jpeg)

Pero sin duda tú si que le molas a el...
No tienes cuenta de Facebook? No pasa nada, Zoom estaba [enviando datos](https://www.vice.com/en/article/k7e599/zoom-ios-app-sends-data-to-facebook-even-if-you-dont-have-a-facebook-account) silenciosamente a Facebook sobre los hábitos de sus usuarios, aunque dicho usuario no tuviera una cuenta de Facebook.

##### Feature...?

Puede parecer bizarro, pero Zoom tenía una ['_feature_'](https://www.huffpost.com/entry/zoom-tracks-not-paying-attention-video-call_l_5e7b96b5c5b6b7d80959ea96) con la que el anfitrión de una llamada podía saber si los participantes estaban haciendo click fuera de la ventana principal de Zoom durante la conferencia.
Molaría que **no** te dedicaras a ver que hago en mi ordenador constantemente, gracias.

##### ZoomBombing

Sep, no hace mucho tenías que estar atento a que un troll cualquiera se colara en tu conferencia '_privada_' y [pusiera porno en streaming](https://techcrunch.com/2020/03/17/zoombombing/).
Hasta tiene cierta gracia.

##### **NUESTROS** datos

Recuerdas la falta de cifrado de antes? Técnicamente si que lo había... Sólo que entre sus clientes y sus servidores.
Así que...
Los de Zoom dicen [no haberlo hecho](https://www.consumerreports.org/privacy/zoom-tightens-privacy-policy-says-no-user-videos-analyzed-for-ads/) (gran sorpresa), pero tienen plena capacidad de recopilar información de usuario (incluyendo streams de video, audio, nombres, chat... Todo el lote) ya que se guardan las claves de cifrado para ellos.

![ours](../../../images/bugs-bunny.jpeg)

Pues mu bien!

##### **NUESTRO** I+D

[Aquí](https://citizenlab.ca/2020/04/move-fast-roll-your-own-crypto-a-quick-look-at-the-confidentiality-of-zoom-meetings/) tienes un breve resumen sobre porqué tener la mayor parte del desarrollo situado en China puede no ser lo más fidedigno del mundo, sobre todo si quieres que el público general piense que el PCC no te está presionando.

Claro que cuesta una fracción de lo que costaría el desarrollo en EEUU o en la UE.
Ahora bien, no esperes que me fie de tu empresa.

##### **NUESTRO** Tiananmen

![this-guy](../../../images/xi-jingpooh.png)

Sorpresa sorpresa, hablar mal de Winnie the Pooh te mete en líos.
Quién lo hubiera imaginado...
Eres de Hong-Kong y quieres conmemorar la [Massacre de Tiananmen de 1989](https://en.wikipedia.org/wiki/1989_Tiananmen_Square_protests) estando en suelo estadounidense?
![this-guy](../../../images/this-guy.png)

Que tal una pizca de [censura](https://www.wsj.com/articles/zoom-catches-heat-for-shutting-down-china-focused-rights-groups-account-11591863002?mod=lead_feature_below_a_pos1)?
Si hasta admitieron haberlo hecho... para ['_cumplir con la ley local_'](https://www.axios.com/zoom-closes-chinese-user-account-tiananmen-square-f218fed1-69af-4bdd-aac4-7eaf67f34084.html)...

### Confianza

Pues eso, confiar en esta gente es casi una broma a estas alturas.
Para ser justos, usar su servicio desde el navegador es significativamente más cuerdo y seguro.

A ver, no se puede decir que sea fácil crear una sala desde su página y la mayoría de las '_herramientas chulas_' no están.
Pero sinceramente, es justamente por eso que es más seguro. Esas herramientas no son razonables, no deberían estar ahí.

Y si, dicen que la mayoría de estos problemas están solucionados. Pero como siempre, nadie lo sabe.
El software de Código Cerrado requiere **confianza** en la empresa propietaria.
Abre el código fuente y la comunidad decidirá si es seguro o no.

## Alternativas

### Skype, Google Meet, Discord & Teams

Lo se, Microsoft y Google no son exactamente dignos de confianza. Pero preferiría que la compañía que me espía le deba explicaciones al gobierno de los EEUU que al PCC.
Además, probablemente estés usando Windows así que... De perdidos al río.
¯\\\_ (ツ)\_/¯

### [Zipcall](https://www.zipcall.com/)

Funciona desde el navegador.
Puedes crear video-conferencias con sus links auto generados o introducir tu propia passphrase.
Puedes mutear micro, pausar la webcam, compartir pantalla o usar el chat de texto desde la web app.

No muy lejos de Zoom desde el navegador.

### [Team.Video](https://team.video/)

Parecido al anterior pero con una UI trementamente pulida y moderna. Además, tiene un montón de [herramientas](https://www.notion.so/Team-Video-Gu-a-del-Usuario-27eb22665b454135b2d429702b1911c6) útiles que te pueden interesar.

### [GoTo Meeting](https://www.gotomeeting.com/)

![goto](../../../images/goto-meeting.png)

Han estado en el negocio desde siempre y probablemente tengan la mayor cantidad de herramientas para reuniones/conferencias de todas las alternativas.

### [Jitsi Meet](https://meet.jit.si/)

Espera, hay una alternativa FOSS a un programa injustificablemente popular? Quien lo hubiera dicho!

![goto](../../../images/jitsi.png)

Poco más o menos que todas las herramientas que puedas querer pero con los beneficios de ser FOSS.
No hay motivo para pensar que la gente que hay detrás estén recopilando tus datos pero, si quieres, puedes simplemente alojarlo en tu propio servidor.
Genial!
