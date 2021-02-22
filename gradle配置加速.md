[build.gradle]

maven{ url 'https://maven.aliyun.com/repository/central'}

maven{ url 'https://maven.aliyun.com/repository/public'}

maven{ url 'https://maven.aliyun.com/repository/google'}

maven{ url 'https://maven.aliyun.com/repository/gradle-plugin'}

maven{ url 'https://maven.aliyun.com/repository/spring'}

maven{ url 'https://maven.aliyun.com/repository/spring-plugin'}

maven{ url 'https://maven.aliyun.com/repository/grails-core'}

maven{ url 'https://maven.aliyun.com/repository/apache-snapshots'}

mavenLocal()


[gradle.properties]

#开启并行编译

org.gradle.parallel=true

#开启守护进程 通过开启守护进程，下一次构建的时候，将会连接这个守护进程进行构建，而不是重新fork一个gradle构建进程

org.gradle.daemon=true

#启用新的孵化模式

org.gradle.configureondemand=true

#开启 Gradle 缓存

org.gradle.caching = true

#调整jvm内存, 默认1G

org.gradle.jvmargs=-Xmx2048m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8

