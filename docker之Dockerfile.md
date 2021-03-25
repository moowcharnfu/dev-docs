#oracle-jdk镜像下载到docker容器里

FROM primetoninc/jdk:1.8

#拷贝文件或者目录,建议是相对路径的源,尽量不请求网络

COPY [源] [目标] 或者 ADD [源] [目标]

#环境变量

ENV [名称] [值]

#工作目录

WORKDIR [目录位置]

#容器执行命令

ENTRYPOINT ["sh", "-c", "具体命令"] exec模式 或者 CMD ["sh", "", ""]


示例[Dockerfile]:

=====

FROM primetoninc/jdk:1.8

WORKDIR /usr/app

#ENTRYPOINT ["sh", "-c", "java -version"]

#CMD ["sh", "-c", "java -version"]

RUN ["sh", "-c", "java -version"]

===

构建、执行并查看日志

docker build -t [标签名] -f Dockerfile .

docker run -d [标签名] --name [别名]

docker logs -f [别名]
