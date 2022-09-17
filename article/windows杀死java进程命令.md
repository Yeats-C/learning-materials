# windows 杀死java进程 命令

## windows  系统查询8080端口的进程

* netstat -ano | findstr 8080

可以使用命令行：

先使用tasklist  或者JPS 命令查看当前系统中的进程列表，然后针对你要杀的进程使用taskkill命令 

![image](https://user-images.githubusercontent.com/64882640/190843799-0c27d0c7-1f99-43dc-bdb0-7188be5ddadd.png)


如要杀nginx.exe进程，命令如下：
taskkill /im nginx.exe /f


也可以使用pid结束进程：

taskkill /pid {pid}

您可以运行taskkill /?来获取更多更多有关taskkill的信息。

taskkill /pid  8568 /F

![image](https://user-images.githubusercontent.com/64882640/190843830-793f0a3d-daaa-4fc3-b524-1a7d45c08775.png)

