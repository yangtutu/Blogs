---
title: 操作系统及开发语言国内源配置
date: 2020-10-30
---

## Alpine

- 中科大

  ```bash
  sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories
  ```

- 阿里云

  ```bash
  sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
  ```

- 清华TUNA

  ```bash
  sed -i 's/dl-cdn.alpinelinux.org/mirror.tuna.tsinghua.edu.cn/g' /etc/apk/repositories
  ```

<!--more-->
## Ubuntu

- 阿里云

  ```bash
  sed -i 's|https\?://[^/]\+/|http://mirrors.aliyun.com/|' /etc/apt/sources.list
  ```

## CentOS 7

- 阿里云

  ```bash
  mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup  
  wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
  yum clean all
  yum makecache
  ```

## pypi

- 临时使用

  ```bash
  pip install -i {source} some-package
  ```

- 设为默认

  ```bash
  pip install pip -U
  pip config set global.index-url {source}
  ```

- 阿里云

  ```bash
  http://mirrors.aliyun.com/pypi/simple/
  ```

- 清华大学

  ```bash
  https://pypi.tuna.tsinghua.edu.cn/simple/
  ```