# Nacos学习笔记

## 一、什么是Nacos
官方：一个更易于构建云原生应用的动态“服务发现（Nacos Discovery）”、“服务配置（Nacos Config）”和服务管理平台。

集  注册中心+配置中心+服务管理 平台

Nacos关键特性：
服务发现和服务健康监测
动态配置服务
动态DNS服务
服务及其元数据管理

## 二、Nacos相关软件对比

![image](https://user-images.githubusercontent.com/64882640/174011312-0c00e657-a7e9-4034-b1df-4d254269b4f9.png)

![image](https://user-images.githubusercontent.com/64882640/174031629-72043e53-815d-4ddf-b118-19c0d4a78c06.png)


## 三、Nacos集成到我们的服务配置项

### 1.修改pom文件

Nacos有个不太方便的点，他与springCloud及springboot项目集成时，需指定特定版本，才能使用相关功能。

* 首先，修改springboot版本。指定修改为：2.3.12.RELEASE

```
parent>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-parent</artifactId>
 <version>2.3.12.RELEASE</version>
 <relativePath/> <!-- lookup parent from repository -->
</parent>
```

![image](https://user-images.githubusercontent.com/64882640/174705685-c21de6c8-5547-4350-b542-e70679ccf3b5.png)

* 然后，修改springCloud版本。指定版本为：2.2.7.RELEASE

```
<dependencyManagement>
 <dependencies>
  <dependency>
   <groupId>com.alibaba.cloud</groupId>
   <artifactId>spring-cloud-alibaba-dependencies</artifactId>
   <version>2.2.7.RELEASE</version>
   <type>pom</type>
   <scope>import</scope>
  </dependency>
 </dependencies>
</dependencyManagement>
```

![image](https://user-images.githubusercontent.com/64882640/174705936-0fc0c650-e6b2-4460-8d45-64ba6f4f2988.png)

* 最后，增加nacos-config配置中心及nacos-服务注册与发现dependency

```
<!--nacos-config配置中心-->
<dependency>
 <groupId>com.alibaba.cloud</groupId>
 <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>

<!--nacos-服务注册与发现-->
<dependency>
 <groupId>com.alibaba.cloud</groupId>
 <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

![image](https://user-images.githubusercontent.com/64882640/174706006-dd580d33-ff5a-4112-a3ce-9205dd24bed5.png)

### 2.在resources层级下增加bootstrap.properties文件

bootstrap.properties的加载是先于application.properties的
此文件主要是nacos配置中心相关配置

```
#以下配置使用 spring cloud for alibaba,最主要是的pom文件里面对spring-cloud-alibaba-dependencies的引用
#程序名称
#spring.application.name=nacos-test
#以下为自定义且为绑定多dataId配置
#多 dataId 下，数组下标越大，代表优先级越高 所以当出现同名配置时，优先级越高的配置会覆盖优先级较低的配置
#nocas 配置地址
spring.cloud.nacos.config.server-addr=192.168.110.110:8848
#自定义Namespace此处为新建namespace的 ID，请切记不要自定义，因为nacos BUG 没有修复，自定义 ID 会导致绑定不了配置
spring.cloud.nacos.config.namespace=921b06db-b901-4296-8d5b-1fa42b5c7986

#此处为扩展data id 的属性配置，目前只支持三个属性，nacos 2.2.0会增加一个 fileExtension
#详情请看pr https://github.com/alibaba/spring-cloud-alibaba/pull/2538
#以用 issusehttps://github.com/alibaba/spring-cloud-alibaba/issues/2537
#基于以上信息，所以自定义 dataId 必须添加文件后缀，且与内容格式相对应
#多组dataId配置不支持同时存在，目前测出来的是 extension与shared 同时存在会初始化报错
#与 application.name 对应的 dataId这种配置方式同时存在会出现绑定不了配置的情况

spring.cloud.nacos.config.extension-configs[0].data-id=test.properties
# 配置 Data Id 所在分组，缺省默认 DEFAULT_GROUP
spring.cloud.nacos.config.extension-configs[0].group=DEFAULT_GROUP
# 配置Data Id 在配置变更时，是否动态刷新，缺省默认 false
spring.cloud.nacos.config.extension-configs[0].refresh=true

spring.cloud.nacos.config.extension-configs[1].data-id=test.yaml
# 配置 Data Id 所在分组，缺省默认 DEFAULT_GROUP
spring.cloud.nacos.config.extension-configs[1].group=DEFAULT_GROUP
# 配置Data Id 在配置变更时，是否动态刷新，缺省默认 false
spring.cloud.nacos.config.extension-configs[1].refresh=true
```
此格式支持多个yml配置

### 3.增加application.yml配置

```
spring:
  cloud:
    nacos:
      server-addr: 192.168.110.110:8848
      discovery:
        username: nacos
        password: nacos
        namespace: public
        ephemeral: false # true 临时实例 15秒心跳轮询 变更状态 30秒下线服务   false 永久实例 哪怕宕机了也不会删除实例
```

![image](https://user-images.githubusercontent.com/64882640/174706153-ddd423cc-b00b-43c2-8b54-f09283053810.png)

此配置主要用于nacos服务注册与发现

### 4.可在启动类用以下代码测试是否已拉取到nacos配置

```
ConfigurableApplicationContext applicationContext=SpringApplication.run(EnterpriseInformationCirculationModelServiceApplication.class, args);
while (true){
  //当动态配置刷新时，会更新到 Enviroment中，因此这里每隔一秒中从Enviroment中获取配置
  String userName = applicationContext.getEnvironment().getProperty("user.name");
  String userAge = applicationContext.getEnvironment().getProperty("user.age");
  System.err.println("from test.properties user name :" + userName + "; age: " + userAge);

  String userName2 = applicationContext.getEnvironment().getProperty("user2.name");
  String userAge2 = applicationContext.getEnvironment().getProperty("user2.age");
  System.err.println("from test.yaml user2 name :" + userName2 + "; age: " + userAge2);
  TimeUnit.SECONDS.sleep(1);
}
```

![image](https://user-images.githubusercontent.com/64882640/174706358-6302cc40-9529-4464-8de6-d45fc4f6cf68.png)




```
2022-06-16 15:41:26.152  INFO 17076 --- [           main] c.a.c.n.registry.NacosServiceRegistry    : nacos registry, DEFAULT_GROUP regional-current-period-summary-model-service 192.168.110.20:10038 register finished

```

![image](https://user-images.githubusercontent.com/64882640/174707740-5bbacfef-0893-4bf9-8d25-4fa1a1f5960d.png)

启动服务后，在服务器服务列表可查看服务是否正常注册

## 四、服务区配置中心配置

![image](https://user-images.githubusercontent.com/64882640/174708198-4e6c521b-2947-40b6-bc09-e60bad60342f.png)

在nacos服务器配置yml

* 1.登录nacos之后可在命名空间创建新的命名空间，此处为最大组管理

![image](https://user-images.githubusercontent.com/64882640/174708249-1d0b22a3-c27c-4a3c-b2a0-712cda445e85.png)

* 2.在配置管理-配置列表选择不同的命名空间，新增配置，

![image](https://user-images.githubusercontent.com/64882640/174708318-8de71033-c47b-433b-9e27-5288819d9261.png)

![image](https://user-images.githubusercontent.com/64882640/174708342-78bfea80-6412-40bc-bd2f-930b9a97878c.png)

dataId为唯一标识，group为命名空间下再次分组，可选五种配置格式。（注意：一定要注意格式，一个配置项出错，全部都 load 不出来）且nacos默认空间public存在许多bug，不推荐使用。配置项需要与bootstrap.properties配置项匹配，才能正确拉取到配置。





