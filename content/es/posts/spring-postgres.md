---
title: "Como conectar Spring y Postgre"
date: 2021-04-22T19:31:41+01:00
url: ./spring-postgre/
draft: true
image: /images/pgadmin-logo.png
categories:
    - howto
tags:
    - spring
    - postgre
    - base de datos
---

E inspeccionar la BD desde pgAdmin.

<!--more-->

### Supongo

-   Un conocimiento básico de cómo funcionan [**Spring**](https://spring.io/) y [**Postgre**](https://www.postgresql.org/).
-   Un conocimiento básico de cómo trabajar con sistemas **SQL**.

## Spring

Los proyectos de Spring se configuran por norma con un archivo `application.properties`.
Se usa un formato clave valor de lo más simple: _key=value_.

Concretamente los atributos que nos interesan están bajo `spring.datasource.*`.
Supongamos que se trata de un proyecto en local:

```
spring.datasource.url=jdbc:postgreql://localhost:{PORT}/{PROJECT_NAME}
spring.datasource.username={USERNAME}
spring.datasource.password={PWD}
spring.datasource.driver-class-name=org.postgreql.Driver
```

Si la BD se encontrara en remoto, habría que sustituir el primer valor por la URL de conexión.
Por defecto, el puerto de Postgre será 5432.

Con ésto ya tendríamos conexión. Pero, sobre todo en entornos tipo [Liquidbase](https://www.liquibase.com/) (que pueden crear su BD sobre la marcha), resulta muy útil consultar los dato de manera explícita.

## Postgre / pgAdmin

Podemos levantar un servidor Docker en un momento con el siguiente `docker-compose.yml` o bien usar una BD pre-existente.

```yaml
services:
    test_database:
        image: postgre
        ports:
            - "5432:5432"
        restart: on-failure
        environment:
            postgre_DB: test
            postgre_USER: { USERNAME }
            postgre_PASSWORD: { PWD }
        volumes:
            - postgre-data:/var/lib/postgreql/data
        networks:
            - internal
volumes:
    postgre-data:

networks:
    internal:
```

<sub><sub>Simpre que Docker esté debidamente instalado, el servicio arrancado y docker-compose levantado.</sub></sub>

Existen varias formas de inspeccionar una BD. En el caso de Postgre, al menos en Linux, lo más sencillo es usar [pgAdmin](https://www.pgadmin.org/). El ejemplo usa `pacman` pero seguramente podrás encontrarlo en cualquier repositorio oficial.

```sh
sudo pacman -S pgadmin4
```

Al ejecutarse, la aplicación debería abrirse en una pestaña del navegador. A efectos prácticos no deja de ser simplemente otro servicio en local accediendo a la BD, pero facilita mucho el trabajo.

Verás una página como ésta, click derecho en Servers `Create > Server...`

![pgadmin1](../../../images/pgadmin1.png)

Usa los mismos datos con los que conectaste la aplicación Spring (o los del docker-compose, los que tengas de la base de datos) para realizar la conexión.

![pgadmin2](../../../images/pgadmin2.png)

En el panel de la izquierda se desplegará (si todo fue bien) el árbol de contenidos de la BD con la que hemos conectado. Sigue la captura para encontrar la chicha...

![pgadmin3](../../../images/pgadmin3.png)

De aquí en adelante todo es explorar las herramientas de la aplicación. Puedes inspeccionar las tablas, lanzar queries en SQL y mil cosas más.
