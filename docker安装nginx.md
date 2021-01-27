docker 安装参考

https://github.com/moowcharnfu/dev-docs/blob/master/docker%E5%AE%89%E8%A3%85.md

nginx启动(没有自动下载)

docker run --name nginx -p 83:80 -d nginx

创建本地挂载目录

mkdir -p /usr/local/nginx/logs /usr/local/nginx/conf /usr/local/nginx/www

拷贝容器文件到本地

docker cp nginx:/etc/nginx/nginx.conf /usr/local/nginx/conf/

######## nginx自定义配置 #

/usr/local/nginx/conf/conf.d/default.conf

location /gzs {

   alias /etc/nginx/www;
   
   try_files $uri $uri/ /index.html;
   
   index index.html index.htm;
   
}


启动nginx

docker stop nginx

docker rm nginx

docker run -d -p 83:80 --net host --name nginx -v /usr/local/nginx/www:/etc/nginx/www -v /usr/local/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /usr/local/nginx/conf/conf.d/default.conf:/etc/nginx/conf.d/default.conf -v /usr/local/nginx/logs:/var/log/nginx nginx
