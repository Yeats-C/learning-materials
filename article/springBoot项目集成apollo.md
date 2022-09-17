# springBoot项目集成apollo

## 一、apollo简介

Apollo（阿波罗）是一款可靠的分布式配置管理中心，诞生于携程框架研发部，能够集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性，适用于微服务配置管理场景。

* AppId

AppId是应用的身份信息，是从服务端获取配置的一个重要信息。


* Namespace


Namespace是配置项的集合，类似于一个配置文件的概念。
Namespace文件有多种格式，例如：properties、xml、yml、yaml、json等。同样Namespace也具有这些格式。在Portal UI中可以看到“application”的Namespace上有一个“properties”标签，表明“application”是properties格式的。

[apollo使用指南](https://www.apolloconfig.com/#/zh/usage/apollo-user-guide?id=_11-%e5%88%9b%e5%bb%ba%e9%a1%b9%e7%9b%ae)

## 二、运行时环境

Apollo本地开发需要以下组件：

1.Java: 1.8+

2.MySQL: 5.6.5+

3.IDE: 没有特殊要求

## 三、springBoot项目集成apollo

* 1.添加apollo pom依赖

在<dependencies>标签里增加下方依赖：

```
		<!--apollo依赖-->
		<dependency>
			<groupId>com.ctrip.framework.apollo</groupId>
			<artifactId>apollo-client</artifactId>
			<version>1.7.0</version>
		</dependency>
```
  
* 2.启动类增加增加注解@EnableApolloConfig({"application"})
  
  ```
  @EnableApolloConfig({"data_flow_dq"})
  ```
  
  作用：此处增加的注解是用来指示Apollo注入application namespace的配置到Spring环境中

 * 3.启动类方法增加System.setProperty("app.id", "server-app-dev")
 
  ```
  public static void main(String[] args) {
    System.setProperty("app.id", "carbon_appid");

    //apollo与fgign联合使用隐藏bug，需需配置此项才能让feign加载时拿到阿波罗配置
    System.setProperty("apollo.bootstrap.namespaces", "data_flow_dq");
    //# 启用Apollo配置开关 在应用启动阶段是否向Spring容器注入被托管的properties文件配置信息。
    System.setProperty("apollo.bootstrap.enabled","true");
    //# 将Apollo配置加载提到初始化日志系统之前
    System.setProperty("apollo.bootstrap.eagerLoad.enabled","true");

    SpringApplication.run(TerminalDataFlowModelServiceApplication.class, args);
  }
  ```
  
  * 4.配置apollo meta server信息（此处只需要在服务器上配置）
  
对于Mac/Linux，默认文件位置为/opt/settings/server.properties
  
对于Windows，默认文件位置为C:\opt\settings\server.properties
  
增加配置文件server.properties
  
配置内容：
```
env=DEV
apollo.meta=http://127.0.0.1:8080
```

测试方法：
```
  @Value("${mysql.host:mysqlLocal}") //给一个默认值，如果没有找到输出local
  private String ceshi;

  @GetMapping(value = "/test")
  @ApiOperation(value = "测试apollo接口")
  public String test(){
    return ceshi;
  }
```
  
  ## Apollo应用配置

服务器地址：http://127.0.0.1:8070/
用户名：dev
密码：*********
  
 
  使用指南:
* 1.新建应用id
首页点击创建应用
![image](https://user-images.githubusercontent.com/64882640/190845210-86ef37ef-4471-4c9d-a258-d5705d2e4cc1.png)

AppId和应用名称建议一致，其他都为权限配置自行选择，点击提交。
![image](https://user-images.githubusercontent.com/64882640/190845226-0386f662-c095-4596-9fc5-dc4145987ad5.png)
  
可以选择不同环境新建Namespace
![image](https://user-images.githubusercontent.com/64882640/190845267-9da28489-8fcb-4e47-a5f1-3a7e8970f09d.png)

创建一个公共的Namespace,文件格式类型建议默认的properties目前这个格式最稳定
![image](https://user-images.githubusercontent.com/64882640/190845283-9c365027-0b28-4b44-ba55-05fb95e38229.png)

新增Namespace配置，点击提交，需要注意提交之后要点发布才能生效。
![image](https://user-images.githubusercontent.com/64882640/190845297-0b98c68c-f058-4be2-b140-5b8b1d3e5abb.png)
![image](https://user-images.githubusercontent.com/64882640/190845315-44c0d824-7572-4324-9ae4-200282231267.png)


 
