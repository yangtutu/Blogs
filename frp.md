# frp

## 简介
frp是一个可用于内网穿透的高性能的反向代理应用。借助中间服务代理转发，可以实现各种内网穿透功能。

## 基本原理

`客户端A` -> `服务端` <- `客户端B`，`服务端`需要有公网IP，通过`服务端`作为桥梁，实现`客户端A`与`客户端B`之间的通讯。

## 服务端安装

### CentOS

- 安装

  ```bash
  # 下载 frp，请选择对应的版本，这里使用0.31.1
  curl https://github.com/fatedier/frp/releases/download/v0.31.1/frp_0.31.1_linux_amd64.tar.gz

  # 解压
  tar -zxvf frp_0.31.1_linux_amd64.tar.gz
  ```

- 配置

  ```bash
  vi frps.ini
  ```

- frps.ini

  ```bash
  [common]
  # 要绑定的端口，默认7000
  bind_port = 7000
  
  # http映射端口，默认80
  vhost_http_port = 80
  
  # https映射端口，默认443
  vhost_https_port = 443
  
  # 安全授权 token，防止端口被扫描到后可以被任意客户端连接
  token = {your token} 
   
  # 控制台的用户名
  dashboard_user = {your username}
  
  # 控制台的密码
  dashboard_pwd = {your password}
  
  # 控制台的端口，默认7750
  dashboard_port = 7750
  ```
- 启动

  ```bash
  frps -c frps.ini
  ```

- 设置开机启动

  - 创建后台启动服务文件

    ```bash
    vi /etc/systemd/system/frp.service
    ```

  - frp.service
  
    ```bash
    [Unit]
    Description=frps
    After=network.target
     
    [Service]
    ExecStart=/root/frp_0.31.1_linux_amd64/frps -c /root/frp_0.31.1_linux_amd64/frps.ini
     
    [Install]
    WantedBy=multi-user.target
    ```
  - 启动
  
    ```bash
    # 设置开启启动
    systemctl enable frp.service
  
    # 启动
    systemctl start frp.service
    ```
    
  - 查看状态
  
    ```bash
    systemctl status frp.service
    ```

## Windows远程

## Linux 远程

