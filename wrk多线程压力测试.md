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
#################################
test.lua 
-- start

-- 请求方式post
wrk.method = "POST"
-- 请求头部 多媒体文件类型
wrk.headers["Content-Type"] = "multipart/form-data; boundary=----img"
-- 处理本地图片
local fileName="hongyoumixian.PNG" -- 图片名
local fileType="image/png" -- image/jpeg --图片对应类型
local f = io.open("/home/moowcharnfu/Pictures/hongyoumixian.PNG", "rb") -- 读取本地文件
-- 请求实体
wrk.body = '------img\r\nContent-Disposition: form-data; name="files"; filename="'..fileName..'"\r\nContent-Type: '..fileType..'\r\n\r\n'..f:read("*all")..'\r\n------img\r\nContent-Disposition: form-data; name="path"\r\n\r\npath\r\n------img--\r\n'

-- 视情况不打印结果
response = function(status, headers, body)
        print(body) --调试用，正式测试时需要关闭，因为解析response非常消耗资源
end

-- end
#################################

```
