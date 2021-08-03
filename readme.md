## 简介
使用 docker-compose 快速搭建一个伪 zk 集群，内部由3台zk服务组成。

不建议在生产环境进行使用，不然 zk 失去了分布式容灾的作用

## 使用
```shell
git clone https://github.com/ddzyan/docker-compose_zk.git

cd docker-compose_zk

mkdir zk1 zk2 zk3

# 启动
docker-compose up -d

# 观察容器状态
docker-compose ps
Name              Command               State                                   Ports
----------------------------------------------------------------------------------------------------------------------
zoo1   /docker-entrypoint.sh zkSe ...   Up      0.0.0.0:2181->2181/tcp,:::2181->2181/tcp, 2888/tcp, 3888/tcp, 8080/tcp
zoo2   /docker-entrypoint.sh zkSe ...   Up      0.0.0.0:2182->2181/tcp,:::2182->2181/tcp, 2888/tcp, 3888/tcp, 8080/tcp
zoo3   /docker-entrypoint.sh zkSe ...   Up      0.0.0.0:2183->2181/tcp,:::2183->2181/tcp, 2888/tcp, 3888/tcp, 8080/tcp

# 进入一个容器
docker-compose  exec  zk1 sh

```

访问 zk 实例内嵌的 web 控制台：[http://localhost:8081/commands](http://localhost:8081/commands/stats)

### 备注

重要配置

1. ZOO_MY_ID 表示当前zookeeper实例在zookeeper集群中的编号，范围为1-255，所以一个zk集群最多有255个节点，会影响到选举结果
2. ZOO_SERVERS 表示当前zookeeper实例所在zookeeper集群中的所有节点的编号、端口、主机名（或IP地址）配置

开放端口

1. 2181：对client端提供服务的端口
2. 3888：选举leader使用
3. 2888：集群内机器通讯使用（Leader监听此端口）