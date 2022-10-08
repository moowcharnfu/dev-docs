```
linux 专用接口测试工具 wrk, 区别于apache ab在wrk可以支持多线程同时进行, 线程数建议 cpu*2/3/4多路复用


命令
get请求(12个线程,1200个并发,持续3分钟测试某个接口)
wrk -t12 -c1200 -d3m http://*******/

!!!post方式需要自己写lua脚本,示例:
#################################
test.lua 
-- start

wrk.method = "POST"
wrk.body = '{"key1": "value1","key2": "value2"}'
wrk.headers["Content-Type"] = "application/json;charset=UTF-8"

-- 视情况不打印结果
response = function(status, headers, body)
print(body) --调试用，正式测试时需要关闭，因为解析response非常消耗资源
end

-- end
#################################
post请求(-s表示lua脚本路径)
wrk -t1 -c1 -d1s -s test.lua http://******/**

post文件请求

```
