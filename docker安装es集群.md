docker 安装参考

https://github.com/moowcharnfu/dev-docs/blob/master/docker%E5%AE%89%E8%A3%85.md

1.查找elasticsearch镜像

docker search elasticsearch

或者（超过10星）

docker search -f stars=10 elasticsearch

2.es搭建

# 创建es目录

mkdir -p /data/elasticsearch/config

mkdir -p /data/elasticsearch/data

chmod +777 -R /data/elasticsearch


# 虚拟内存调整

sysctl -a|grep vm.max_map_count

sysctl -w vm.max_map_count=262144

sysctl -p


# 增加集群配置

/data/elasticsearch/config/es1.yml

cluster.name: skywalking

node.name: node-1

network.bind_host: 0.0.0.0

network.publish_host: 192.168.4.15

http.port: 9200

transport.tcp.port: 9300

http.cors.enabled: true

http.cors.allow-origin: "*"

node.master: true

node.data: true

discovery.zen.ping.unicast.hosts: ["192.168.4.15:9300", "192.168.4.39:9300"]

discovery.zen.minimum_master_nodes: 1


/data/elasticsearch/config/es2.yml

cluster.name: skywalking

node.name: node-2

network.bind_host: 0.0.0.0

network.publish_host: 192.168.4.39

http.port: 9200

transport.tcp.port: 9300

http.cors.enabled: true

http.cors.allow-origin: "*"

node.master: true

node.data: true

discovery.zen.ping.unicast.hosts: ["192.168.4.15:9300", "192.168.4.39:9300"]

discovery.zen.minimum_master_nodes: 1


# docker操作

docker stop elasticsearch

docker rm elasticsearch

docker logs -f elasticsearch


es1:

docker run --name elasticsearch -p 9200:9200 -p 9300:9300 \

-e ES_JAVA_OPS="-Xms256m -Xmx256m" \

-v /data/elasticsearch/config/es1.yml:/usr/share/elasticsearch/config/elasticsearch.yml \

-v /data/elasticsearch/data:/usr/share/elasticsearch/data \

-d elasticsearch:6.5.0



es2:

docker run --name elasticsearch -p 9200:9200 -p 9300:9300 \

-e ES_JAVA_OPS="-Xms256m -Xmx256m" \

-v /data/elasticsearch/config/es2.yml:/usr/share/elasticsearch/config/elasticsearch.yml \

-v /data/elasticsearch/data:/usr/share/elasticsearch/data \

-d elasticsearch:6.5.0

# 查看日志, 正常后检查es状态

http://192.168.4.15:9200/_cat/nodes?v

http://192.168.4.39:9200/_cat/nodes?v


