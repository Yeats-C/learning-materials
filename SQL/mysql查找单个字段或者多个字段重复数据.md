# mysql查找单个字段或者多个字段重复数据

## 一、单个字段重复

* 第一步

要找出重复数据，我们首先想到的就是，既然是重复，那么 数量就是大于 1  就算是重复。 那就是 count 函数 。

因为我们要排查的是 单个 字段account ，那么就是需要按照 account 字段 维度 去分组。  那就是 group by 函数。

```
 SELECT account ,COUNT(account) as count FROM accountinfo GROUP BY account;
```

* 第二步 

没错，如我们所想，count大于1的即是 account为 A  和 B 的数据。

那么我们稍作筛选，只把count大于1的数据的account  找出来。
利用having 拼接筛选条件，写出来的mysql 语句是：

```
 SELECT account FROM accountinfo GROUP BY account HAVING COUNT(account) > 1;
```

* 第三步

重复的account数据 A B 都找出来了，接下来我们只需要把account为A 和 B 的其他数据都一起查询出来。

那就是利用第二步查出来的数据做为子查询条件，使用 IN 函数。

```
 SELECT * FROM  accountinfo WHERE account IN
 (
 SELECT account FROM accountinfo GROUP BY account HAVING COUNT(account) > 1
 );
```

## 场景二  多个字段重复数据查找

* 第一步

因为有了文章上半部讲到的单个字段重复的数据查找思路，所以到这边应该更好理解了。

同样， account 和 deviceId 都相同的重复数据就是指， 这种数据存在的数量 大于 2，那么就是存在重复了。

我们还是使用到了 group by  函数 和 count 函数 和 having and  函数（因为需要同时满足两个字段条件，使用and）。

```
SELECT account, COUNT(account), deviceId, COUNT(deviceId) 
FROM accountinfo 
GROUP BY account, deviceId 
HAVING  (COUNT(account) > 1) AND  (COUNT(deviceId) > 1) 
```

* 第二步

一样 也是把第一步里的到的关键信息 account 和 deviceId做为子查询条件，从原表里把  account 和 deviceId 同时相同的数据都查找出来。

```
SELECT t.* FROM  accountinfo t, (
 
SELECT account, COUNT(account), deviceId, COUNT(deviceId) 
FROM accountinfo 
GROUP BY account, deviceId 
HAVING  (COUNT(account) > 1) AND  (COUNT(deviceId) > 1) 
) a 
 
WHERE t.account=a.account AND t.deviceId=a.deviceId 
```

[参考文章](https://blog.csdn.net/qq_35387940/article/details/108074927)

