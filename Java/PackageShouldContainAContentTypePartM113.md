# XSSFWorkbook 读取xlsx Package should contain a content type part [M1.13]

## 一、程序报错
我这边用org.apache.poi做了excel读取功能（之前已经开发好了，用了很长一段时间了），今天突然报错 
```
生成方案异常信息为：org.apache.poi.POIXMLException: org.apache.poi.openxml4j.exceptions.InvalidFormatException: Package should contain a content type part [M1.13] 
```
* 目前的配置
org.apache.poi ：目前最稳定版本 3.17

* 读取Excel的代码
```
XSSFWorkbook wb = new XSSFWorkbook(fs);
```

fs解析的是我们的  模板文件，可以肯定后缀是xlsx  2007版本.

## 二、解决方案

### 1.更换模板文件
考虑程序之前已经运行很长一段时间没有异常，那么第一反应应该是模板出了问题（本地测试模板能WPS打开保存），明明是xlsx的后缀确一直Package should contain a content type part [M1.13] ，
是不是识别成了xls，故更换一次模板文件--------未解决

### 2.修改org.apache.poi版本
版本由3.17修改为3.1和3.9----------未解决

### 3.读取Excel判断后缀
```
// 判断excel的后缀，不同的后缀用不同的对象去解析
// xls是低版本的Excel文件
if (filename.endsWith(".xls")) {
  wb = new HSSFWorkbook(fs);
}
// xlsx是高版本的Excel文件
if (filename.endsWith(".xlsx")) {
  wb = new XSSFWorkbook(fs);
}
if (wb == null) {
  throw new NullArgumentException("模板文件为空");
}
```
----未解决，依然是进入 wb = new XSSFWorkbook(fs); 后报错

### 4.xlsx文档降级成xls
用office将xlsx文档降级成xls，结果反而出了新的异常  Failed to read zip entry source

问题：poi读取excel模板配置，按模板格式导出excel，读取excel模板时，报以上错误。

原因：excel模板在项目打包编译时，xlsx文件解压缩时出问题，可以去target\classes\templates查看，excel打不开，提示文件损坏

解决：在pom文件中加入以下配置即可避免此问题
```
<plugins>
<!-- 资源文件拷贝插件 -->
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-resources-plugin</artifactId>
  <configuration>
    <encoding>UTF-8</encoding>
    <!-- 过滤后缀文件 -->
    <nonFilteredFileExtensions>
      <nonFilteredFileExtension>xlsx</nonFilteredFileExtension>
      <nonFilteredFileExtension>xls</nonFilteredFileExtension>
    </nonFilteredFileExtensions>
  </configuration>
</plugin>
```
加上配置之后，文件没有损坏，但是回到了最初的问题 Package should contain a content type part [M1.13] 

### 5.最终解决

将xlsx文件用office打开后重新保存。

重启电脑

清除缓存重启idea

最后xlsx文档明显有更新

问题原因，应该是本机wps异常，打开excel后将文件损坏，导致读取时可能认为版本不一致或者已加密。

