---
title: RabbitMQ Installation (Linux & Windows)
date: 2020-06-06
---

## Installation

- CentOS

  ```bash
  yum install rabbitmq-server -y
  
  systemctl enable rabbitmq-server
  systemctl start rabbitmq-server
  
  # Enable rabbitmq_management
  rabbitmq-plugins enable rabbitmq_management
  systemctl restart rabbitmq-server
  
  # Add admin user
  rabbitmqctl add_user {user} {password} # Use your own user and password
  rabbitmqctl set_user_tags {user} administrator
  rabbitmqctl set_permissions -p '{vhost}' {user} '.*' '.*' '.*' # Use your vhost or default '/'
  
  rabbitmqctl delete_user guest
  ```

- Windows

<!--more-->

  ```shell
  # Install Elang
  http://www.erlang.org/downloads
  
  # Install RabbitMQ
  http://www.rabbitmq.com/download.html
  
  cd rabbitmq_server-x.x.xx\sbin
  
  # Set Storage Directory
  rabbitmq-service.bat remove
  set RABBITMQ_BASE={directory} # Use your own directory
  rabbitmq-service.bat install
  
  # Start RabbitMQ
  net start RabbitMQ
  
  # Enable rabbitmq_management
  rabbitmqctl stop_app
  rabbitmqctl reset
  rabbitmq-plugins enable rabbitmq_management
  rabbitmqctl start_app
  
  # Add admin user
  rabbitmqctl add_user {user} {password} # Use your own user and password
  rabbitmqctl set_user_tags {user} administrator
  rabbitmqctl set_permissions  -p  "{vhost}" {user} ".*" ".*" ".*" # Use your vhost or default '/'
  ```
