---
title: Jenkins - 安装及配置
date: 2020-08-21
---

## CentOS 7 安装

- 安装

```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum upgrade -y 
sudo yum install jenkins java-1.8.0-openjdk-devel -y
```
- 配置端口

```bash
vi /etc/sysconfig/jenkins

# 监听端口
JENKINS_PORT="8080"
```

<!--more-->

- 配置权限

```bash
vi /etc/sysconfig/jenkins

# 修改用户为root用户
$JENKINS_USER="root"
```

- 修改目录权限

```bash
chown -R root:root /var/lib/jenkins
chown -R root:root /var/cache/jenkins
chown -R root:root /var/log/jenkins
```

- 启动

```bash
systemctl enable jenkins
systemctl start jenkins
```

- 首次启动密码

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

- 修改插件源

  `Manage Jenkins` -> `Manage Plugins` -> `Advanced`
  修改`Update Site`中的`URL`为`http://mirror.esuni.jp/jenkins/updates/update-center.json`
  原值为：`https://updates.jenkins.io/update-center.json`

- 添加环境变量PATH

  `Manage Jenkins` -> `Configure System`
  在`Global properties`下勾选`Environment variables`，添加以下内容：

  | Name | Value |
  | ------- | ------- |
  | PATH | /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/.dotnet/tools:/root/bin |


## Docker 安装

项目初期或者业务相对简单的话，也可以直接使用 Docker 安装的方式

``` shell
docker run --name jenkins \
  -u root \
  --privileged=true \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /var/jenkins_home:/var/jenkins_home \
  -p 8080:8080 -p 50000:50000 \
  jenkins:2.60.3
```
