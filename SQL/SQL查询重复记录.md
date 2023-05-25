# SQL查询重复记录

### 1、查找表中多余的重复记录，重复记录是根据单个字段（peopleId）来判断
```
select * from people
where peopleId in (select peopleId from people group by peopleId having count(peopleId) > 1)
```

### 2、删除表中多余的重复记录，重复记录是根据单个字段（peopleId）来判断，只留有rowid最小的记录
```
delete from people 
where peopleId  in (select peopleId  from people group by peopleId having  count(peopleId) > 1)
and rowid not in (select min(rowid) from   people group by peopleId  having count(peopleId )>1)
```

### 3、查找表中多余的重复记录（多个字段） 
```
select * from vitae a
where (a.peopleId,a.seq) in (select peopleId,seq from vitae group by peopleId,seq having count(*) > 1)
```

### 4、删除表中多余的重复记录（多个字段），只留有rowid最小的记录
``` 
delete from vitae a
where (a.peopleId,a.seq) in   (select peopleId,seq from vitae group by peopleId,seqhaving count(*) > 1)
and rowid not in (select min(rowid) from vitae group by peopleId,seq havingcount(*)>1)
```


### 5、查找表中多余的重复记录（多个字段），不包含rowid最小的记录
```
select * from vitae a
where (a.peopleId,a.seq) in   (select peopleId,seq from vitae group by peopleId,seqhaving count(*) > 1)
and rowid not in (select min(rowid) from vitae group by peopleId,seq havingcount(*)>1)
```

### 6、查询重复记录需求实现思路
* 1.创建临时表，承载重复记录
* 2.建立定时任务，凌晨时分触发函数
* 3.函数中，执行以下操作
* 清空临时表
* 将重复记录查询insert到临时表
* 记录更新数据日志
* 4.提供接口直接查询临时表
* 逻辑：mysql执行定时任务，每日进行重复数据查询，插入到临时表中，按照分组、分页查询数据

参考文章：[SQL查询重复记录](https://www.cnblogs.com/1917q/p/14024972.html)
