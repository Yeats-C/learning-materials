# Mysql常用函数

## 从mysql字段中删除第一个字符

```
SUBSTR('字段名称',2)

SELECT SUBSTR(queryChildrenAreaInfo(150000000000),3) as areaCodes;
```

##  删除MySQL中字符串的第一个字符以外的所有字符

```
left('字段名称',1)
```

## 获取字符串长度

* MySQL LENGTH(str) 函数的返回值为字符串的字节长度，使用 uft8（UNICODE 的一种变长字符编码，又称万国码）编码字符集时，一个汉字是 3 个字节，一个数字或字母是一个字节。

```
mysql> SELECT LENGTH('name'),LENGTH('数据库');
+----------------+---------------------+
|LENGTH('name')  | LENGTH('数据库')    |
+----------------+---------------------+
|              4 |                   9 |
+----------------+---------------------+
1 row in set (0.04 sec)
```
