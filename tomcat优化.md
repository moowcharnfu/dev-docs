max-threads 并发处理能力

min-spare-threads 初始化线程大小

accept-count 同时接收请求大小

max-connections 同时接收请求满了之后还能接收多少请求

org.apache.coyote.http11.Http11NioProtocol NIO无阻塞协议,spring-boot默认支持,tomcat默认HTTP/1.1

[tomcat]

conf/server.xml

<Connector port="8080" protocol="org.apache.coyote.http11.Http11NioProtocol"

               maxThreads="200" minSpareThreads="100" maxConnections="10000" acceptCount="10000"
               
               connectionTimeout="20000"
               
               redirectPort="8443"
               
               URIEncoding="UTF-8"/>
               
               
[spring-boot]

server.tomcat.accept-count=10000

server.tomcat.min-spare-threads=100

server.tomcat.max-threads=300

server.tomcat.max-connections=10000
