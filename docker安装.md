docker 安装

1.添加yum源

yum install epel-release -y

yum clean all

yum list

2.安装并运行Docker

yum install docker-io –y

systemctl start docker

3.检查安装结果

docker info

4.镜像加速

cat /etc/docker/daemon.json 

{

  "registry-mirrors": ["http://hub-mirror.c.163.com", "https://docker.mirrors.ustc.edu.cn", "https://registry.docker-cn.com"]
  
}

systemctl daemon-reload

systemctl restart docker

systemctl status docker.service

官方doc

https://docs.docker.com/engine/reference/commandline/history/
