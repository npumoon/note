看一段关于ConcurrentHashMap的 put 与 putIfAbsent 操作的区别时，其中的value是List，用的是 CopyOnWriteArrayList。

## 线程安全的List

> `java.util.Collections` 内部静态类 SynchronizedList 

每个操作上都加锁了，遍历情况除外，用户手动加锁

```java

```

> `java.util.concurrent.CopyOnWriteArrayList`  juc 包

读不加锁，写加锁。

写的时候，先拷贝到新数组，然后写入，再赋给原`array`。

所以在写的过程中，进行读操作时，读的是旧的array中的数据，会有短暂时间的非实时数据。

```java

private transient volatile Object[] array;  // accessed only via getArray/setArray. volatile 保证可见性，happens-before

public boolean add(E e) {
    
}

// 加锁完成add方法
public boolean add(int index, E element) {
    final ReentrantLock lock = this.lock;
    lock.lock(); // 加锁
    try {
        Object[] elements = getArray();
        int len = elements.length;
        if (index > len || index < 0)
            throw new IndexOutOfBoundsException("Index: " + index + ", Size: " + len);
        
        Object[] newElements;
        int numMoved = len - index;
        if (numMoved == 0)
            newElements = Arrays.copyOf(elements, len + 1);   // 比较有意思的地方 源码里的if else 的主体代码里，如果只要一句，一般不加大括号。
        else {
            newElements = new Ojbect[len+1];
            System.arraycopy(elements,  0, newElements, 0, index); // 内部为 Java的native方法
            System.arraycopy(elements, index, newElments, index+1, numMoved);
        }
        newElements[index] = element;
        setArray(newElements);
    } finally {
        lock.unlock(); // 最后一定要解锁啊
    }
}

final Object[] getArray() {
    return array;
}

final void setArray(Object[] a) {
    array = a;
}
```



