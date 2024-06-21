```
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

docker build --label version=版本号 -t [标签名:版本号] -f Dockerfile .

docker run -d --restart=always --name [别名] -p 主机端口:容器端口 -v 主机目录:容器目录 [标签名:版本号]
docker exec -it [别名] /bin/bash
docker logs -f [别名]


spring-boot集成docker插件配置示例

<!--镜像仓库的项目-->
<docker.image.prefix>ai</docker.image.prefix>
<!--镜像仓库的ip-->
<docker.registry>192.168.12.207</docker.registry>
<!--镜像仓库的port-->
<docker.port>5000</docker.port>

*.Dockerfile
FROM primetoninc/jdk:1.8
ADD ai-manager-1.0.0.jar /usr/app/
ENV JAVA_OPTIONS ""
ENV HOME /usr/app
WORKDIR /usr/app/
ENTRYPOINT ["sh", "-c", "java -jar $JAVA_OPTIONS ai-manager-1.0.0.jar"]

<!-- Dockerfile方式 -->
    <plugin>
        <groupId>com.spotify</groupId>
        <artifactId>docker-maven-plugin</artifactId>
        <version>${docker.plugin.version}</version>
        <executions>
            <execution>
                <phase>install</phase>
                <goals>
                    <goal>build</goal>
                </goals>
            </execution>
            <execution>
                <id>tag-image</id>
                <phase>install</phase>
                <goals>
                    <goal>tag</goal>
                </goals>
                <configuration>
                    <image>${docker.image.prefix}/${project.artifactId}:${project.version}</image>
                    <newName>${docker.registry}:${docker.port}/${docker.image.prefix}/${project.artifactId}:${project.version}</newName>
                </configuration>
            </execution>
            <execution>
                <id>push-image</id>
                <phase>install</phase>
                <goals>
                    <goal>push</goal>
                </goals>
                <configuration>
                    <imageName>${docker.registry}:${docker.port}/${docker.image.prefix}/${project.artifactId}:${project.version}</imageName>
                </configuration>
            </execution>
        </executions>

        <configuration>
            <imageName>${docker.image.prefix}/${project.artifactId}:${project.version}</imageName>
            <dockerDirectory>${project.basedir}/src/main/resources/docker</dockerDirectory>
            <resources>
                <resource>
                    <targetPath>/</targetPath>
                    <directory>${project.build.directory}</directory>
                    <include>${project.build.finalName}.jar</include>
                </resource>
            </resources>
            <!--<imageTags>
                <imageTag>${project.version}</imageTag>
                <imageTag>latest</imageTag>
            </imageTags>-->
            <!-- 如果有harbor账号,需要配置在maven配置xml中 -->
            <serverId>harbor</serverId>
            <registryUrl>http://${docker.registry}:${docker.port}/</registryUrl>
            <!-- docker主机默认端口2375 -->
            <!-- <dockerHost>http://${docker.registry}:2375</dockerHost> -->
            <dockerHost>http://${docker.registry}:4243</dockerHost>
        </configuration>
    </plugin>

<!-- 插件参数方式 -->
    <plugin>
        <groupId>com.spotify</groupId>
        <artifactId>docker-maven-plugin</artifactId>
        <version>${docker.plugin.version}</version>
        <executions>
            <execution>
                <phase>install</phase>
                <goals>
                    <goal>build</goal>
                </goals>
            </execution>
            <execution>
                <id>tag-image</id>
                <phase>install</phase>
                <goals>
                    <goal>tag</goal>
                </goals>
                <configuration>
                    <image>${docker.image.prefix}/${project.artifactId}:${project.version}</image>
                    <newName>${docker.registry}:${docker.port}/${docker.image.prefix}/${project.artifactId}:${project.version}</newName>
                </configuration>
            </execution>
            <execution>
                <id>push-image</id>
                <phase>install</phase>
                <goals>
                    <goal>push</goal>
                </goals>
                <configuration>
                    <imageName>${docker.registry}:${docker.port}/${docker.image.prefix}/${project.artifactId}:${project.version}</imageName>
                </configuration>
            </execution>
        </executions>

        <configuration>
            <imageName>${docker.image.prefix}/${project.artifactId}:${project.version}</imageName>
            <baseImage>primetoninc/jdk:1.8</baseImage>
            <env>
                <JAVA_OPTIONS>""</JAVA_OPTIONS>
                <HOME>/usr/app</HOME>
            </env>
            <workdir>/usr/app/</workdir>
            <entryPoint>["sh", "-c", "java $JAVA_OPTIONS  -jar ai-identity-1.0.0.jar"]</entryPoint>
            <resources>
                <resource>
                    <targetPath>/usr/app</targetPath>
                    <directory>${project.build.directory}</directory>
                    <include>${project.build.finalName}.jar</include>
                </resource>
            </resources>
            <!--<imageTags>
                <imageTag>${project.version}</imageTag>
                <imageTag>latest</imageTag>
            </imageTags>-->
            <!-- 如果有harbor账号,需要配置在maven配置xml中 -->
            <serverId>harbor</serverId>
            <registryUrl>http://${docker.registry}:${docker.port}/</registryUrl>
            <!-- docker主机默认端口2375 -->
            <!-- <dockerHost>http://${docker.registry}:2375</dockerHost> -->
            <dockerHost>http://${docker.registry}:4243</dockerHost>
        </configuration>
    </plugin>


```
