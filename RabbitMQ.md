# RabbitMQ

## 安装

- CentOS

  ```bash
  yum install rabbitmq-server -y
  
  systemctl enable rabbitmq-server
  systemctl start rabbitmq-server
  
  rabbitmq-plugins enable rabbitmq_management
  systemctl restart rabbitmq-server
  
  rabbitmqctl add_user {user} {password} # Use your own user and password
  rabbitmqctl set_user_tags {user} administrator
  rabbitmqctl set_permissions -p '{vhost}' {user} '.*' '.*' '.*' # Use your vhost or default '/'
  
  rabbitmqctl delete_user guest
  ```
