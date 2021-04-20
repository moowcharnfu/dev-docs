删除2天前的log文件

find ./ -mtime +2 -name "*.log" | xargs -I {} rm -rf {}

内存释放

To free pagecache:  echo 1 > /proc/sys/vm/drop_caches

To free dentries and inodes:  echo 2 > /proc/sys/vm/drop_caches

To free pagecache, dentries and inodes:  echo 3 > /proc/sys/vm/drop_caches

echo 3 > /proc/sys/vm/drop_caches

查看服务器连接情况

netstat -n | awk '/^tcp/ {++state[$NF]} END {for(key in state) print key,"t",state[key]}'
