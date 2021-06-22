# MySQL查询按照指定规则排序
* 1.按照指定(单个)字段排序
```
  select * from table_name order id desc;
```
* 2.按照指定(多个)字段排序
```
  select * from table_name order id desc,status desc;
```
* 3.按照指定字段和规则排序
```
  select * from table_name order by field(status,5,1,2,3,4);
```
* 4.字符串排序 将字符串列参与计算自动转为int型
```
  select * from table_name order by (varchar_column+0);
```
