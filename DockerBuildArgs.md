---
title: 使用参数构建Docker镜像
date: 2020-12-02
---

&emsp;&emsp;本文主要介绍如何通过docker build args来实现同一个Dockerfile打出不同环境的docker镜像

- Dockerfile

  ```dockerfile
  FROM alpine

  ARG COUNTRY

  RUN if [ "$COUNTRY" = "CN" ] ; then sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories ; fi

  ARG ENV

  ENV MY_EMV $ENV
  ```

<!--more-->

- 测试环境

  ```bash
  docker build -t alpine:test --build-arg ENV=testing .
  docker run -dit --name alpine-test alpine:test sh
  docker exec -it alpine-test sh -c export | grep MY_EMV

  # 输出： 
  # export MY_EMV='testing'
  ```

- 正式环境

  ```bash
  docker build -t alpine:prod --build-arg ENV=prod --build-arg COUNTRY=CN .
  docker run -dit --name alpine-prod alpine:prod sh
  docker exec -it alpine-prod sh -c export | grep MY_EMV

  #输出：
  # export MY_EMV='prod'

  docker exec -it alpine-prod sh -c "cat /etc/apk/repositories"

  # 输出：
  # http://mirrors.ustc.edu.cn/alpine/v3.12/main
  # http://mirrors.ustc.edu.cn/alpine/v3.12/community
  ```