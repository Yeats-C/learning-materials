# learning-materials
This is a place where I will organize my study materials.
It will store some of my daily study documents.
I hope I can keep going.


## 一： 编程基础
不管是C还是C++，不管是Java还是PHP，想成为一名合格的程序员，基本的数据结构和算法基础还是要有的。下面几篇文章从思想到实现，为你梳理出常用的数据结构和经典算法。

1-1 常用数据结构

[数组、链表、堆、栈、队列、Hash表、二叉树等](https://www.cnblogs.com/xdecode/p/9321848.html)

1-2 算法思想

算法时间复杂度和空间复杂度的分析计算

算法思想：递推、递归、穷举、贪心、分治、动态规划、迭代、分枝界限

1-3 经典算法

经典排序：[插入排序、冒泡排序、快排（分划交换排序）、直接选择排序、堆排序、合并排序](/Java/十大经典排序算法总结.md)

经典查找：顺序查找、二分查找、二叉排序树查找

1-4 高级数据结构

[B+/B-数、红黑树、图等](https://www.jianshu.com/p/b38300fe53b4)

1-5 高级算法

图的深度优先搜索、图的广度优先搜索、拓扑排序、Dijkstra算法（单源最短路径）、霍夫曼编码、辗转相除法、最小生成树等

## 二：Java语言基础
诞生不过二十余年的Java语言凭借其跨平台、面向对象、适合于分布式计算的特性，广泛应用于Web网站、移动设备、桌面应用中，并且已经连续多年稳居TOBIE编程语言排行榜前列，最近更是登上冠军宝座。Java有哪些优秀而又与众不同的地方首先一定要清楚。

2-1 基础语法

Java语法格式，常量和变量，变量的作用域，方法和方法的重载，运算符，程序流程控制，各种基本数据类型及包装类

[Java中List\<String\>与String字符串互转](/Java/java中ListString与String字符串互转.md)

[Object与json字符串的相互转换](/Java/Object与json字符串的相互转换.md)

2-2 重要：集合类

Collection以及各种List、Set、Queue、Map的实现以及集成关系，实现原理

Collections和Arrays

[Java遍历map中value值](/Java/Java遍历map中value值.md)

2-3 其他JavaAPI

[String和StringBuffer](/Java/StringBufferStringBuilderString.md)，System和Runtime类，Date和DateFomat类

java.lang包

java.util包（集合类体系、规则表达式、zip，以及时间、随机数、属性、资源和Timer等）

java.math包

java.net包

java.text包（各种格式化类等）

java.security包

2-4 面向对象、面向接口

对象的三大特性：封装、继承和多态，优缺点

如何设计类，类的设计原则

this关键字，[final关键字](/Java/final,finally,finalize.md)，[static关键字](/Java/static分析.md)

对象的实例化过程

方法的重写和重载；方法和方法的参数传递过程

构造函数

内部类，抽象类，接口

对象的多态性（子类和父类之间的转换、父类纸箱子类的引用），抽象类和接口在多态中的应用

2-5 JVM内存模型、[垃圾回收](/Java/GC学习.md)

2-6 关于异常

Throwable/Error/Exception，Checked Exception vs. Unchecked Exception，异常的捕捉和抛出，异常捕捉的原则，finally的使用

2-7 多线程

[线程和进程的概念,如何在程序中创建多线程](/java/进程与线程的概念.md)，线程安全问题，线程之间的通讯

线程的同步

死锁问题的剖析

线程池

2-8 IO

java.io包，理解IO体系的基于管道模型的设计思路以及常用IO类的特性和使用场合。

File及相关类，字节流InputStream和OutputStream，字符流Reader和Writer，以及相应缓冲流和管道流，字节和字符的转化流，包装流，以及常用包装类使用

分析IO性能

2-9 XML

熟悉SAX、DOM以及JDOM的优缺点并且能够使用其中的一种完成XML的解析及内容处理；这几种解析方法的原理

2-10 一些高级特性

反射、代理、泛型、枚举、Java正则表达式

2-11 网络编程

[网络通信协议原理及适用场景](https://www.cnblogs.com/fuguy/p/13089548.html#_lab2_1_0)，Socket编程，WEB服务器的工作原理

2-11 JDK1.5、JDK1.6、JDK1.7、JDK1.8每个版本都比前面一个版本添加了哪些新特性，进行了哪些提升

## 三：数据库相关
前面说到了数据结构，数据库简单来说就像是电子化的档案柜，是按照一定的数据结构来组织、存储和管理数据的仓库。

3-1理论基础

数据库设计原则和范式

事务（ACID、工作原理、事务的隔离级别、锁、事务的传播机制）

3-2 各种数据库优缺点、使用场景分析

MySQL/SQLServer/Oracle以及各种NoSQL(Redis、MongoDB、Memcached、Hbase、CouchDB等)

3-2 SQL语句

数据库创建，权限分配，表的创建，增删改查，连接，子查询

触发器、存储过程、事务控制

3-3 优化

[索引原理及适用](/Java/索引.md)，大表查询优化，多表连接查询优化，子查询优化等

3-4 分库、分表、备份、迁移

导入、导出，分库、分表，冷备热备，主从备份、双机热备、纵向扩展、横向扩展

3-5 JDBC

JDBC Connection、Statement、PreparedStatement、CallableStatement、ResultSet等不同类的使用

连接池（配置使用、实现原理）

ORM，DAO

[数据库常用语法](/database/mysql%E5%B8%B8%E7%94%A8%E8%AF%AD%E6%B3%95.md)

[MySql异常之group_concat字符长度限制](/SQL/MySql中group_concat字符长度限制.md)

[mysql指定排序规则](/SQL/mysql指定排序规则.md)

[sql判断字符串是否包含某个字符串](/SQL/sql判断字符串是否包含某个字符串.md)

## 四：JavaWeb核心技术（包括部分前端）
HTML5/Css/js原生/jQuery

Ajax（跨域等）

JSP/JavaBean/Servlet/EL/JSTL/TabLib

JSF

JSON

EJB

序列化和反序列化

规则引擎

搜索引擎

模板引擎

缓存

身份认证

测试

集群

持久化

生成静态页技术

高性能

安全

事务JTA

[Java定时任务配置（Scheduled注解各参数详解）](/Java/Java定时任务配置（Scheduled注解各参数详解）.md)

其他需要了解的，如：管理JMX、安全JCCA/JAAS、集成JCA、通信JNDI/JMS/JavaMain/JAF、SSI技术

## 五、主流框架及工具
Struts1/Struts2

spring（IoC、AOP等），SpringMVC

持久化：hibernate/MyBatis

日志：Log4j

单元测试：JUnit

消息队列：ActiveMQ、RabbitMQ等

负载均衡：Nginx/HaProxy

Web服务器：Tomcat、JBoss、Jetty、Resin、WebLogic、WebSphere等

通信：WebService(cxf的soap、restful协议)

缓存：redis、Memcached

工作流：Activity、JBPM

搜索引擎：lucene，基于lucene封装的solr

模板引擎：Velocity、FreeMaker

大数据：Hadoop（HDFS和MapReduce）

构建工具：Ant/Maven

文件管理：[华为obs文件上传](/Java/华为OBS文件上传.md)

运维：[docker运维](/docker/docker使用.md)

[python部署docker遇到的问题](/python/python遇到的问题.md)

## 六、JavaWeb系统设计与架构
Java设计模式

JAVA与UML建模

面向服务架构：SOA/SCA/ESB/OSGI/EAI，微服务

面向资源架构：ROA/REST

面向云架构：COA/Saas/云计算

大型网站负载均衡、系统调优等

分布式id生成算法:[雪花算法](/Java/雪花算法.md)

[springboot运行或打包时动态指定配置文件application\-\*\.yml](/Java/springboot运行或打包时动态指定配置文件application.md)

## 七、More
排错能力：

应该可以根据异常信息比较快速的定位问题的原因和大致位置

优化能力

代码规范、代码管理：

有自己的代码规范体系，代码可读性好

知识面广：

懂各种网络产品及特性，懂各种中间件，能够知道坑在哪儿，深谙各种技术方案的优缺点，懂整合各种资源并达到最优....了解各种技术及应用场景，有足够的工作经验解决集成中遇到的各种奇葩问题

[基于PostgreSQL和PostGIS的坐标转换函数，支持点、线、面的WGS84和CGCS2000与GCJ02和BD09坐标系与之间互转。](/SQL/PostGIS%E5%9D%90%E6%A0%87%E8%BD%AC%E6%8D%A2%E5%87%BD%E6%95%B0.md)

[JAVA操作EXCEL](/Java/JAVA操作EXCEL.md)

[Windows系统查询指定端口好的java进程并杀掉](/Liunx/Windows系统查询指定端口好的java进程并杀掉.md)

[java中推送消息到钉钉群](/Java/java中推送消息到钉钉群.md)

[java数据库动态配置定时任务](/Java/java数据库动态配置定时任务.md)

技术管理/技术总监：

产品管理、项目管理、团队建设、团队提升

CTO：

发展战略

## 八、日常问题

[postman测试传入ListString参数](/Java/postman测试传入ListString参数.md)

[SpringbootMaven项目引入钉钉机器人jar包(SDK)遇到的问题](/Java/SpringbootMaven项目引入钉钉机器人jar包(SDK)遇到的问题.md)

[运行JAR包 提示没有主清单属性解决办法](/Java/运行JAR包 提示没有主清单属性解决办法.md)
