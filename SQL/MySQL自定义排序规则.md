# MySQL自定义排序规则

* 有三个函数（order by field，ORDER BY INSTR，ORDER BY locate）

## FIELD函数

* 格式：field(value,str1,str2,str3,str4)

value与str1、str2、str3、str4比较，返回1、2、3、4，如遇到null或者不在列表中的数据则返回0.

SELECT * FROM user order by field(id,2,3,5,4) desc

结果显示顺序：4 5 3 2 1 6

id uname passwd

4 dd dd
5 ee ee
3 cc cc
2 bb bb
1 aa aa
6 ff ff


## INSTR(字段名, 字符串)

这个函数返回字符串在某一个字段的内容中的位置, 没有找到字符串返回0，否则返回位置（从1开始）.

SELECT * FROM user ORDER BY INSTR( ’2,3,5,4′, id ) ASC

结果显示顺序：1 6 2 3 5 4

1 aa aa
6 ff ff
2 bb bb
3 cc cc
5 ee ee
4 dd dd

## locate

SELECT * FROM user ORDER BY locate( id, ’2,3,5,4′ ) ASC

结果显示顺序：1 6 2 3 5 4

1 aa aa
6 ff ff
2 bb bb
3 cc cc
5 ee ee
4 dd dd

 

SELECT * FROM user ORDER BY locate( id, ’2,3,5,4′ ) DESC

结果显示顺序：4 5  3 2 1 6

4 dd dd
5 ee ee
3 cc cc
2 bb bb
1 aa aa
6 ff ff


如我想要查找的数据库中的ID顺序首先是（2,3,5,4）然后在是其它的ID顺序，你首先要把他降序排即（4 5 3 2），然后在SELECT * FROM user order by field(id,4,5,3,2) DESC 或 SELECT * FROM user ORDER BY INSTR( ’4,5,3,2′, id ) DESC  或用 SELECT * FROM user ORDER BY locate( id, ’4,5,3,2′ ) DESC 就得到你想要的结果了。






原文链接：https://blog.csdn.net/angellee1988/article/details/82712790
