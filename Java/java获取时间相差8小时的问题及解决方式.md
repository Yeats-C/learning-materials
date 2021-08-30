# java获取时间相差8小时的问题及解决方式

## 三种时间差错问题：
```
java下使用new date（）获取的时间会和真实的本地时间相差8小时。
本地获取的时间没有错，存入数据库的时候时间相差8小时。
数据库时间没有错，获取到了后端，之后返回给前端相差8小时。
```

## 原因：
```
new date（）调用的是jvm时间，而jvm使用的时间默认是0时区的时间，即：和北京时间将会相差8小时。
mybatis将本地的数据传入到mysql数据库服务器的时候，服务器会对数据进行检测，会把date类型的数据自动转换为mysql服务器所对应的时区，即0时区，所以会相差8小时。
springboot中对加了@RestController或者@Controller+@ResponseBody注解的方法的返回值默认是Json格式，
所以，对date类型的数据，在返回浏览器端时，会被springboot默认的Jackson框架转换，而Jackson框架默认的时区GMT（相对于中国是少了8小时）。所以最终返回到前端结果是相差8小时
```

## 解决方案：
* 手动设置jvm时间：将时间改为第8时区的时间：
```
TimeZone.setDefault(TimeZone.getTimeZone("GMT+8"));
```

* springboot项目，可以面向切面加上这个，或者启动main类上加上如下代码：
```
@PostConstruct
void started() {
    TimeZone.setDefault(TimeZone.getTimeZone("GMT+8"));
}
```
* 在apprication.yml文件中配置一下数据库连接信息，url加上这么一句
```
&serverTimezone=GMT%2b8
```


* 将spring的json构造器的时区改正即可，在application.yml文件中添加：
```
jackson:
    date-format: yyyy-MM-dd HH:mm:ss #jackson对响应回去的日期参数进行格式化
    time-zone: GMT+8
    serialization:
      write-dates-as-timestamps: false
```
