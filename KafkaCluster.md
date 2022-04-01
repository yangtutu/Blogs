---
title: 安装 kafka 集群
date: 2022-04-01
description: 安装 kafka 集群
---

## 操作系统

CentOS 7+


## 安装步骤

准备多台服务器，数量建议为奇数，例如：3，5，7 等。

假设有 3 台服务器，IP 分别为 192.168.1.1、192.168.1.2、192.168.1.3，修改下述脚本文件，并拷贝到 3 台服务器分别执行。

setup.sh

``` bash
#!/bin/bash

# Modify the link if you want to download other version
KAFKA_DOWNLOAD_URL="https://dlcdn.apache.org/kafka/3.1.0/kafka_2.13-3.1.0.tgz"

# Please use your own server ip
SERVERS=("192.168.1.1" "192.168.1.2" "192.168.1.3")


ID=0

MECHINE_IP=$(hostname -i)
echo "Mechine IP: "${MECHINE_IP}

LENGTH=${#SERVERS[@]}

for (( i=0; i<${LENGTH}; i++ ));
do
    if [ "${SERVERS[$i]}" = "${MECHINE_IP}" ]; then
        ID=$((i+1))
    fi
done

echo "ID: "${ID}

if [ "${ID}" -eq "0" ]; then
  echo "Mechine IP is not matched to server list"
  exit 1
fi

ZOOKEEPER_CONNECT=$(printf ",%s:2181" "${SERVERS[@]}")
ZOOKEEPER_CONNECT=${ZOOKEEPER_CONNECT:1}
echo "Zookeeper Connect: "${ZOOKEEPER_CONNECT}


echo "---------- Update yum ----------"
yum update -y
yum install -y wget


echo "---------- Install java ----------"
yum -y install java-1.8.0-openjdk
java -version


echo "---------- Create kafka user & group ----------"
groupadd -r kafka
useradd -g kafka -r kafka -s /bin/false


echo "---------- Download kafka ----------"
cd /opt
wget ${KAFKA_DOWNLOAD_URL} -O kafka.tgz
mkdir -p kafka
tar -xzf kafka.tgz -C kafka --strip-components=1
chown -R kafka:kafka /opt/kafka


echo "---------- Install and start zookeeper ----------"
mkdir -p /data/zookeeper
chown -R kafka:kafka /data/zookeeper
echo "${ID}" > /data/zookeeper/myid


# zookeeper config
# https://zookeeper.apache.org/doc/r3.1.2/zookeeperAdmin.html#sc_configuration
cat <<EOF > /opt/kafka/config/zookeeper-cluster.properties
# the directory where the snapshot is stored.
dataDir=/data/zookeeper

# the port at which the clients will connect
clientPort=2181

# setting number of connections to unlimited
maxClientCnxns=0

# keeps a heartbeat of zookeeper in milliseconds
tickTime=2000

# time for initial synchronization
initLimit=10

# how many ticks can pass before timeout
syncLimit=5

# define servers ip and internal ports to zookeeper
EOF

for (( i=0; i<${LENGTH}; i++ ));
do
    INDEX=$((i+1))
    echo "server.${INDEX}=${SERVERS[$i]}:2888:3888" >> /opt/kafka/config/zookeeper-cluster.properties
done


# zookeeper.service
cat <<EOF > /usr/lib/systemd/system/zookeeper.service
[Unit]
Description=Apache Zookeeper server (Kafka)
Documentation=http://zookeeper.apache.org
Requires=network.target remote-fs.target
After=network.target remote-fs.target

[Service]
Type=simple
User=kafka
Group=kafka
ExecStart=/opt/kafka/bin/zookeeper-server-start.sh /opt/kafka/config/zookeeper-cluster.properties
ExecStop=/opt/kafka/bin/zookeeper-server-stop.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl start zookeeper && systemctl enable zookeeper


echo "---------- Install and start kafka ----------"
mkdir -p /data/kafka
chown -R kafka:kafka /data/kafka


# kafka config
# https://kafka.apache.org/documentation/#configuration
cat <<EOF > /opt/kafka/config/server-cluster.properties
# The id of the broker. This must be set to a unique integer for each broker.
broker.id=${ID}

# Hostname and port the broker will advertise to producers and consumers. If not set,
# it uses the value for "listeners" if configured.  Otherwise, it will use the value
# returned from java.net.InetAddress.getCanonicalHostName().
advertised.listeners=PLAINTEXT://${MECHINE_IP}:9092

# A comma separated list of directories under which to store log files
log.dirs=/data/kafka

# The default number of log partitions per topic. More partitions allow greater
# parallelism for consumption, but this will also result in more files across
# the brokers.
num.partitions=1

# The replication factor for the group metadata internal topics "__consumer_offsets" and "__transaction_state"
# For anything other than development testing, a value greater than 1 is recommended to ensure availability such as 3.
offsets.topic.replication.factor=1
transaction.state.log.replication.factor=1
transaction.state.log.min.isr=1

# The minimum age of a log file to be eligible for deletion due to age
log.retention.hours=168

# The maximum size of a log segment file. When this size is reached a new log segment will be created.
log.segment.bytes=1073741824

# The interval at which log segments are checked to see if they can be deleted according
# to the retention policies
log.retention.check.interval.ms=300000

# Zookeeper connection string (see zookeeper docs for details).
# This is a comma separated host:port pairs, each corresponding to a zk
# server. e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002".
# You can also append an optional chroot string to the urls to specify the
# root directory for all kafka znodes.
zookeeper.connect=${ZOOKEEPER_CONNECT}/kafka

# Timeout in ms for connecting to zookeeper
zookeeper.connection.timeout.ms=60000
EOF


# kafka.service
cat <<EOF > /usr/lib/systemd/system/kafka.service
[Unit]
Description=Apache Kafka server (broker)
Documentation=http://kafka.apache.org/documentation.html
Requires=network.target remote-fs.target
After=network.target remote-fs.target kafka-zookeeper.service
 
[Service]
Type=simple
User=kafka
Group=kafka
ExecStart=/opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server-cluster.properties
ExecStop=/opt/kafka/bin/kafka-server-stop.sh
Restart=on-failure
 
[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl start kafka && systemctl enable kafka
``` 

