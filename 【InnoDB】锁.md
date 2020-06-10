### 锁

X 锁
S 锁

锁的算法实现
1. Gap Key
2. Next-Key
3. 

### 非一致性锁定读
 
> 非锁定：不需要等待访问的行上X锁（排他锁）的释放
 
 **MVCC** （multi version concurrency control）
 
 利用读取快照的方式，解决行记录被锁住时的其他线程读取行为。
 
 快照有多个版本
 > `Read-Committed` 隔离级别下，对于快照数据，非一致性读总是读取被锁定行的最新一份快照信息。
 > 而 `Repeatable-Read` 隔离级别下，对于快照数据，非一致性读总是读取事务开始时的行数据版本。

### 一致性的锁定读

1.  select ... for update 

    加排他锁 （X锁）

2.  select ... LOCK IN SHARE MODE

    加共享锁（S锁）
    

 
 ---
 ***补充***
 
 快照数据是指该行的之前版本的数据，这部分数据是通过 **undo段**来完成，undo 用来在事务中回滚数据，因此快照数据本身**没有额外的开销**。
    
    
 
 
 
