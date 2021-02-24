docker 安装参考

https://github.com/moowcharnfu/dev-docs/blob/master/docker%E5%AE%89%E8%A3%85.md

1.查找mysql镜像

docker search mysql

或者（超过10星）

docker search -f stars=10 mysql

2.拉取mysql镜像库

docker pull mysql:5.7

查看本地镜像
docker image ls

3.启动

docker run -itd --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
