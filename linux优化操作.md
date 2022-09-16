删除2天前的log文件

find ./ -mtime +2 -name "*.log" | xargs -I {} rm -rf {}

内存释放

To free pagecache:  echo 1 > /proc/sys/vm/drop_caches

To free dentries and inodes:  echo 2 > /proc/sys/vm/drop_caches

To free pagecache, dentries and inodes:  echo 3 > /proc/sys/vm/drop_caches

echo 3 > /proc/sys/vm/drop_caches

查看服务器连接情况

netstat -n | awk '/^tcp/ {++state[$NF]} END {for(key in state) print key,"t",state[key]}'

清理僵尸进程

ps -e -o ppid,stat |grep Z| cut -d '' -f2| awk '{print $1}' | xargs kill -9

linux内核参数优化(time_wait)

<pre>
/etc/sysctl.conf增加如下配置
#time_wait优化
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_tw_reuse = 1
#队列数
net.ipv4.tcp_max_syn_backlog = 8000
#允许多少个time_wait
net.ipv4.tcp_max_tw_buckets = 5000
#close_wait关闭时长(30min)
net.ipv4.tcp_keepalive_time = 1800

生效配置文件
sysctl -p
</pre>
