# MYSQL CASE WHEN多条件同时满足的多个AND组合嵌套的情况

```
SELECT id, time, type, 
CASE when (reason is null or reason = '') and type = '驳回' THEN '未填写驳回理由' 
ELSE reason 
END reason 
from workFlow 
order by time desc; 
```

* CASE WHEN 中是可以在同一个case中加上同一类型的判断条件的，但是需要加上（）。

[参考文章](https://blog.csdn.net/qb170217/article/details/81534399)
