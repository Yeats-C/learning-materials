# Windows系统查询指定端口好的java进程并杀掉
端口号：10003
查询：
```
netstat -ano |findstr 10003
```
![image](https://user-images.githubusercontent.com/64882640/122852435-2f9cb200-d343-11eb-971b-05ebec6ec8a3.png)

杀死进程：
```
taskkill /f /pid 10003
```
