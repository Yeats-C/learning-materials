# pgsql 比较数字字符串_postgreSQL 数字与字符串类型转换操作

### 1. 数字转字符串
```
select cast(123 as VARCHAR);
```

### 2. 字符串转数字
```
select cast('123' as INTEGER);
```

### 3. pgSql, mySql中字符串转化为数字
```
语法 to_number(text, text)

例如：select b from BBS b where b.isDeleted=false order by to_number(trim(both 'ibs' from b.className), '999999')
```
