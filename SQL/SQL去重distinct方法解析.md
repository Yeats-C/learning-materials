SQL去重distinct方法解析

一 distinct

含义：distinct用来查询不重复记录的条数,即distinct来返回不重复字段的条数（count(distinct id)）,其原因是distinct只能返回他的目标字段，而无法返回其他字段

用法注意：

1.distinct 【查询字段】，必须放在要查询字段的开头，即放在第一个参数；

2.只能在SELECT 语句中使用，不能在 INSERT, DELETE, UPDATE 中使用；

3.DISTINCT 表示对后面的所有参数的拼接取 不重复的记录，即查出的参数拼接每行记录都是唯一的

4.不能与all同时使用，默认情况下，查询时返回的就是所有的结果。

1.1只对一个字段查重

对一个字段查重，表示选取该字段一列不重复的数据

1.2多个字段去重

对多个字段去重，表示选取多个字段拼接的一条记录，不重复的所有记录

 期望结果：只对第一个参数PLAN_NUMBER取唯一值

解决办法一： 使用 group_concat 函数

语句：SELECT GROUP_CONCAT(DISTINCT PLAN_NUMBER) AS PLAN_NUMBER,PRODUCT_NAME FROM psur_list GROUP BY PLAN_NUMBER

解决办法二：使用group by

语句：SELECT PLAN_NUMBER,PRODUCT_NAME FROM psur_list GROUP BY PLAN_NUMBER

1.3针对null处理

distinct不会过滤掉null值，返回结果包含null值

1.4与distinctrow同义

语句：SELECT  DISTINCTROW COUNTRY FROM psur_list 

 二 聚合函数中使用distinct

在聚合函数中DISTINCT 一般跟 COUNT 结合使用。count（）会过滤掉null项

语句：SELECT COUNT(DISTINCT COUNTRY) FROM psur_list
