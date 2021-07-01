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

* 注：如果是比较复杂的对象的话，我们可以用TypeReference来进行转换，如：
       A<B> instance = JSON.parseObject(jsonStr, new TypeReference<A<B>>() {});

```
提示1：实体类模型的setter、getter方法一定要按标准来；否者fastjson将不能识别，
             导致转换出错。

提示2：当使用fastjson将json字符串转化为对象时，fastjson默认是对大小写不敏感的。
            即：假设json字符串里面的key为aBCd,对象里面的属性是abcd，那么也该属
                   性也是能够转换的；注意:Spring的jackson默认是大小写敏感的。

提示3：我们在将对象转化为json字符串时,可以使用@JSONField()注解来初步做一些
             配置，如：设置某一属性转换为指定key的json值、设置该属性对应的值在
             转换后的json字符串的哪一个位置等等,
```
