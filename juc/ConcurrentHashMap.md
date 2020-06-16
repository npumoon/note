
## put 方法 VS putIfAbsent 方法

```java
public V put(K key, V value) {
    return putVal(key, value, false);
}

public V putIfAbsent(K key, V value) {
    return putVal(key, value, true);
}

```

put时，如果key存在，旧的value会被新的value覆盖掉。

putIfAbsent，如果key存在，返回旧的value。


