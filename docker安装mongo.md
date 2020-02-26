docker 安装参考

https://github.com/moowcharnfu/dev-docs/blob/master/docker%E5%AE%89%E8%A3%85.md

1.查找mongo镜像

docker search mongo

2.拉取mongo镜像库

docker pull mongo

查看本地镜像

docker image ls 

3.运行mongo
# -d 后台运行
# -p 将容器内部端口向外映射
# --name 命名容器名称
# -v 将容器内数据文件夹或者日志、配置等文件夹挂载到宿主机指定目录
docker run -d --name mongo -p 27017:27017 -v /home/mongo:/data/db mongo

4.查看日志
docker logs -f mongo

# 初始化mongo数据库
6.登录使用mongo

docker exec -it mongo /bin/bash

进入mongo命令界面

mongo

1.使用admin数据库

use admin

2.创建新用户

db.createUser({user:"admin",pwd:"admin",roles:["userAdminAnyDatabase"]})

3.验证用户可用性,返回1表示创建成功

db.auth("admin","admin")

4.退出

exit
