![Thread_pool](https://user-images.githubusercontent.com/11711898/161199222-927ff489-23d3-4420-8d36-2bd91d00d2ed.png)
```
采用juc的ThreadPoolExecutor自定义创建线程池

// 创建
private ExecutorService executorService = new ThreadPoolExecutor(Runtime.getRuntime().availableProcessors() * 10,
          Runtime.getRuntime().availableProcessors() * 30,
          1, TimeUnit.MINUTES,
          new LinkedBlockingQueue<>(1024)),
          Executors.defaultThreadFactory(),
          // 超过数量调用线程执行
          new ThreadPoolExecutor.CallerRunsPolicy());

@PreDestroy
public void destroy() {
  // 停止服务前关闭
  executorService.shutdown();
}
          
// 使用
executorService.execute(Runnable**);
```

```
采用juc的semaphore信号量来控制线程顺序

// 创建
// 10个信号量资源,获取到才处理操作
private Semaphore semaphore = new Semaphore(10);

//使用
// 获取许可证
boolean caught = semaphore.tryAcquire(10, TimeUnit.MILLISECONDS);
if (caught) {
    // do something...
    // 操作完成, 释放许可证
    semaphore.release();
}
```
