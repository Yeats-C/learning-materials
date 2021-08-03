# 若依后台框架学习一：前后端分离后API交互如何保证数据安全性？

前后端分离的开发方式，我们以接口为标准来进行推动，定义好接口，各自开发自己的功能，最后进行联调整合。无论是开发原生的APP还是webapp还是PC端的软件,只要是前后端分离的模式，就避免不了调用后端提供的接口来进行业务交互。

网页或者app，只要抓下包就可以清楚的知道这个请求获取到的数据，这样的接口对爬虫工程师来说是一种福音，要抓你的数据简直轻而易举。

数据的安全性非常重要，特别是用户相关的信息，稍有不慎就会被不法分子盗用，所以我们对这块要非常重视，容不得马虎。

## 二、如何保证API调用时数据的安全性？

1、通信使用https

2、请求签名，防止参数被篡改

3、身份确认机制，每次请求都要验证是否合法

4、APP中使用ssl pinning防止抓包操作

5、对所有请求和响应都进行加解密操作

## 三、对所有请求和响应都进行加解密操作

方案有很多种，当你做的越多，也就意味着安全性更高，今天我跟大家来介绍一下对所有请求和响应都进行加解密操作的方案，即使能抓包，即使能调用我的接口，但是我返回的数据是加密的，只要加密算法够安全，你得到了我的加密内容也对我没什么影响。

像这种工作最好做成统一处理的，你不能让每个开发都去关注这件事情，如果让每个开发去关注这件事情就很麻烦了，返回数据时还得手动调用下加密的方法，接收数据后还得调用下解密的方法。

* 先来看看怎么使用，启动类上增加@EnableEncrypt注解开启加解密操作

![image](https://user-images.githubusercontent.com/64882640/127992419-ab7b8d1d-39dd-46f9-b5ab-427e71ad749b.png)

 * 增加加密的key配置：
```
spring.encrypt.key：加密key，必须是16位

spring.encrypt.debug：是否开启调试模式,默认为false,如果为true则不启用加解密操作
```

为了考虑通用性，不会对所有请求都执行加解密，基于注解来做控制

响应数据需要加密的话，就在Controller的方法上加@Encrypt注解即可。

![image](https://user-images.githubusercontent.com/64882640/127992510-b7b5eca5-f25a-4feb-8af6-0e562a807a09.png)

当我们访问/list接口时，返回的数据就是加密之后base64编码的格式。

还有一种操作就是前段提交的数据，分为2种情况，一种是get请求，这种暂时没处理，后面再考虑，目前只处理的post请求，基于json格式提交的方式，也就是说后台需要用@RequestBody接收数据才行, 需要解密的操作我们加上@Decrypt注解即可。

![image](https://user-images.githubusercontent.com/64882640/127992590-b9e56ee7-dcfc-41f7-a3af-c068b166fdb7.png)

加了@Decrypt注解后，前端提交的数据需要按照AES加密算法，进行加密，然后提交到后端，后端这边会自动解密，然后再映射到参数对象中。

上面讲解的都是后端的代码，前端使用的话我们以js来讲解，当然你也能用别的语言来做，如果是原生的安卓app也是用java代码来处理。

* 前端需要做的就2件事情：

1、统一处理数据的响应，在渲染到页面之前进行解密操作

2、当有POST请求的数据发出时，统一加密

封装一个js加解密的类，需要注意的是加密的key需要和后台的对上，不然无法相互解密，代码如下：

![image](https://user-images.githubusercontent.com/64882640/127992794-d507745b-5a66-415e-84de-f6b521c5a982.png)

axios拦截器中统一处理代码：

![image](https://user-images.githubusercontent.com/64882640/127992858-f195e03a-d405-4cca-ae20-2d0b26bd2b6a.png)


到此为止，我们就为整个前后端交互的通信做了一个加密的操作，只要加密的key不泄露，别人得到你的数据也没用，问题是如何保证key不泄露呢？

服务端的安全性较高，可以存储在数据库中或者配置文件中，毕竟在我们自己的服务器上，最危险的其实就时前端了，app还好，可以打包，但是要防止反编译等等问题。

## 四、spring-boot-starter-encrypt原理

最后来简单的介绍下spring-boot-starter-encrypt的原理吧，也让大家能够理解为什么Spring Boot这么方便，只需要简单的配置一下就可以实现很多功能。

启动类上的@EnableEncrypt注解是用来开启功能的,通过@Import导入自动配置类

![image](https://user-images.githubusercontent.com/64882640/127993060-ab07cfa4-0f89-4255-a6f0-c29cf5e8835b.png)

EncryptAutoConfiguration中配置请求和响应的处理类，用的是Spring中的RequestBodyAdvice和ResponseBodyAdvice，在Spring中对请求进行统计处理比较方便。如果还要更底层去封装那就要从servlet那块去处理了。

![image](https://user-images.githubusercontent.com/64882640/127993112-2bc5daa4-5b2d-46df-a2b5-2dc51dc2cc2b.png)

通过RequestBodyAdvice和ResponseBodyAdvice就可以对请求响应做处理了，大概的原理就是这么多了。

[参考：](https://github.com/yinjihuan/monkey-api-encrypt/wiki/%E4%BD%BF%E7%94%A8%E6%96%87%E6%A1%A3)
