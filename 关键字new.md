
```java
Person p = new Person();
```

new 关键字干了些什么

分配内存……，哈哈哈

### 检验类是否被加载到JVM中（常量池中定位到一个类的符号引用），如果没有找到，则先进行类的加载、解析、初始化。

### 分配内存（根据垃圾收集器是否整理内存来决定使用哪种方式分配内存）
1. 指针碰撞（Bump the Pointer）: 针对连续内存
2. 空闲列表（Free List）：针对非连接内存
  
  主流垃圾回收器：
   CMS （Mark Sweep，标记清理，使用 空闲列表）；
   Parallel Scavenge（标记-复制）
  
  分配时，考虑线程安全问题：
  1. CAS + 失败重试
  2. 内存分配在各自线程空间内进行。每个线程在java堆上预先分配一小块内存，Thread Local Allocation Buffer（TLAB），TLAB用完时需要重新分配新的TLAB，才会进行同步锁定
  
### 初始化零值

除对象头之外的内存空间初始化为零值。

### 设置对象头信息

对象头里：对象属于哪个类的实例，元数据信息，对象哈希code，对象的GC分代年龄。

### 初始化对象 
`< init >`方法
