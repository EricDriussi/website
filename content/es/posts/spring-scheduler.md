---
title: "Cómo usar Spring Scheduler"
url: ./spring-scheduler/
date: 2021-04-29T17:19:46+01:00
draft: true
image: /images/spring.png
categories:
    - howto
tags:
    - spring
    - scheduler
    - cron
---

Y de paso aprender como hacer Cron Jobs.

<!--more-->

## Habilitar el recurso

Hay muchas formas de manejar Triggers en Spring, pero desde luego la más fácil es usar el su propio Scheduler.

Para habilitarlo sólo habrá que anotar la clase que contenga el main con `@EnableScheduling`.

Vale la pena recalcar que el comportamiento por defecto no contempla la ejecución paralela de tareas programadas.
Si se trata de una aplicación que deberá manejar muchos Triggers valdrá la pena habilitar la ejecución asíncrona.
Para ésto habrá que anotar con `@EnableAsync` la misma clase mencionada arriba y usar `@Async` en el método deseado.

## Tipos de Scheduling

Spring nos ofrece fundamentalmente tres maneras de manejar Triggers:

### Fixed Rate

Lanza el método cada 'X' milisegundos.
Se habilita con la anotación `@Scheduled(fixedRate = tiempoEnMilisegundos)`.

```java
@Scheduled(fixedRate = 2000)
public void repiteCadaDosSegundos() {
	System.out.println("Me ejecuto cada dos segundos sin considerar la ejecución anterior!");
}
```

### Fixed Delay

Lanza el método 'X' milisegundos después de haber terminado la ejecución anterior.
Se habilita con la anotación `@Scheduled(fixedDelay = tiempoEnMilisegundos)`.

```java
@Scheduled(fixedDelay = 2000)
public void repiteALosDosSegundos() {
	System.out.println("Me ejecuto a los dos segundos de haber terminado la ejecución anterior!");
}
```

También se puede ajustar el retraso inicial de la ejecución del método añadiendo un `initialDelay`, quedando así: `@Scheduled(fixedDelay = 2000, initialDelay = 3000)`.

### Cron

Para mayor flexibilidad, Spring nos da la posibilidad de ajustar la repetición mediante Cron.
Se habilita con la anotación `@Scheduled(cron = "* * * * * *")`.

```java
@Scheduled(cron = "0 0 0 * * *")
public void repiteCadaMediaNoche() {
	System.out.println("Me ejecuto todos los días a media noche!");
}
```

## Unix-jobs vs Spring-jobs

Hay que puntualizar que los Cron de Spring no son exactamente iguales a los Cron tradicionales que verás en sistemas Unix.

#### Unix-Cron:

```code
┌───────────── minuto (0 - 59)
│ ┌───────────── hora (0 - 23)
│ │ ┌───────────── día del mes (1 - 31)
│ │ │ ┌───────────── mes (1 - 12)
│ │ │ │ ┌───────────── día de la semana (0 - 6) (Domingo a Sábado)
│ │ │ │ │
│ │ │ │ │
* * * * *
```

#### Spring-Cron:

```code
 ┌───────────── segundo (0-59)
 │ ┌───────────── minuto (0 - 59)
 │ │ ┌───────────── hora (0 - 23)
 │ │ │ ┌───────────── día del mes (1 - 31)
 │ │ │ │ ┌───────────── mes (1 - 12)
 │ │ │ │ │ ┌───────────── día de la semana (0 - 7) (Domingo a Domingo)
 │ │ │ │ │ │
 │ │ │ │ │ │
 * * * * * *
```

Así, donde el Cron de Unix sólo tiene 5 campos (en algunos sistemas hay 6 pero generalmente es para permisos de usuarios), el de Spring tiene 6; añadiendo la posibilidad de gestionar las tareas al segundo.

Además, mientras que el Cron tradicional soporta macros sólo en algunos sistemas, el de Spring lo hace por defecto:

| Macro      | Descripción         | Cron          |
| ---------- | ------------------- | ------------- |
| `@yearly`  | Una vez al año      | `0 0 0 1 1 *` |
| `@monthly` | Una vez al mes      | `0 0 0 1 * *` |
| `@weekly`  | Una vez a la semana | `0 0 0 * * 0` |
| `@daily`   | Una vez al día      | `0 0 0 * * *` |
| `@hourly`  | Una vez cada hora   | `0 0 * * * *` |
