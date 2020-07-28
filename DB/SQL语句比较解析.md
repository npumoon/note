
## 使用Limit时需要注意

>语句 1
```SQL

```

>语句 2
```SQL
SELECT r1.id, from_node_id, area_id, batch_id, invite_type,be_invited, inviter, STATUS, created, modified, COMMENT FROM resource_assign_invite r1 
inner join (select id from resource_assign_invite where batch_id = 153452239 AND STATUS = 0ORDER BY ID desc limit 0,1 ) 
as r2 on r1.id=r2.id;
```

耗时比较
![示意图]耗时比较.jpg