## 基本操作

``` bash
# 启动 zookeeper
systemctl start zookeeper

# 停止 zookeeper
systemctl stop zookeeper

# 重启 zookeeper
systemctl restart zookeeper

# 查看 zookeeper 日志
systemctl status zookeeper -l

# 启动 kafka
systemctl start kafka

# 停止 kafka
systemctl stop kafka

# 重启 kafka
systemctl restart kafka

# 查看 kafka 日志
systemctl status kafka -l
``` 

## 简单测试

``` bash
# 进入 kafka bin 目录
cd /opt/kafka/bin/

# 创建一个 topic
kafka-topics.sh --create --topic test --partitions 3 --replication-factor 1 --bootstrap-server localhost:9092

# 查看 topic 描述
kafka-topics.sh --topic test --describe --bootstrap-server localhost:9092

# 启动生产者然后输入消息
kafka-console-producer.sh --topic test --bootstrap-server localhost:9092

# 启动消费者消费消息
kafka-console-consumer.sh --topic test --from-beginning --bootstrap-server localhost:9092

# 删除 topic
kafka-topics.sh --topic test --delete --bootstrap-server localhost:9092
``` 

## 参考

- [Kafka 3.1 安装-基于CentOS 7.9](https://www.skynemo.cn/archives/base-on-centos79-install-kafka31)

- [Installing a Kafka Cluster and Creating a Topic](https://acloudguru.com/hands-on-labs/installing-a-kafka-cluster-and-creating-a-topic)

- [ZooKeeper Administrator's Guide](https://zookeeper.apache.org/doc/r3.1.2/zookeeperAdmin.html)

- [Apache Kafka](https://kafka.apache.org/quickstart)

- [Installing Multi-node Kafka Cluster](https://www.learningjournal.guru/article/kafka/installing-multi-node-kafka-cluster/)

- [Setting up Kafka cluster using Docker Swarm](https://medium.com/@NegiPrateek/wtf-setting-up-kafka-cluster-using-docker-swarm-6429bdb5784b)

- [Setting up Kafka Cluster using Docker Stack](https://medium.com/@NegiPrateek/wtf-setting-up-kafka-cluster-using-docker-stack-5efc68841c23)
