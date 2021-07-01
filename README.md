## network

```bash
# 创建网络
docker network create --driver bridge --subnet 172.30.0.0/24 --gateway 172.30.0.1 testnet 
# 查看网络
docker network ls
# 查看具体的网络信息
docker network inspect testnet
```


## volume

```bash
# 查看volume
docker volume ls

docker volume create volume_name


# 查看volume信息
docker volume inspect volume_name

# 查看容器挂载信息
docker inspect container_name | grep Mounts -A 20

# 删除无用的volume
docker volume rm $(docker volume ls -qf dangling=true)
```

在 window 环境下，`/var/lib/docker/` 映射到 `\\wsl$\docker-desktop-data\version-pack-data\community\docker\` 目录下。所有的image、container、volume都存放在该目录下。

## compose

```bash
# 启动
docker-compose -f .\docker-compose-zookeeper.yaml up
# 停止
docker-compose -f docker-compose.yml stop
# 停止并删除服务

```

## container

```bash
# 进入容器
docker exec -it name /bin/bash

# 删除所有已经停止的容器
docker rm $(docker ps -a|grep Exited |awk '{print $1}')docker rm $(docker ps -qf status=exited)
# 删除所有未打标签的镜像
docker rmi $(docker images -q -f dangling=true)
# 删除所有容器、网络、数据卷
docker system prune
```


## log

```bash
docker logs --tail=100 zoo1
```


## Portainer

```bash
# server
docker volume create portainer_data

# 本地服务模式
docker run -d -p 9000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data --name prtainer portainer/portainer

```


## zookeeper + kafka

```bash
# 查看zookeeper版本
docker exec zoo1 pwd

# 查看kafka版本
docker exec kafka1 find / -name \*kafka_\* | head -1 | grep -o '\kafka[^\n]*'

# 进入kafka
docker exec -it kafka1 /bin/bash

# 创建topic
./bin/kafka-topics.sh --create --zookeeper zoo1:2181,zoo2:2181,zoo3:2181 --partitions 3 --replication-factor 1 --topic test

# 查看topic
./bin/kafka-topics.sh --list --zookeeper zoo1:2181,zoo2:2181,zoo3:2181

./bin/kafka-topics.sh --describe --topic test --zookeeper zoo1:2181,zoo2:2181,zoo3:2181

# 消费topic
./bin/kafka-console-consumer.sh --bootstrap-server kafka1:9092,kafka2:9093,kafka3:9094 --topic test --from-beginning

```



## Kafka

```kafka
advertised_host_name: 注册到zookeeper的broker名称。已弃用。
listeners: 监听器，告诉连接者通过什么协议访问指定主机名和端口开放的 Kafka 服务，不写IP或域名代表没有限制。
advertised_listeners: 注册到zookeeper的监听信息
```







[Docker 进阶（九）案例：Docker Swarm 搭建 zookeeper + kafka 集群_芳哥_143671的博客-CSDN博客](https://blog.csdn.net/qq_41143671/article/details/109513013)

[docker-可视化容器管理工具Portainer – 运维派 (yunweipai.com)](http://www.yunweipai.com/34991.html)

[二 用docker compose搭建kafka集群 - 简书 (jianshu.com)](https://www.jianshu.com/p/b811ea29428c)