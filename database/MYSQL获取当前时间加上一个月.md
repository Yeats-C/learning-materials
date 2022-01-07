# MYSQL 获取当前时间加上一个月

```
update user set leverstart=now(),leverover=date_add(NOW(), interval 1 MONTH) where id=1;
```

* date_add() 增加
* date_sub()减少

* month 月份
* minute 分钟
* second 秒
