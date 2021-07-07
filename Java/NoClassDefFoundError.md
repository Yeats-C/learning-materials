#  真正解决方案：java.lang.NoClassDefFoundError: javax/xml/bind/DatatypeConverter

今天在启动若依框架时，遇到此异常。

## 问题原因
```
JAXB API是java EE 的API，因此在java SE 9.0 中不再包含这个 Jar 包。
java 9 中引入了模块的概念，默认情况下，Java SE中将不再包含java EE 的Jar包
而在 java 6/7 / 8 时关于这个API 都是捆绑在一起的
```

## 解决办法

手动加入这些依赖Jar包

```
<dependency>
    <groupId>javax.xml.bind</groupId>
    <artifactId>jaxb-api</artifactId>
    <version>2.3.0</version>
</dependency>
<dependency>
    <groupId>com.sun.xml.bind</groupId>
    <artifactId>jaxb-impl</artifactId>
    <version>2.3.0</version>
</dependency>
<dependency>
    <groupId>com.sun.xml.bind</groupId>
    <artifactId>jaxb-core</artifactId>
    <version>2.3.0</version>
</dependency>
<dependency>
    <groupId>javax.activation</groupId>
    <artifactId>activation</artifactId>
    <version>1.1.1</version>
</dependency>
 
```
