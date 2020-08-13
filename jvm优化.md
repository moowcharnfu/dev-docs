vm-options优化参数

-Xms500M -Xmx500M -XX:+UseG1GC -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintHeapAtGC -Xloggc:E:\opt\gc\logback-gc.log

Xms初始堆大小

Xmx最大堆大小

XX:+UseG1GC 使用G1垃圾收集器, 此gc比CMSGC(并发标记清除)\SerialGC(串行)\parallelGC(并行)高效

XX:+PrintHeapAtGC 在进行GC的前后打印出堆的信息

Xloggc:E:\opt\gc\logback-gc.log gc的日志文件位置

处理方式: 复制, 标记, 标记清除, 标记压缩, 重置等, stw(Stop-The-World机制简称STW,全局暂停现象，全局停顿)
