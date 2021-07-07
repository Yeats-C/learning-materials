# 运行JAR包 提示没有主清单属性解决办法

## 问题：本地开发调试程序完成，到docker部署的时候，出错，没有主体

![image](https://user-images.githubusercontent.com/64882640/124692074-7d064b00-df0f-11eb-8c9c-dec7a7904d06.png)

首先考虑镜像build异常，重新打包之后还是一样的错误，故决定先本地调试。


* 本地 调试，clean  install package 都没问题
* 本地找到target目录下的jar 打算本地启动调试下

** 发现本地的jar大小只有2MB，启动失败 **
![image](https://user-images.githubusercontent.com/64882640/124692751-b68b8600-df10-11eb-946e-5863c472af23.png)

网上搜索运行JAR包 提示没有主清单属性，问题原因为**JAR包中的META-INF文件夹下的MANIFEST.MF文件缺少定义jar接口类。就是缺少默认运行的Main类。**

打开MANIFEST.MF文件发现确实少了Main-Class

* 文章说可以手动加载，在最后行加入一条信息
```

格式：Main-Class: 包名 类名

本例：Main-Class: org.springframework.boot.loader.JarLauncher

(ps:Main-Class:后面有空格     类名后面不加.class)

```

但，发现少的东西不止这些，故继续研究。

文章说需要在pom.xml里面加入配置
```
<build>
   <plugins>
	<plugin>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-maven-plugin</artifactId>
	</plugin>
    </plugins>
</build>
```

但我记得这个配置我在pom文件里应该是有的，检查pom文件，发现 此依赖被同事删除了
```
<plugin>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-maven-plugin</artifactId>
  <configuration>
    <includeSystemScope>true</includeSystemScope>
    <excludes>
      <exclude>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
      </exclude>
    </excludes>
  </configuration>
</plugin>
```
**加上之后，打包编译成功，docker部署启动成功。**

## 扩展 spring-boot-maven-plugin插件的作用
* spring-boot-maven-plugin插件在打Jar包时会引入依赖包  
* **可以打成直接运行的Jar包** 　maven项目的pom.xml中，添加了org.springframework.boot:spring-boot-maven-plugin
插件，当运行“mvn package”进行打包时，会打包成一个可以直接运行的 JAR 文件，使用“Java -jar”命令就可以直接运行。
* **可以引入依赖包**  一般的maven项目的打包命令，不会把依赖的jar包也打包进去的，只是会放在jar包的同目录下，能够引用就可以了，但是spring-boot-maven-plugin插件，会将依赖的jar包全部打包进去。
* spring-boot-maven-plugin插件，在很大程度上简化了应用的部署，只需要安装了 JRE 就可以运行。
