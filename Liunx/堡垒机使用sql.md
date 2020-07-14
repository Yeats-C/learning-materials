在liunx上用堡垒机直接使用sql，找运维开权限以后。
使用  mysql -u -p -h 访问数据库
-u:用户名
-p:密码
-h:服务器地址

例如：
mysql -u store_write -p8pZoWnOnLguvLmmQT -hrm-2zeela0dj4k34987s.mysql.rds.aliyuncs.com  

然后：查看库
show databases;

连接库：
use 数据库名;

查看表：
show tables;




