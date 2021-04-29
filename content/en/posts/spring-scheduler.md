---
url: /en/spring-scheduler/
date: 2021-04-29T17:19:52+01:00
title: "How to use Spring Scheduler"
draft: true
image: /images/spring.png
categories:
    - howto
tags:
    - spring
    - scheduler
    - cron
---

And while we're at it, learn how to make Cron Jobs.

<!--more-->

## Enable the resource

There are many ways to manage Triggers in Spring, but by far the easiest one is using the built-in Scheduler.

To enable it you only have to annotate the main class with `@EnableScheduling`.

It's worth pointing out that the default behavior doesn't allow for parallel execution of tasks.
If you are working with an app that will be executing multiple Triggers, it might be worth it to enable this function.
To do this you'll annotate with `@EnableAsync` the same class mentioned above, and with `@Async` the desired method.

## Types of Scheduling

Spring offers us basically three ways of managing recurrent jobs:

### Fixed Rate

Runs the method every 'X' milliseconds.
Enable it with `@Scheduled(fixedRate = timeInMilliseconds)`.

```java
@Scheduled(fixedRate = 2000)
public void repeatEveryTwoSeconds() {
	System.out.println("I run every two seconds, no matter the previous run!");
}
```

### Fixed Delay

Runs the method 'X' milliseconds after the previous execution is done.
Enable it with `@Scheduled(fixedDelay = timeInMilliseconds)`.

```java
@Scheduled(fixedRate = 2000)
public void repeatAfterTwoSeconds() {
	System.out.println("I run two seconds after the previous run is over!");
}
```

You can also adjust the initial execution delay adding `initialDelay`, as such: `@Scheduled(fixedDelay = 2000, initialDelay = 3000)`.

### Cron

For greater flexibility, Spring allows us to adjust the repetition pattern with Cron.
Enable it with `@Scheduled(cron = "* * * * * *")`.

```java
@Scheduled(cron = "0 0 0 * * *")
public void repeatEveryMidnight() {
	System.out.println("I run every day at midnight");
}
```

## Unix-jobs vs Spring-jobs

Keep in mind that the Spring version of Cron is not exactly identical to traditional Cron, like the one you'll find in Unix based systems.

#### Unix-Cron:

```code
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of month(1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of week (0 - 6) (Sunday to Saturday)
│ │ │ │ │
│ │ │ │ │
* * * * *
```

#### Spring-Cron:

```code
 ┌───────────── second (0-59)
 │ ┌───────────── minute (0 - 59)
 │ │ ┌───────────── hour (0 - 23)
 │ │ │ ┌───────────── day of month (1 - 31)
 │ │ │ │ ┌───────────── month (1 - 12)
 │ │ │ │ │ ┌───────────── day of week (0 - 7) (Saturday to Saturday)
 │ │ │ │ │ │
 │ │ │ │ │ │
 * * * * * *
```

As you can see, where Unix-like-Cron has only 5 fields (some systems have 6 but that's used for user permissions), Spring-like-Cron has 6; adding the hability do manage tasks my the second.

Moreover, while traditional Cron only supports macros in some systems, Springs version does so by default:

| Macro      | Description    | Cron          |
| ---------- | -------------- | ------------- |
| `@yearly`  | Once a year    | `0 0 0 1 1 *` |
| `@monthly` | Once a month   | `0 0 0 1 * *` |
| `@weekly`  | Once a week    | `0 0 0 * * 0` |
| `@daily`   | Once a day     | `0 0 0 * * *` |
| `@hourly`  | Once evey hour | `0 0 * * * *` |
