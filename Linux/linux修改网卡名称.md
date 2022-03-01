# linux修改网卡名称

linux修改网卡名称(一般修改为eth0)(redHat7)

## 1.首先将网卡配置文件名称重命名为eth0：

```
cd /etc/sysconfig/network-scripts/

mv ifcfg-eno1677736 ifcfg-eth0
```

## 2.其次编辑修改后的网卡文件

```
vi ifcfg-eth0
```

* 将NAME参数改为与网卡文件相同的名称：NAME=eth0

## 3.接下来禁用网卡命名规则。

* 此功能通过/etc/default/grub文件来控制，要禁用此次功能，在文件中加入”net.ifnames=0 biosdevname=0″即可。

```
vim /etc/default/grub；

GRUB_CMDLINE_LINUX="rd.lvm.lv=rhel/root crashkernel=auto rd.lvm.lv=rhel/swap vconsole.font=latarcyrheb-sun16 vconsole.keymap=us net.ifnames=0 biosdevname=0 rhgb quiet"
```

![image](https://user-images.githubusercontent.com/64882640/156142119-e7e595bd-684e-4cf7-b8f7-89be049b567a.png)

## 4.添加udev网卡规则

* 在/etc/udev/rules.d目录中创建一个网卡规则70-persistent-net.rules文件。

```
vim /etc/udev/rules.d/70-persistent-net.rules
```

* 在文件中写入以下参数

```
SUBSYSTEM==”net”,ACTION==”add”,DRIVERS==”?*”,ATTR{address}==”需要修改名称的网卡MAC地址”,ATTR｛type｝==”1″ ,KERNEL==”eth*”,NAME=”eth0″
```

## 5.命令生成更新grub配置参数

```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

![image](https://user-images.githubusercontent.com/64882640/156144065-c4d5a376-1f60-40da-b8ef-46ecc4318197.png)


## 6.reboot重启系统验证成功
