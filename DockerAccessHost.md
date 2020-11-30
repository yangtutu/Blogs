---
title: 在Docker容器中操作宿主机Host
date: 2020-11-27
---

&emsp;&emsp;本文将通过远程执行SSH指令的方式实现在Docker容器中操作宿主机Host。

- 在宿主机上生成SSH Key

  ```bash
  ssh-keygen -t rsa -b 4096
  ```

- 把Public Key加入到authorized_keys

  ```bash
  cat /root/.ssh/id_rsa.pub > ~/.ssh/authorized_keys
  ```

- 启动容器，并把Private Key挂载到容器中

  ```bash
  docker run --name alpine -dit -v /root/.ssh/id_rsa:/root/.ssh/id_rsa alpine sh
  ```

<!--more-->

- 进入容器

  ```bash
  docker exec -it alpine sh
  ```

- 安装SSH

  ```bash
  apk add openssh
  ```

- 操作宿主机

  ```bash
  # 获取宿主机名称, 根据需要替换{user}, {nodeIp} 和 {command}
  ssh -q -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o PasswordAuthentication=no -i /root/.ssh/id_rsa {user}@{nodeIp} "{command}"
  ```