docker 安装参考

https://github.com/moowcharnfu/dev-docs/blob/master/docker%E5%AE%89%E8%A3%85.md

1.查找gitlab镜像

docker search gitlab

或者（超过10星）

docker search -f stars=10 gitlab

2.拉取gitlab镜像库

docker pull gitlab/gitlab-ce
# 查看本地镜像
docker image ls

3.运行gitlab

# -d 后台运行
# -p 将容器内部端口向外映射
# --name 命名容器名称
# -v 将容器内数据文件夹或者日志、配置等文件夹挂载到宿主机指定目录
docker run -d -p 8443:443 -p 8480:80 -p 8422:22 --name gitlab --restart unless-stopped -v /home/gitlab/config:/etc/gitlab -v /home/gitlab/logs:/var/log/gitlab -v /home/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce

可用

docker run --detach -p 8443:443 -p 80:80 -p 10022:22 --name gitlab --restart=unless-stopped --volume /var/lib/docker/volumes/gitlab-data/etc:/etc/gitlab --volume /var/lib/docker/volumes/gitlab-data/log:/var/log/gitlab --volume /var/lib/docker/volumes/gitlab-data/data:/var/opt/gitlab gitlab/gitlab-ce

4.配置gitlab使用Ip访问

vi /home/gitlab/config/gitlab.rb
# 配置http协议所使用的访问地址,不加端口号默认为80
external_url 'http://192.168.4.110'

# 配置ssh协议所使用的访问地址和端口
gitlab_rails['gitlab_ssh_host'] = '192.168.4.110'
# # 此端口是run时22端口映射的8422端口
gitlab_rails['gitlab_shell_ssh_port'] = 8422


# 修改
unicorn['listen'] = 'localhost'
unicorn['port'] = 8080
# 修改
nginx['listen_port'] = 80

5.重启

docker restart gitlab

查看日志

docker logs -f gitlab

6.登录使用gitlab
