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

来自：[SQL查询重复记录](https://www.cnblogs.com/1917q/p/14024972.html)
