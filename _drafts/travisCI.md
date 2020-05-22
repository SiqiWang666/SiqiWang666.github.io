---
title: "Setup CI/CD Workflow"
categories:
  - Blog
tags:
  - tutorial
---

This blog is a recap about how I setup the CI/CD workflow with Docker step by step using Travis CI for reliable UDP project.

# Clarifying Requirements

A few things I want Travis CI to perform after each time I push to branch master.

1. Tell Travis how to run test suites
1. Build image using Dockerfile
1. Push image to Docker hub
1. Get notification from email

# Configure `.travis.yml` file
Because we are using Docker, we need super user permission.
```yaml
sudo: required
```

Next thing we need do is tell Travis CI we need Docker CLI pre-installed.
```yaml
services:
  - docker
```

## How to run test?
`docker-compose.yml` is used to start sender and receiver services at the same time in the *development stage*. Building sender service is a little tricky.

**Sender Service:** Makefile is used to automate build and test. The build process involves two steps: installing `make` and Java `jdk:11`.

*Step One:* I first try to use `gcc` as the base image to bring `make` right to hand. The `gcc` image is up to 409.15MB and it costs up to 8s to complete the pulling down stage.

```bash
Step 1/7 : FROM gcc as base
latest: Pulling from library/gcc
376057ac6fa1: Pull complete
5a63a0a859d8: Pull complete
496548a8c952: Pull complete
2adae3950d4d: Pull complete
0ed5a9824906: Pull complete
7e3206aff56a: Pull complete
7f1979c30d35: Pull complete
23391fd76f15: Pull complete
3941ca8ed448: Pull complete
Digest: sha256:f5875b35477b328558faa7656e3d3164214e3deae8ed50bcd70119adab4d8b87
Status: Downloaded newer image for gcc:latest
 ---> 828cd42f8a79
```

Nope! I am not gonna do that.

Inspired by issue [#](https://github.com/docker-library/golang/issues/136), I use `alpine` as base image and add things I need.

```bash
FROM alpine as build-env
RUN apk add --update gcc \
&& apk add --update make
```

*Step Two* Next step we need to bring `jdk:11` and copy over all the source code.