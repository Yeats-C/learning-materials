项目在linux环境打包前需将Dockerfile放入工程

一、docker流程
1、安装docker：yum -y install docker；查看版本：docker -v；更新docker版本：yum update -y
2、启动docker：systemctl start docker；
3、查看docker：docker；可以查看命令
4、下载GIT代码：git clone 仓库https地址，回车输入用户名，密码；
5、JAVA打包：mvn clean package -DskipTests=true，注意打包之前进入代码目录 ，目录：src\gas-industry-collect-service，如代码目录为gas-industry-collect-service，先进入cd src，然后cd gas-industry-collect-service；注意：cd 目录名，返回上级：cd ..
6、构建镜像：docker build -t pro/gas-industry-collect-service:v1.0 .；格式：docker build -t 组织/服务:v版本号，最后.代表本次执行的上下文路径

二、docker启动镜像
1、镜像列表：docker images

REPOSITORY: 来自于哪个仓库；
TAG: 镜像的标签信息，比如 5.7、latest 表示不同的版本信息；
IMAGE ID: 镜像的 ID, 如果您看到两个 ID 完全相同，那么实际上，它们指向的是同一个镜像，只是标签名称不同罢了；
CREATED: 镜像最后的更新时间；
SIZE: 镜像的大小，优秀的镜像一般体积都比较小，这也是我更倾向于使用轻量级的 alpine 版本的原因；
2、查看所有容器命令及端口映射：docker ps -a
3、网络端口映射：
docker run -d -p 10401:10401 pro/gas-industry-collect-service:v1.0 /bin/bash
      注意：P有大小写之分，-P :是容器内部端口随机映射到主机的端口。-p：是容器内部端口绑定到指定的主机端口。
docker run -d -p 127.0.0.1:1401:1401
4、启动一个已停止的容器：docker start 镜像ID
5、进入运行容器中：docker exec -it 容器ID /bin/sh
6、查看日志：docker log 容器ID
7、查看镜像详细信息：docker inspect 镜像名
docker inspect pro/gas-industry-collect-service:v1.0
8、查看镜像历史：docker history 镜像名，说明：可以列出各个层（layer）的创建信息
docker history pro/gas-industry-collect-service:v1.0
9、docker删除文件：docker rmi 文件名
10、推送：docker push IP地址:端口、
11、ctrl+c 退出容器并关闭容器，ctrl+p+q 退出不关闭容器

三、选项说明：
1、将 tail -f /dev/null 添加到命令中，将 tail -f /dev/null 添加到命令中
docker run -d centos tail -f /dev/null

Options
Mean

-a stdin
指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；

-d
后台运行容器，并返回容器ID；

-i
以交互模式运行容器，通常与 -t 同时使用；

-P
随机端口映射，容器内部端口随机映射到主机的高端口

-p
指定端口映射，格式为：主机(宿主)端口:容器端口

-t
为容器重新分配一个伪输入终端，通常与 -i 同时使用；

–name=“nginx-lb”
为容器指定一个名称；

–dns 8.8.8.8
指定容器使用的DNS服务器，默认和宿主一致；

–dns-search example.com
指定容器DNS搜索域名，默认和宿主一致；

-h “mars”
指定容器的hostname；

-e username=“ritchie”
设置环境变量；

–env-file=[]
从指定文件读入环境变量；

–cpuset=“0-2” or --cpuset=“0,1,2”
绑定容器到指定CPU运行；

-m
设置容器使用内存最大值；

–net=“bridge”
指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；

–link=[]
添加链接到另一个容器；

–expose=[]
开放一个端口或一组端口；

–volume , -v
绑定一个卷


2、查看日志：容器的 metadata 在 /var/lib/docker/containers/containerId/ 目录下，其中 containerId-json.log 文件中记录了回写的内容。
3、限制内存运行：docker run -it --rm -m 200M centos


8、映射端口、映射目录、启动等

四、打包过程中缺少包
1、安装GIT：yum install -y git；
2、显示当前目录下list：ll
3、安装maven（版本太低）：yum install -y maven
3.1、重新安装maven3.5.2版本：yum -y install apache-maven
3.2、卸载maven：yum remove maven
3.3、升级maven版本：yum upgrade apache-maven
4、安装mvn：mvn
4.1、查看版本：mvn -version
5、查看安装内容：yum list installed
6、清理文件：yum clean all
7、删除文件：rpm -e **    --nodeps，用这个命令 把** 替换成包名

删除镜像：docker rm -f ID名




