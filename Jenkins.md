# Jenkins

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

点击`Manage Jenkins` -> 点击`Manage Plugins` -> 点击`Advanced` -> 把`Update Site`中的`URL`修改为`http://mirror.esuni.jp/jenkins/updates/update-center.json`

原值为：`https://updates.jenkins.io/update-center.json`
