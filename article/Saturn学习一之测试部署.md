# Saturn学习一之测试部署

## 1.快速开始

* Saturn包括两大部分，Saturn Console和Saturn Executor。

* Saturn Console是一个GUI，用于作业/Executor管理，统计报表展现，系统配置等功能。它同时也是整个调度系统的大脑：将作业任务分配到各Executor。

* Saturn Executor是执行任务的Worker：按照作业配置的要求去执行部署于Executor所在容器或物理机当中的作业脚本和代码。

[参考官方文档](https://vipshop.github.io/Saturn/#/zh-cn/3.x/quickstart)

本次测试的是在Docker环境快速启动。 CentOS 8 64位。

* 执行命令

```
$ git clone https://github.com/vipshop/Saturn
$ git checkout develop
$ cd saturn-docker
$ chmod +x quickstart-docker.sh
$ ./quickstart-docker.sh
```

quickstart-docker.sh脚本将做如下事情：

** 构建基于OpenJDK7的基础镜像
** 构建基于OpenJDK7的Saturn-Console镜像
** 构建基于OpenJDK7的Saturn-Executor镜像
** 启动一个ZooKeeper集群的容器
** 启动一个Saturn-Console容器
** 启动两个Saturn-Executor容器
** 添加一个Java作业和一个Shell作业

启动成功后，访问Saturn-Console：http://localhost:9088

![image](https://user-images.githubusercontent.com/64882640/155670250-ef326871-bfa5-4a24-9976-8ab456c28eb3.png)



