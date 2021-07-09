## 解决docker部署python项目pip安装慢或timeout问题

# 1. 编辑Dockerfile文件, 修改部分如下：
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
