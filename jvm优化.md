vm-options优化参数

# 例子

# 配置内存大小(初始化堆大小512M;最大堆大小1G;线程堆栈大小1M;永久区初始大小128M;永久区最大大小256M;老年代与新生代比例为2即新生代1/3,老年代2/3;新生代与survivor比例为8即新生代8/10,fromSpace1/10,toSpace1/10;使用G1GC垃圾收集器)

# version < jdk8
# JAVA_OPTS="-Xms512m -Xmx1024m -Xss1024K -XX:PermSize=128m -XX:MaxPermSize=256m -XX:NewRatio=2 -XX:SurvivorRatio=8 -XX:+UseG1GC -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintHeapAtGC -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp -Xloggc:/tmp/sso.gc.log"

# version >= jdk8(PermSize->MetaspaceSize)
# JAVA_OPTS="-Xms512m -Xmx1024m -Xss1024K -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=256m -XX:NewRatio=2 -XX:SurvivorRatio=8 -XX:+UseG1GC -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintHeapAtGC -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp -Xloggc:/tmp/sso.gc.log"



-Xms500M -Xmx500M -XX:+UseG1GC -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintHeapAtGC -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp -Xloggc:E:\opt\gc\logback-gc.log

Xms初始堆大小

Xmx最大堆大小

XX:+UseG1GC 使用G1垃圾收集器, 此gc比CMSGC(并发标记清除)\SerialGC(串行)\parallelGC(并行)高效

XX:+PrintHeapAtGC 在进行GC的前后打印出堆的信息

-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp JVM在遇到OOM(OutOfMemoryError)时生成Dump文件

Xloggc:E:\opt\gc\logback-gc.log gc的日志文件位置

XX:G1HeapRegionSize=1M/2M/4M/8M/16M/32M G1内堆内存区块大小，G1将堆内存默认均分为2048块，1M<region<32M，(Xms + Xmx ) /2 / 2048 , 不大于32M，不小于1M，且为2的指数

处理方式: 复制, 标记, 标记清除, 标记压缩, 重置等, stw(Stop-The-World机制简称STW,全局暂停现象，全局停顿)



g1gc官方文档
https://www.oracle.com/technetwork/tutorials/tutorials-1876574.html

------------------

jstat -gcutil [pid] 1000 10


hprof观察内存消耗

jmap -F -dump:live,format=b,file=/tmp/[pid].hprof [pid]

然后用MAT加载日志文件来观察



