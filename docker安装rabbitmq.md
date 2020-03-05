docker 安装参考

https://github.com/moowcharnfu/dev-docs/blob/master/docker%E5%AE%89%E8%A3%85.md

1.查找rabbitmq镜像(rabbitmq-management是有管理后台的)

docker search rabbitmq:management

2.拉取rabbitmq镜像库

docker pull rabbitmq:management

查看本地镜像

docker image ls

3.运行rabbitmq
# -d 后台运行
# -p 将容器内部端口向外映射
# --name 命名容器名称
设置rabbmitmq用户名和密码为amdin/admin

docker run -d --name rabbitmq -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin -p 15672:15672 -p 5672:5672 rabbitmq:management

4.查看日志 docker logs -f rabbitmq
