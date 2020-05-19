删除2天前的log文件

find ./ -mtime +2 -name "*.log" | xargs -I {} rm -rf {}
