# linux常用命令

## cd 进入文件夹  例如：cd src


## ll 查看当前目录下所有文件 

## vi 修改文件 vi 文件名
步骤：

* 1、执行 vi world.txt  进入编辑器（默认命令模式），

* 2、点击a或i进入编辑模式，敲入内容：hello linux world !

* 3、然后按键盘上的esc键退出编辑模式（进入到命令模式），

* 4、最后敲冒号：，

* 5、再敲wq保存并退出。

-------
wq解释为：write quite
不想保存，q
强制退出 q!

## rz 选择文件进行上传

## sz 文件名  

* sz后面跟文件名可以进行文件从linux上面下载

## linux移动文件夹到另一个文件夹

```
mv /root/user/p05-fu /root/user/nia/p05-fu      移动p05-fu文件夹到nia文件夹下
mv /root/user/p05-fu /root/user/p04-fu          重命名也可以这样写，将p05重命名为p04
```
