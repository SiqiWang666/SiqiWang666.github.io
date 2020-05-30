---
title: "Initialize a Spring Boot App"
date: 2020-04-30
read_time: false
categories:
  - Blog
tags:
  - spring
---

How to create a spring application by using [spring initializr][spring initializr]?

Spring initializr makes things easier.

1. Pick the build management tool and language.
2. Choose the Spring Boot version. I would recommend you to pick a stable one.
3. Give your group and artifact a special name.
4. Pick how would you like the way packaging it.
5. Choose a version of Java.

Then the next thing I would do is to add dependencies. You can feel free to add dependencies on the way.

<figure class="half">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/spring/spring-initializr.png">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/spring/spring-initializr-dependancy.png">
    <figcaption>screen snapshot</figcaption>
</figure>

Click **GENERATE**! Unzip it under your working directory!

[spring initializr]:(https://start.spring.io/)