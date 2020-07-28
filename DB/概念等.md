### join

inner join
left join
right join
full join


inner join
保留两张表中完全匹配的的结果集

left join
左表所有行，即使右表没有

right join
右表所有行，即使左表没有

full join
返回所有行，即使左右表没有匹配

https://www.cnblogs.com/yanglang/p/8780722.html

补上图

这么理解吧

每个表的记录都可以看做是多个副本的，inner join 是匹配的留下，形成新的记录，没有匹配上的都扔掉.



## 名词schema

1. Schema 在不同的 DBMS 中有不同的含义

mysql 中跟 database 没两样

SQL Server有点不一样
Oracle 有点不一样

