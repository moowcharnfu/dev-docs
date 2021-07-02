```

1.安装zookeeper

2.使用dubbo
1)pom.xml
<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-dependencies</artifactId>
			<version>${spring-boot.version}</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
		<dependency>
			<groupId>org.apache.dubbo</groupId>
			<artifactId>dubbo-bom</artifactId>
			<version>${dubbo.version}</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
		<dependency>
			<groupId>org.apache.dubbo</groupId>
			<artifactId>dubbo-dependencies-zookeeper</artifactId>
			<version>${dubbo.version}</version>
			<type>pom</type>
		</dependency>
	</dependencies>
</dependencyManagement>

<dependencies>
	<!-- spring -->
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-test</artifactId>
		<scope>test</scope>
	</dependency>

	<dependency>
		<groupId>org.apache.dubbo</groupId>
		<artifactId>dubbo</artifactId>
	</dependency>
	<dependency>
		<groupId>org.apache.dubbo</groupId>
		<artifactId>dubbo-dependencies-zookeeper</artifactId>
		<type>pom</type>
	</dependency>
</dependencies>

<properties>
	<java.version>1.8</java.version>
	<maven.compiler.source>${java.version}</maven.compiler.source>
	<maven.compiler.target>${java.version}</maven.compiler.target>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	<spring-boot.version>2.1.6.RELEASE</spring-boot.version>
	<dubbo.version>3.0.0</dubbo.version>
</properties>


2)创建接口层
public interface ***Service{}

3)创建provider
实现接口并添加@DubboService标注是提供者
#配置文件
# dubbo application
dubbo.application.name=${spring.application.name}

# Dubbo Registry
dubbo.registry.address=zookeeper://127.0.0.1:2181

# 是否简化url到注册中心
dubbo.registry.simplified=false

# 配置中心
dubbo.metadata-report.address=zookeeper://127.0.0.1:2181
dubbo.config-center.address=zookeeper://127.0.0.1:2181

# Dubbo Protocol
dubbo.protocol.name=dubbo
dubbo.protocol.port=20880
# 线程池大小
dubbo.protocol.threads=300
# 最大连接数
dubbo.protocol.accepts=3000
# fixed或者cached, cached在空闲1分钟后会回收
dubbo.protocol.threadpool=fixed
# message即只有请求响应消息派发到线程池, 其它连接断开事件\心跳等消息\直接在IO线程上执行(默认all)
dubbo.protocol.dispatcher=message

#qos是dubbo的在线运维命令
dubbo.application.qos-port=21880
dubbo.application.qos-enable=true
dubbo.application.qos-accept-foreign-ip=false

# provider
# 全局超时时间
dubbo.provider.timeout=3000
# 全局重试次数
dubbo.provider.retries=0

# 接口超时时间
dubbo.service.***Service.timeout=5000
# 接口并发访问数
dubbo.service.***Service.executes=200

4)创建consumer
注入接口并添加@DubboReference标注为消费者
#配置文件
# dubbo application
dubbo.application.name=${spring.application.name}

# Dubbo Registry
dubbo.registry.address=zookeeper://127.0.0.1:2181
# 是否简化url到注册中心
dubbo.registry.simplified=false

#qos是dubbo的在线运维命令
dubbo.application.qos-port=21881
dubbo.application.qos-enable=true
dubbo.application.qos-accept-foreign-ip=false

# consumer
# 全局超时时间
dubbo.consumer.timeout=2000
# 全局重试次数
dubbo.consumer.retries=2
# 接口负载均衡策略(random, roundrobin, leastactive,详细见#org.apache.dubbo.common.constants.LoadbalanceRules)
dubbo.consumer.loadbalance=leastactive
# 接口集群策略(failover, failfast, failsafe, failback, forking, 详细见#org.apache.dubbo.common.constants.ClusterRules)
dubbo.consumer.cluster=failfast
# 接口最大请求数
dubbo.consumer.actives=1000

# 接口超时时间
dubbo.reference.***Service.timeout=3000

3.访问
#qos
http://localhost:21881/ls或者telnet localhost 21881 > ls
#项目接口


dubbo工作原理

![image](https://images2018.cnblogs.com/blog/897247/201805/897247-20180503190329662-1922174585.png)

```
