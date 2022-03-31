# This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its de 错误解决办法

## 前因

在数据库创建查询省市区及下级所有区域编码函数

```
DROP FUNCTION IF EXISTS queryChildrenAreaInfo;

CREATE FUNCTION queryChildrenAreaInfo(areaCode VARCHAR(64))

RETURNS VARCHAR(4000)

BEGIN

DECLARE sTemp VARCHAR(4000);

DECLARE sTempChd VARCHAR(4000);

SET sTemp='$';

SET sTempChd = CAST(areaCode AS CHAR);

WHILE sTempChd IS NOT NULL DO

SET sTemp= CONCAT(sTemp,',',sTempChd);

SELECT GROUP_CONCAT(city_code) INTO sTempChd FROM e101_province_dictionary WHERE FIND_IN_SET(pid,sTempChd)>0 and level!=4;

END WHILE;

RETURN sTemp;

END;
```

出现以下错误：

![image](https://user-images.githubusercontent.com/64882640/160971989-f45ba665-2312-4d43-9390-3a1b09d6de72.png)

## 分析

这是我们开启了bin-log, 我们就必须指定我们的函数是否是
* 1 DETERMINISTIC 确定性的
* 2 NO SQL 没有SQl语句，当然也不会修改数据
* 3 READS SQL DATA 只是读取数据，当然也不会修改数据
* 4 MODIFIES SQL DATA 要修改数据
* 5 CONTAINS SQL 包含了SQL语句

* 其中在function里面，只有 DETERMINISTIC, NO SQL 和 READS SQL DATA 被支持。如果我们开启了 bin-log, 我们就必须为我们的function指定一个参数。

## 解决

在MySQL中创建函数时出现这种错误的解决方法：

```
set global log_bin_trust_function_creators=TRUE;
```
