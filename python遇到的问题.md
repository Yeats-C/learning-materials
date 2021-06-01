# python遇到的问题

* 前言，本文前提条件：python项目部署docker，dockerfile文件
```
FROM python:3.6
ENV PATH /usr/local/bin:$PATH
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
CMD python main.py
```
## python在docker上
1.python项目在docker打包镜像的时候，打出来的镜像特别大，每一个镜像都有至少1.2~1.3G，这个能想办法减少一些内存占用吗？

2.python镜像启动服务的时候，是不能用run -d -p 守护进程启动是吗？每次这个命令启动程序都是0秒马上关闭状态？

3.我目前启动python镜像都是用的run -it -p 启动，但是启动之后，python好像不会和java一样 ，自动找到main函数启动服务，这个是因为dockerFile文件的CMD命令格式有问题吗？
