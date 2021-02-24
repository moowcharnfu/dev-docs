1.官网地址

https://openjdk.java.net/

2.下载地址

https://jdk.java.net/15/

wget  https://download.java.net/java/GA/jdk15.0.2/0d1cfde4252546c6931946de8db48ee2/7/GPL/openjdk-15.0.2_linux-x64_bin.tar.gz

3.安装配置

解压

tar zxvf openjdk-15.0.2_linux-x64_bin.tar.gz

移动目录

mv jdk-15.0.2/ /usr/local/

创建软链接

ln -s /usr/local/jdk-15.0.2/ /usr/local/java

配置环境变量

vi /etc/profile

export JAVA_HOME=/usr/local/java

export PATH=$JAVA_HOME/bin:$PATH

export  CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

source /etc/profile

4.查看java版本

java -version
