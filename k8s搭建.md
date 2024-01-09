```

准备三台机器, 内存3G+
172.17.204.175 k8s-master
172.17.206.7 k8s-n1
172.17.199.243 k8s-n2

1.下载最新版kubectl/kubelet/kubeadm
kubeadm：用来初始化集群的指令。
kubelet：在集群中的每个节点上用来启动 Pod 和容器等。
kubectl：用来与集群通信的命令行工具。
crictl:下载镜像用

sudo apt install containerd socat conntrack docker.io
开机启动
sudo systemctl enable docker && sudo systemctl start docker 

# 安装基础软件并设置源
sudo apt-get install -y ca-certificates curl software-properties-common apt-transport-https
curl -s https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | sudo apt-key add -
sudo tee /etc/apt/sources.list.d/kubernetes.list <<EOF
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF

# 刷新软件列表，然后直接安装(3台都运行)
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl

# 阻止自动更新(apt upgrade时忽略)。所以更新的时候先unhold，更新完再hold。
sudo apt-mark hold kubelet kubeadm kubectl

====== 重要(3台都运行) =====
关闭swap
临时关
sudo swapoff -a
永久关
sudo sed -i 's/.*swap.*/#&/' /etc/fstab

3.执行测试，以保障你安装的版本是最新的：
crictl --version
kubelet --version
kubeadm version
kubectl version --client

开机启动kubelet(3台都运行)
sudo cp /lib/systemd/system/kubelet.service /etc/systemd/system/kubelet.service
sudo systemctl enable kubelet

ss -ant/netstat -ant 查看端口

4.修改hosts(将本机Ip换个别名), (3台都运行)
cat /etc/hosts
172.17.204.175 k8s-master
172.17.206.7 k8s-n1
172.17.199.243 k8s-n2


5.生成配置文件(mster运行)
kubeadm config print init-defaults --component-configs KubeletConfiguration > kubeadm-config.yaml

修改内容
localAPIEndpoint:advertiseAddress: 172.17.204.175 #修改成本机ip
nodeRegistration:name: k8s-master #修改master节点名
imageRepository: registry.cn-hangzhou.aliyuncs.com/google_containers   #修改为阿里云地址

6.镜像(mster运行)
kubeadm config images list --config=kubeadm-config.yaml
拉取镜像
sudo kubeadm config images pull --config=kubeadm-config.yaml

##查看已安装的镜像
需要配置/etc/crictl.yaml
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 10
debug: false

查看镜像
sudo crictl images
查看进程
sudo crictl ps -a

7.初始化master节点
# 由于默认拉取镜像地址k8s.gcr.io国内无法访问，这里需要指定阿里云镜像仓库地址
sudo kubeadm init --config=kubeadm-config.yaml

8. 部署网络插件calico(3台都运行)
master运行
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
拷贝到其他节点
sudo scp /etc/cni/net.d/10-calico.conflist pc@k8s-n1:/etc/cni/net.d/
sudo scp /etc/cni/net.d/calico-kubeconfig pc@k8s-n2:/etc/cni/net.d/
修改cluster节点10-calico.conflist中的nodename为 k8s-n1 和 k8s-n2

9. master 生成加入集群的token并scp下发到目标机器
kubeadm token create --print-join-command

10. cluster 加入集群
sudo kubeadmin join *******

11. master 查看集群情况
kubectl get nodes
kubectl describe nodes

12.部署dashboard
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

################################# 排查问题 ##########################################
journalctl -xefu kubelet
kubectl get componentstatus

证书问题
sudo kubeadm certs check-expiration
sudo kubeadm certs renew all

可能问题时间不同步,调整成相同时区相同时间即可
"The connection to the server 172.17.204.175:6443 was refused - did you specify the right host or port?"
sudo timedatectl set-timezone Asia/Shanghai

```
