## 解决docker部署python项目pip安装慢或timeout问题

# 1. 编辑Dockerfile文件, 修改部分如下：挂载清华镜像
```
RUN pip install  -i https://pypi.tuna.tsinghua.edu.cn/simple/  -r requirements.txt
```

# 2. 完整Dockerfile
```
FROM python:3.6
ENV PATH /usr/local/bin:$PATH
ADD . /code
WORKDIR /code
RUN pip install  -i https://pypi.tuna.tsinghua.edu.cn/simple/  -r requirements.txt
CMD python /code/main.py
```

# 3.注：pip国内的一些镜像
```
阿里云 http://mirrors.aliyun.com/pypi/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
豆瓣(douban) http://pypi.douban.com/simple/
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
```
