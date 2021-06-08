# MySql中group_concat字符长度限制
事件：mysql的group_concat默认连接长度为1024字符，超过长度的都会截取掉（舍弃）。

解决办法：
##  临时：
```
通过以下命令查询长度
SELECT @@global.group_concat_max_len;
show variables like "group_concat_max_len";

使用以下语句设置长度：
SET GLOBAL group_concat_max_len=102400;
SET SESSION group_concat_max_len=102400; 
```
需注意这只是临时修改长度值，mysql重启后会重新默认赋值1024.

## 永久
```
在MySQL配置文件中my.conf或my.ini中添加:
#[mysqld]
group_concat_max_len=102400
```
重启mysql服务
