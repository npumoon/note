## JDK8 默认垃圾收集算法
```vim
$ java -XX:+PrintCommandLineFlags -version
-XX:InitialHeapSize=262253696 -XX:MaxHeapSize=4196059136 -XX:+PrintCommandLineFlags -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:-UseLargePagesIndividualAllocation -XX:+UseParallelGC
java version "1.8.0_131"
Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)
```

首行最后一个：-XX:+UseParallelGC
年轻代：Parallel Scavenge
老年代：Parallel Old

### Parallel Scavenge



## 堆分配的理论计算与实际

### 实际

eden : 六百多兆；
survivor(2)：十几兆；

### 理论计算
初始化堆内存（默认），其他也是默认
>    最大：`-Xmx2048m`
>    最小：`-Xms2048m`
>
> 年轻代：老年代 = 1 ： 2
> 年轻代比例： `eden ： survivor : survivor = 8 : 1 ：1`

计算下来：
```
young = 2048 * (1/3) = 682.66666666
eden = young * (8/10) = 546.1333333
survivor * 2 = young * (2/10) = 136.533333

old = 2048 * (2/3) = 1365.3333333
```

young区：实际与理论的 eden 大小差距一百多兆，survivor差距一百多兆。

搜索了一下，这个参数 `+useAdaptiveSizePolicy` 影响着内存分配比例问题。

### AdaptiveSizePolicy
JVM GC Ergonomics（自适应调节策略） 的一部分

开启：
> +useAdaptiveSizePolicy

**JDK8 默认使用 UseparallelGC 垃圾回收器，该垃圾回收器默认启动了 AdaptiveSizePolicy。**

它会自适应、自动的调整各种参数，为了获取协调后的最佳比例。
每次GC后，重新计算 Eden、From和To区的大小，计算依据是GC过程中统计的 GC时间、吞吐量、内存占用量。

该策略的三个目标：
1. Pause goal : 应用达到预期的 GC 暂停时间
2. Throughput Goal ： 应用达到预期的吞吐量，应用正常运行时间/（正常运行时间 + GC 耗时）
3. Minimum footprint : 尽可能小的内存占用量。

虽然听起来很智能的样子，不过这个策略也不是一定要开启。

可能出现的场景：Eden 和 from 的存活对象 比 to 区要大，无法直接转移到 to 区，所以会转移到老年代。

想要关掉时：
1. 显式设置eden:survivor比例： -XX:SurvivorRatio = 8。
2. 使用 CMS 垃圾回收器（-XX:+useConcMarkSweepGC），默认关闭 AdaptiveSizePolicy。【即使加上显式开启设置，也是不生效的】










