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

* 构建基于OpenJDK7的基础镜像
* 构建基于OpenJDK7的Saturn-Console镜像
* 构建基于OpenJDK7的Saturn-Executor镜像
* 启动一个ZooKeeper集群的容器
* 启动一个Saturn-Console容器
* 启动两个Saturn-Executor容器
* 添加一个Java作业和一个Shell作业

启动成功后，访问Saturn-Console：http://localhost:9088

![image](https://user-images.githubusercontent.com/64882640/155670250-ef326871-bfa5-4a24-9976-8ab456c28eb3.png)


## 2.测试部署问题
1.Cannot prepare internal mirrorlist: No URLs in mirrorlist --解决方法

* 命令查看yum镜像列表
```
sudo dnf install -y curl policycoreutils openssh-server perl
```
报错 【Cannot prepare internal mirrorlist: No URLs in mirrorlist】
```
问题：
在CentOS 8中，使用yum时出现错误，镜像列表中没有url，类似如下:
Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist

原因
在2022年1月31日，CentOS团队终于从官方镜像中移除CentOS 8的所有包。
CentOS 8已于2021年12月31日寿终正非，但软件包仍在官方镜像上保留了一段时间。现在他们被转移到https://vault.centos.org

解决方法
如果你仍然需要运行CentOS 8，你可以在/etc/yum.repos.d中更新一下源。使用vault.centos.org代替mirror.centos.org。

$ sudo sed -i -e "s|mirrorlist=|#mirrorlist=|g" /etc/yum.repos.d/CentOS-*
$ sudo sed -i -e "s|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g" /etc/yum.repos.d/CentOS-*
```



2.ERROR: Couldn't connect to Docker daemon at http+docker://localunixsocket - is it running?
![image](https://user-images.githubusercontent.com/64882640/155675688-a9bc95a4-32ed-4d8d-a110-244f7fb13dd5.png)

服务器明明安转了docker，却找不到docker服务。

* 查看当前docker服务状态命令

```
systemctl status docker
```

![image](https://user-images.githubusercontent.com/64882640/155676060-5b4e5c9e-65a5-4798-b5f2-4f2564a29743.png)

以上说明docker服务正常启动。


* 使用命令启动docker

```
systemctl start docker
```

一直报【Failed to restart docker.service: Unit docker.service not found.】

* 使用命令 搜索 docker服务
```
yum list installed | grep docker
```

![image](https://user-images.githubusercontent.com/64882640/155676958-bb3a2088-64ab-4dc5-a7d3-5e80d52dd04b.png)

看不懂这个是什么docker版本，反正不是官方包，判断这个版本有问题。

* 命令 删除此服务

```
yum remove podman-docker.noarch -y
```

![image](https://user-images.githubusercontent.com/64882640/155678861-b60da1df-826c-478f-8165-32bc93301937.png)

* 命令 删除docker文件夹（清理历史数据，防止冲突）

```
rm -rf /var/lib/docker
```

* 命令 添加docker仓库源 设置yum源,这里设置为阿里云，如果不设置将会连接国外的站点，可能会安装失败
```
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

![image](https://user-images.githubusercontent.com/64882640/155679730-a0acd7bd-770e-43e5-8523-d5b29c175d33.png)

* 命令 安装docker社区版  (这里安装的是docker-ce(社区版)，而docker-ee是企业版，是收费的)

```
sudo yum install docker-ce
```

* 异常
```
[root@localhost saturn-docker]# sudo yum install docker-ce
Docker CE Stable - x86_64                                                                                    102 kB/s | 3.5 kB     00:00    
错误：
 问题: package docker-ce-3:20.10.12-3.el8.x86_64 requires containerd.io >= 1.4.1, but none of the providers can be installed
  - package containerd.io-1.4.10-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.10-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.11-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.11-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.12-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.12-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.3-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.3-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.3-3.2.el8.x86_64 conflicts with runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.3-3.2.el8.x86_64 obsoletes runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.4-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.4-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.6-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.6-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.8-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.8-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.9-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - package containerd.io-1.4.9-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.2-1.module_el8.5.0+911+f19012f9.x86_64
  - problem with installed package buildah-1.21.4-1.module_el8.4.0+886+c9a8d9ad.x86_64
  - package buildah-1.21.4-1.module_el8.4.0+886+c9a8d9ad.x86_64 requires runc >= 1.0.0-26, but none of the providers can be installed
  - package buildah-1.22.3-2.module_el8.5.0+911+f19012f9.x86_64 requires runc >= 1.0.0-26, but none of the providers can be installed
  - package containerd.io-1.4.10-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.10-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.11-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.11-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.12-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.12-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.3-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.3-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.3-3.2.el8.x86_64 conflicts with runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.3-3.2.el8.x86_64 obsoletes runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.4-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.4-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.6-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.6-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.8-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.8-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.9-3.1.el8.x86_64 conflicts with runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - package containerd.io-1.4.9-3.1.el8.x86_64 obsoletes runc provided by runc-1.0.0-74.rc95.module_el8.4.0+886+c9a8d9ad.x86_64
  - cannot install the best candidate for the job
  - package runc-1.0.0-56.rc5.dev.git2abd837.module_el8.3.0+569+1bada2e4.x86_64 is filtered out by modular filtering
  - package runc-1.0.0-66.rc10.module_el8.5.0+1004+c00a74f5.x86_64 is filtered out by modular filtering
  - package runc-1.0.0-72.rc92.module_el8.5.0+1006+8d0e68a2.x86_64 is filtered out by modular filtering
(尝试在命令行中添加 '--allowerasing' 来替换冲突的软件包 或 '--skip-broken' 来跳过无法安装的软件包 或 '--nobest' 来不只使用软件包的最佳候选)

```

* 判断是冲突了
* 命令 添加 '--allowerasing' 来替换冲突的软件包

```
sudo yum -y install docker-ce --allowerasing
```
![image](https://user-images.githubusercontent.com/64882640/155680436-57e6b0d0-22bd-4ae2-bd02-7bec851a0537.png)
![image](https://user-images.githubusercontent.com/64882640/155680490-355f1d9b-da2a-499f-bce3-bd8468a7bb5f.png)
![image](https://user-images.githubusercontent.com/64882640/155680534-77e54ba8-b7c2-4e39-8ddf-50f9de767c2c.png)

* 上方操作常用命令
```
* 给docker.service文件添加执行权限
chmod +x /etc/systemd/system/docker.service 

* 重新加载配置文件（每次有修改docker.service文件时都要重新加载下）
systemctl daemon-reload         

* 启动
systemctl start docker

* 设置开机启动
systemctl enable docker.service

* 查看docker服务状态
systemctl status docker

* 查看docker版本
docker -v

* 列出并排序您存储库中可用的版本。此示例按版本号（从高到低）对结果进行排序。
 yum list docker-ce --showduplicates | sort -r
```



