看一段关于ConcurrentHashMap的 put 与 putIfAbsent 操作的区别时，其中的value是List，用的是 CopyOnWriteArrayList。

## 线程安全的List

`java.util.Collections` 内部静态类 SynchronizedList 

每个操作上都加锁了，遍历情况除外，用户手动加锁

```java

```

`java.util.concurrent.CopyOnWriteArrayList`  juc 包

读不加锁，写加锁。

写的时候，先拷贝到新数组，然后写入，再赋给原`array`。

```java


```



