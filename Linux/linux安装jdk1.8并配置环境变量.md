# linux安装jdk1.8并配置环境变量

linux安装jdk跟windows相差不大，都是先下载压缩包解压，配置环境变量等步骤

## 1.切换root用户（ 如果当前登录的用户权限够的话，请忽略这步）

## 2.创建安装jdk的文件目录

```
mkdir -p java/jdk
```

![image](https://user-images.githubusercontent.com/64882640/156125301-ba9135fd-ba81-429e-8d07-71328848f234.png)

## 3.切换到jdk文件夹

```
cd java/jdk/
```

![image](https://user-images.githubusercontent.com/64882640/156125456-200091da-9a7e-4f61-be45-6cf6dcecf1f1.png)

## 4.下载jdk压缩包（本文下载的是tar.gz的文件，不需要安装）

```
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie"  http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.tar.gz
```

* 注意：如果wget命令不能用,报错：-bash: wget: command not found。执行一下该命令(安装依赖包) yum -y install wget

 下载命令运行后，等待下载完成
 
 ![image](https://user-images.githubusercontent.com/64882640/156125705-d83acb26-47e3-4fd3-993c-4b46150cd5f3.png)

## 5.解压

```
tar -zxvf jdk-8u131-linux-x64.tar.gz
```

## 6.重命名一下

解压完成后，会生成jdk1.8.0.131文件夹，输入命令：mv  jdk1.8.0.131  jdk1.8 重命名一下，不改名称也行，看个人习惯

```

```
