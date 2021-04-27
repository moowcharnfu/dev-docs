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

**使用nexus存储docker镜像, Sonatype Nexus 3 -> repositories -> Create Repository: docker (hosted) -> http端口 5000**

cat /etc/docker/daemon.json 

{

  "registry-mirrors": ["http://hub-mirror.c.163.com", "https://docker.mirrors.ustc.edu.cn", "https://registry.docker-cn.com"],
  
  "insecure-registries": ["192.168.1.1:5000" ]
  
}

systemctl daemon-reload

systemctl restart docker

systemctl status docker.service

官方doc

https://docs.docker.com/engine/reference/commandline/history/
