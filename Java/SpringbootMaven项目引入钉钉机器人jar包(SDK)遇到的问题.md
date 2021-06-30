# Springboot Maven项目引入钉钉机器人jar包(SDK)遇到的问题

## 下载的jar包放到了 resource/lib下，在pom.xml中添加如下配置

![image](https://user-images.githubusercontent.com/64882640/123891082-f4c1fc00-d98a-11eb-8e32-f1c2b1d7a364.png)

这个也很好理解，需要在编译的时候让maven加载到正确的依赖，然后编译通过了，打包成功,本地调试也调通了，钉钉信息能直接发送到群。

## 但是，jar包在服务器部署的时候遇到了以下错误
```
Failed to introspect bean class [com.xxx] for lookup method metadata:
could not find class that it depends on; 
nested exception is java.lang.NoClassDefFoundError: com/dingtalk/api/DingTalkClient

```
首先考虑是不是引入jar方法有冲突，有重复方法名，调试后发现不是。

## 最后，在pom文件加入以下配置
```
<plugins>
     <plugin>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-maven-plugin</artifactId>
         // ++++++++ 添加这部分配置
         <configuration>
             <includeSystemScope>true</includeSystemScope>
         </configuration>
         // ++++++++ 添加这部分配置
     </plugin>
 </plugins>

```
includeSystemScope 直接翻一下—包含系统局部包,设置为 true。

代表maven打包时会将外部引入的jar包（比如在根目录下或resource文件下新加外部jar包）打包到项目jar，在服务器上项目才能运行，不加此配置，本地可以运行，因为本地可以再lib下找到外部包，但是服务器上jar中是没有的。

终于搞定了，线上正常服务调用。

## 延伸，也可用以下配置
```
<excludes>
    <exclude>lib/*.jar</exclude>
</excludes>
```
