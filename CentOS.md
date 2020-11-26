---
title: CentOS
date: 2020-07-30
---

## yum源

- aliyun

  ```bash
  mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup  
  wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
  yum clean all
  yum makecache
  ```
