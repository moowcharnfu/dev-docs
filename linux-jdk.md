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

温馨提醒！！！！由于各种依赖项目的原因, 建议还是使用oracle-jdk来开发, 避免有些不兼容的问题出现(如zk-org.apache.zookeeper:zookeeper:3.4.14就会出现找不到hostname的情况)

oracle-jdk8下载地址

https://download.oracle.com/otn/java/jdk/8u281-b09/89d678f2be164786b292527658ca1605/jdk-8u281-linux-x64.tar.gz?AuthParam=1614152409_3fe0cbcd1cbd155c92aacedeca68925a

