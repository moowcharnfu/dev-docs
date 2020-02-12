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
