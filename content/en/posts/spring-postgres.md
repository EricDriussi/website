---
title: "How to connect Spring and Postgre"
date: 2021-04-22T19:31:41+01:00
url: ./spring-postgre/
draft: true
image: /images/pgadmin-logo.png
categories:
    - howto
tags:
    - spring
    - postgres
    - database
---

And inspect the DB using pgAdmin.

<!--more-->

### Given

-   A basic understanding of how [**Spring**](https://spring.io/) and [**Postgre**](https://www.postgresql.org/) work.
-   A basic understanding of **SQL** systems work.

## Spring

Spring projects are usually configured through a `application.properties` file.
It uses a very simple _key=value_ format.

Specifically the attributes we are interested in are under `spring.datasource.*`.
Let's suppose a local project:

```
spring.datasource.url=jdbc:postgreql://localhost:{PORT}/{PROJECT_NAME}
spring.datasource.username={USERNAME}
spring.datasource.password={PWD}
spring.datasource.driver-class-name=org.postgreql.Driver
```

If the DB is hosted elsewhere, you'll have to substitute the first value for your given connection URL.
By default, Postgre will use port 5432.

With that, we should have a working connection. But, especially in [Liquidbase](https://www.liquibase.com/) type environments (which can create on the fly DB), it's very useful to be able to explicitly inspect the data.

## Postgre / pgAdmin

We can spin up a Docker server in a pinch with the following `docker-compose.yml` or just use a pre-existing DB.

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

<sub><sub>As long as Docker is correctly installed, has it's service running and docker-compose is up.</sub></sub>

There are many ways to inspect a DB. In the case of Postgre, at least in Linux, the easiest way is to just use [pgAdmin](https://www.pgadmin.org/). The example uses `pacman` but you'll surely find it in any old official repository.

```sh
sudo pacman -S pgadmin4
```

After executing, the app should open in a web browser. Basically it's just another service connected to the DB, it just eases developing and testing.

You'll see a page similar to this one, right click on Servers `Create > Server...`

![pgadmin1](../../../images/pgadmin1.png)

Use the same data with which you connected the Spring application (or the ones from docker-compose, whichever data you have regarding the DB) to establish a connection.

![pgadmin2](../../../images/pgadmin2.png)

The DB content tree should unfold (if all went well) on the left panel. Follow the screen-shot to find the good stuff.

![pgadmin3](../../../images/pgadmin3.png)

From here on out it's all about exploring pgAdmin's tools. You can inspect the tables, launch queries and a whole lot more.
