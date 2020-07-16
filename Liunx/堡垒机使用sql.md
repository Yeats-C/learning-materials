## 怎么在堡垒机上使用sql直接操作数据库

在liunx上用堡垒机直接使用sql，找运维开权限以后。
使用  mysql -u -p -h 访问数据库
-u:用户名
-p:密码
-h:服务器地址

例如：
``` sql
mysql -u store_write -p8pZoWnOnLguvLmmQT -hrm-2zeela0dj4k34987s.mysql.rds.aliyuncs.com  
```
然后：查看库
``` sql
show databases;
```

连接库：
``` sql
use 数据库名;
```

查看表：
``` sql
show tables;
```

最后，就可以敲命令了。


