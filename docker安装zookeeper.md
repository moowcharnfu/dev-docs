docker 安装参考

https://github.com/moowcharnfu/dev-docs/blob/master/docker%E5%AE%89%E8%A3%85.md

1.查找zookeeperq镜像

docker search zookeeper

2.拉取zookeeper镜像库

docker pull zookeeper

查看本地镜像

docker image ls

docker inspect {zookeeperId如7eb4d30d3408}

3.这里是集群模式, 分别创建挂载目录

zk1:

mkdir -p /data/zk1/data

mkdir -p /data/zk1/conf

/data/zk1/conf/zoo.cfg
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=/data/zk1/data
# the port at which the clients will connect
clientPort=2181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1
# 保留两天日志20个快照文件
autopurge.snapRetainCount=20

autopurge.purgeInterval=48

server.1=192.168.4.55:3881:3884

server.2=192.168.4.109:3882:3885

server.3=192.168.4.110:3893:3896

/data/zk1/data/myid

echo 1 > /data/zk1/data/myid

zk2:

mkdir -p /data/zk2/data

mkdir -p /data/zk2/conf

/data/zk2/conf/zoo.cfg
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=/data/zk2/data
# the port at which the clients will connect
clientPort=2181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1
# 保留两天日志20个快照文件
autopurge.snapRetainCount=20

autopurge.purgeInterval=48

server.1=192.168.4.55:3881:3884

server.2=192.168.4.109:3882:3885

server.3=192.168.4.110:3893:3896

/data/zk2/data/myid

echo 2 > /data/zk2/data/myid

zk3:

mkdir -p /data/zk3/data

mkdir -p /data/zk3/conf

/data/zk3/conf/zoo.cfg
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=/data/zk3/data
# the port at which the clients will connect
clientPort=2181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1
# 保留两天日志20个快照文件
autopurge.snapRetainCount=20

autopurge.purgeInterval=48

server.1=192.168.4.55:3881:3884

server.2=192.168.4.109:3882:3885

server.3=192.168.4.110:3893:3896

/data/zk3/data/myid

echo 3 > /data/zk3/data/myid


4.运行zookeeper

-d 后台运行
-p 将容器内部端口向外映射(宿主机端口:容器端口)
--name 命名容器名称
--restart always 设置自动重启

docker run -d --restart always --name zookeeper-3181 -p 3181:2181 -v /data/zk1/data:/data -v /data/zk1/conf:/conf zookeeper

docker run -d --restart always --name zookeeper-3181 -p 3181:2181 -v /data/zk2/data:/data -v /data/zk2/conf:/conf zookeeper

docker run -d --restart always --name zookeeper-3181 -p 3181:2181 -v /data/zk3/data:/data -v /data/zk3/conf:/conf zookeeper

5.查看日志 docker logs -f zookeeper-3181
