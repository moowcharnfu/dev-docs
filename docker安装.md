前序操作:

selinux有三种模式：enforcing 强制 permissive 宽容的 diabled 禁止的，顾名思义，权限限制，从高到低

cat /etc/selinux/config

SELINUX=disabled

重装docker

rpm -qa | grep docker

yum remove 对应docker包

curl -fsSL https://get.docker.com/ | sh

systemctl restart docker

stemctl enable docker

docker version

--------------------------------

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
  
  "insecure-registries": ["192.168.1.1:5000" ],

  "bip": "172.22.001/24" #docker0网桥段配置
}

systemctl daemon-reload

systemctl restart docker

systemctl status docker.service

官方doc

https://docs.docker.com/engine/reference/commandline/history/
