docker 安装参考

https://github.com/moowcharnfu/dev-docs/blob/master/docker%E5%AE%89%E8%A3%85.md

1.查找jenkins镜像

docker search jenkins

2.拉取jenkis镜像库

docker pull jenkins/jenkins

查看本地镜像

docker image ls 

3.运行jenkins
# -d 后台运行
# -p 将容器内部端口向外映射
# --name 命名容器名称
# -v 将容器内数据文件夹或者日志、配置等文件夹挂载到宿主机指定目录
docker run -d --name jenkins -p 90:8080 -v /home/jenkins:/home/jenkins jenkins/jenkins

4.获取jenkins初始密码

查看日志
docker logs -f jenkins

初始密码放在 "/var/jenkins_home/secrets/initialAdminPassword", 需要运行命令 

docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword

6.登录使用jenkins
