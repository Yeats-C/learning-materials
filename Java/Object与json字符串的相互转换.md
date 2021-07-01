# Object与json字符串的相互转换

## 一、引入fastjson的依赖jar包
```
<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.2.59</version>
		</dependency> 
```

## 二、进行JSON字符换与Object的相互转换

* Object转 JSON
```
JSON.toJSONString(map)
```

* JSON转Object
```
 //将jsonString转换为User对象
 User user=JSON.parse0bject(jsonString, User.class);
```
