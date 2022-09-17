# mysql 查看锁的方式

## 查询是否锁表

* show OPEN TABLES where In_use > 0;


## 查询进程（如果您有SUPER权限，您可以看到所有线程。否则，您只能看到您自己的线程）

* show processlist

![image](https://user-images.githubusercontent.com/64882640/190844047-8e654eb5-7e45-498c-81c0-8441336a95f5.png)


![image](https://user-images.githubusercontent.com/64882640/190844068-2e048a35-54ae-447f-a103-6dbfe1788055.png)

id 为5的证明一直在等待资源。

## 杀死进程id（就是上面命令的id列）

* kill id 5




## 查看正在锁的事务 

* SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX;

![image](https://user-images.githubusercontent.com/64882640/190844124-eab2a802-c878-490f-bb35-2c4d3aa2e8f2.png)


## 杀死进程id（就是上面命令的trx_mysql_thread_id列）

* kill 线程ID


例子：

查出死锁进程：SHOW PROCESSLIST
杀掉进程          KILL 420821;

## 其它关于查看死锁的命令：

1：查看当前的事务
SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX;

2：查看当前锁定的事务

SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS;

3：查看当前等锁的事务
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS;
