```
# 1.下载

## 官方文档

https://skywalking.apache.org/docs/

https://skywalking.apache.org/docs/main/v8.5.0/readme/


## 下载地址

https://archive.apache.org/dist/skywalking/


# 2.配置

## skywalking-oap-server

conf/application.yml

修改存储方式为es并配置es地址


## skywalking-client

!!!将optional-plugins下的所有jar添加到plugins下, 使其生效得到更多apm体验

agent/config/agent.config

### 打开
agent.sample_n_per_3_secs=${SW_AGENT_SAMPLE:-1}

### 增加
### 日志上报配置

plugin.toolkit.log.grpc.reporter.server_host=${SW_GRPC_LOG_SERVER_HOST:127.0.0.1}

plugin.toolkit.log.grpc.reporter.server_port=${SW_GRPC_LOG_SERVER_PORT:11800}

plugin.toolkit.log.grpc.reporter.max_message_size=${SW_GRPC_LOG_MAX_MESSAGE_SIZE:10485760}

plugin.toolkit.log.grpc.reporter.upstream_timeout=${SW_GRPC_LOG_GRPC_UPSTREAM_TIMEOUT:30}

agent.keep_tracing=true


### 显示参数配置

plugin.mongodb.trace_param=true

plugin.mongodb.filter_length_limit=256

plugin.jdbc.trace_sql_parameters=true

plugin.jdbc.sql_parameters_max_length=512

plugin.jdbc.sql_body_max_length=2048

plugin.tomcat.collect_http_params=true

plugin.springmvc.collect_http_params=true

plugin.httpclient.collect_http_params=true

plugin.http.http_params_length_threshold=1024

plugin.http.http_headers_length_threshold=2048

plugin.feign.collect_request_body=true

plugin.feign.filter_length_limit=1024

plugin.dubbo.collect_consumer_arguments=true

plugin.dubbo.consumer_arguments_length_threshold=256

plugin.dubbo.collect_provider_arguments=true

plugin.dubbo.consumer_provider_length_threshold=256

plugin.lettuce.trace_redis_parameters=true

plugin.lettuce.redis_parameter_max_length=128

plugin.springannotation.classname_match_regex=@Bean,@Service,@Dao,@Repository


# 3.1.项目上报日志(可选)

<!-- 8.4.0 GRPCLogClientAppender 日志有坑, 升级到8.5.0来支持时间格式-->

<skywalking.version>8.5.0</skywalking.version>


## log4j1

	<dependency>
  
	<groupId>org.apache.skywalking</groupId>
  
	<artifactId>apm-toolkit-log4j-1.x</artifactId>
  
	<version>${skywalking.version}</version>
  
	</dependency>

	<dependency>
  
	<groupId>org.apache.skywalking</groupId>
  
	<artifactId>apm-toolkit-trace</artifactId>
  
	<version>${skywalking.version}</version>
  
	</dependency>

-------------------------------------------------------

### log4j.properties

log4j.rootLogger=INFO,grpc-log

#skywalking日志上报功能定义

log4j.appender.grpc-log=org.apache.skywalking.apm.toolkit.log.log4j.v1.x.log.GRPCLogClientAppender

log4j.appender.grpc-log.layout=org.apache.skywalking.apm.toolkit.log.log4j.v1.x.TraceIdPatternLayout

log4j.appender.grpc-log.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss.SSS} [%T] %-5level %logger{36} - %msg%n



## log4j2

	<dependency>
  
	<groupId>org.apache.skywalking</groupId>
  
	<artifactId>apm-toolkit-log4j-2.x</artifactId>
  
	<version>${skywalking.version}</version>
  
	</dependency>

	<dependency>
  
	<groupId>org.apache.skywalking</groupId>
  
	<artifactId>apm-toolkit-trace</artifactId>
  
	<version>${skywalking.version}</version>
  
	</dependency>

-------------------------------------------------------

### log4j2.xml

	<Appenders>
  
	<!-- skywalking日志上报功能定义 -->
  
	<GRPCLogClientAppender name="grpc-log">
  
		<PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    
	</GRPCLogClientAppender>
  
	</Appenders>

	<Loggers>
  
	<root level="info">
  
		<!-- skywalking日志上报功能实现 -->
    
		<AppenderRef ref="grpc-log" />
    
	</root>
  
	</Loggers>


## logback

	<dependency>
  
	<groupId>org.apache.skywalking</groupId>
  
	<artifactId>apm-toolkit-logback-1.x</artifactId>
  
	<version>${skywalking.version}</version>
  
	</dependency>

	<dependency>
  
	<groupId>org.apache.skywalking</groupId>
  
	<artifactId>apm-toolkit-trace</artifactId>
  
	<version>${skywalking.version}</version>
  
	</dependency>

-------------------------------------------------------

### logback.xml

<!-- skywalking日志上报功能定义 -->

	<appender name="grpc-log" class="org.apache.skywalking.apm.toolkit.log.logback.v1.x.log.GRPCLogClientAppender">
  
	<encoder class="ch.qos.logback.core.encoder.LayoutWrappingEncoder">
  
		<layout class="org.apache.skywalking.apm.toolkit.log.logback.v1.x.TraceIdPatternLogbackLayout">
    
			<pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n</pattern>
      
		</layout>
    
	</encoder>
  
	</appender>

	<root level="INFO">
  
	<!-- skywalking日志上报功能实现 -->
  
	<appender-ref ref="grpc-log" />
  
	</root>

# 3.2.手动上报追踪请求

在追踪方法上增加@Trace

@Trace

private String traceHello() {}

# 3.3.异步线程追踪请求

在异步处理类上增加@TraceCrossThread

@TraceCrossThread

public class MyTask implements Callable<String> {}

# 4.启动项目

## 启动参数增加

-javaagent:${skywalking-agent.jar地址如 /data/agent/skywalking-agent.jar} # 探针Agent jar 地址

-Dskywalking.agent.service_name=${groupName::serviceName如 local::test} # 配置 Agent 名字

-Dskywalking.collector.backend_service=${ip地址:grpc端口如 10.0.1.1:11800} # 配置 SkyWalking 服务器地址

-Dskywalking.trace.ignore_path=/test/* # 配置 SkyWalking 忽略地址, 可选操作(可以不配置此项)

-Dskywalking.plugin.toolkit.log.grpc.reporter.server_host=${ip地址如 10.0.1.1} # SkyWalking 服务器主机

-Dskywalking.plugin.toolkit.log.grpc.reporter.server_port=${grpc端口如 11800} # SkyWalking 服务器端口
	
-Dskywalking.agent.sample_n_per_3_secs=${样本上报频率如全部-1} #样本上报
	
	
# skywalking-nginx-lua
##配置前需要安装lua环境,这里使用openresty
	
http://openresty.org/cn/linux-packages.html
	
##下载官方SkyWalking Nginx LUA文件
	
https://skywalking.apache.org/downloads/

##nginx.config配置例子
<pre>
http >
    lua_package_path "${skywalking-lua路径}/lib/?.lua;;";
    # skywalking配置
    # Buffer represents the register inform and the queue of the finished segment
    lua_shared_dict tracing_buffer 100m;
    # Init is the timer setter and keeper
    # Setup an infinite loop timer to do register and trace report.
    init_worker_by_lua_block {
        local metadata_buffer = ngx.shared.tracing_buffer
 
        metadata_buffer:set('serviceName', '组::服务名')
        -- Instance means the number of Nginx deloyment, does not mean the worker instances
        metadata_buffer:set('serviceInstanceName', '1')
        -- type 'boolean', mark the entrySpan include host/domain
        metadata_buffer:set('includeHostInEntrySpan', false)
 
        -- set randomseed
        require("skywalking.util").set_randomseed()
 
        #http端口12800
        require("skywalking.client"):startBackendTimer("http://10.0.1.1:12800")
 
        -- Any time you want to stop reporting metrics, call `destroyBackendTimer`
        -- require("skywalking.client"):destroyBackendTimer()
 
        -- If there is a bug of this `tablepool` implementation, we can
        -- disable it in this way
        -- require("skywalking.util").disable_tablepool()
 
        skywalking_tracer = require("skywalking.tracer")
    }
	
server>
	location = /lua {
	  default_type text/html;  
	  rewrite_by_lua_block {
			skywalking_tracer:start("backend service")
	   }
	   
	  content_by_lua_block {
			ngx.say("hello, world")
			local uri = ngx.var.uri
			ngx.say("uri:")
			ngx.say(uri)
			local headers=ngx.req.get_headers()
			local ip=headers["X-REAL-IP"] or headers["X_FORWARDED_FOR"] or ngx.var.remote_addr or "0.0.0.0"
			ngx.say("ip:")
			ngx.say(ip)
	  }

	  body_filter_by_lua_block {
			if ngx.arg[2] then
				skywalking_tracer:finish()
			end
	  }

	  log_by_lua_block {
			skywalking_tracer:prepareForReport()
	  }
	}
<pre>
```


