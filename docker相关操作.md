<pre>
查看当前机器docker进程
docker ps -a

docker-swarm
设成本机为swarm集群leader
docker swarm init
查看swarm集群节点
docker node ls
获取加入swarm集群token命令(在附属机器上执行docker swarm join操作)
docker swarm join-token manager
退出集群
docker swarm leave --force

docker-service
查看集群下启动的服务
docker service ls
移除服务
docker service rm {name}
启动服务(建议用docker-compose-file方式)

########################
https://docs.docker.com/compose/install/
https://www.runoob.com/docker/docker-compose.html
docker-compose   用于dev   支持build restart  但是不支持deploy,  
swarm/stack      用于 prod  支持 deply的各种设置,包括分配cpu和内存, 但是创建容器只支持从image,不支持build.
########################
docker-compose.yml示例
######
version: "3.7"
services:
  服务名1:
    image: 镜像及版本号
    ports:
      - "主机端口:容器端口"
    networks:
      - 网络名1
    volumes:
      - 挂载目录:容器目录
    links:
      - 服务名2
    deploy:
      mode：replicated # 6个副本
      replicas: 6
      resources:
        limits:
          memory: 1024M
        reservations:
          memory: 512M
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints: [node.hostname == host1]
    environment:
      - DEBUG=1
    logging:
      driver: json-file
      options:
        max-size: "200k" # 单个文件大小为200k
        max-file: "10" # 最多10个文件

  服务名2:
    image: 镜像及版本号
    volumes:
      - 挂载目录:容器目录
    networks:
      - 网络名1
    deploy:
      resources:
        limits:
          memory: 1024M
        reservations:
          memory: 512M
      placement:
        constraints: [node.hostname == host2]
    environment:
      - DEBUG=1
    logging:
      driver: json-file
      options:
        max-size: "200k" # 单个文件大小为200k
        max-file: "10" # 最多10个文件

  服务名3:
    image: 镜像及版本号
    ports:
      - "主机端口:容器端口"
    networks:
      - 网络名1
      - 网络名2
    depends_on:
      - 服务名2
    deploy:
      resources:
        limits:
          memory: 1024M
        reservations:
          memory: 512M
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints: [node.hostname == host3]
    environment:
      - DEBUG=1
    logging:
      driver: json-file
      options:
        max-size: "200k" # 单个文件大小为200k
        max-file: "10" # 最多10个文件

networks:
  网络名1:
    external: true #事先已经存在的网络
  网络名2:
    external: true
    name: host
######

docker-compose
停止
docker-compose down
启动
docker-compose -f {文件位置} -f {文件位置} up -d

docker-stack
查看所有compose文件方式启动的服务
docker stack ls
以compose文件方式启动
docker stack deploy -c {文件位置} -c {文件位置} {别名}
删除compose文件方式启动的服务
docker stack rm {别名}
<pre>
