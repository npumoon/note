> 测试某`CyclicBarrier`时，`newFixedThreadPool`之后，没有进行`shutdown`，`main`方法结束后，程序没有自动 `exit`； 使用 `newCachedThreadPool`之后，`main`方法结束，`60s`后，程序自动结束了。
> 
>程序结束：`JVM`退出

toc

这看起来和KeepAliveTime有关
------

```java
ThreadPoolExecutor(
    int corePoolSize,
    int maximumPoolSize,
    long keepAliveTime,
    TimeUnit unit,
    ...)
```

`keepAliveTime` : 
> when the number of threads is greater than the **core**, this is the maximum time that **excess idle threads** will wait for new tasks before terminating.

超过corePoolSize的**空闲线程**，指定时间过后，会被结束掉。

#### 1. newFixedThreadPool
```java
public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }

```

如果存在多于`corePoolSize`的空闲线程（这也不太能啊，core和maximum一样），立马被结束掉，不会存活下来。

*tips*
>
> new LinkedBlockingQueue()
> 无界阻塞队列，最大值：Integer.MAX_VALUE 
> maximum怕是用不上咯。

#### 2. newCachedThreadPool
```java
public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }
```
60s后，多于`corePoolSize`(0)的空闲线程就要被结束掉，所以完成任务后，再空闲60s，这个线程池就拜拜了。

------

那开头的疑惑，算是解了半个

`fixedThreadPool` 一直有存活的线程（core)，线程池没有关闭；
`cachedThreadPool`空闲60s后，就没有存活的线程了，线程池自动关闭；

那还有一个疑惑：

### 线程池没有自动 shutdown 的情况下
`main`作为主线程，还需要等待线程池内的线程结束么？

普通线程，`main`执行完，子线程执行完，也就结束了。
而线程池内的线程，处于空闲状态，是活着的，非守护线程（可进行设置）

所以`main`线程实际上是结束了的，只不过其他还有非守护线程存在（线程池里的），所以实际上是JVM不会退出。

### JVM何时退出

1. `main`方法执行完成并返回后，应用内不存在其他用户线程，JVM就会关闭并退出；

2. 内部调用 `System.exit(0);`，这种情况下，无论应用内是否存在用户线程、守护线程，都会强制退出。


### tips
> https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/lang/Thread.html
> 
`public final void setDaemon​(boolean on)`
>Marks this thread as either a daemon thread or a user thread. **The Java Virtual Machine exits when the only threads running are all daemon threads.**

This method must be invoked before the thread is started.







