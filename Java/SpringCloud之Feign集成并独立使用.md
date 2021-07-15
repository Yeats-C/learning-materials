# SpringCloud之Feign集成并独立使用
Feign 是一个声明式REST Web服务客户端，可以处理微服务间的Web服务调用。他是使用注解加接口的形式形成去调用服务的,这里主要是Spring boot 单独使用 Feign 发起网络请求.

## 一、Feign 依赖
```
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-openfeign</artifactId>
  <version>2.2.0.RELEASE</version>
</dependency>
```

## 二、开启 Feign

* 在 Application 中添加注解
```
@EnableFeignClients(basePackages = {"pro.photovoltaic.photovoltaicWeixinService.*"})
```
**@EnableFeignClients 扫描标记@FeignClient的接口并创建实例bean

basePackages 扫描的包**

```
/**
 * 启动类
 */
@SpringBootApplication
//@EnableDiscoveryClient
//@EnableScheduling //定时任务
@MapperScan(basePackages = {"pro.photovoltaic.photovoltaicWeixinService.mapper"})
@Component
@EnableFeignClients(basePackages = {"pro.photovoltaic.photovoltaicWeixinService.*"})
public class PhotovoltaicWeixinServiceApplication {

  public static void main(String[] args) {
    SpringApplication.run(PhotovoltaicWeixinServiceApplication.class, args);
  }

  /**
   * 配置跨域访问的过滤器
   *
   * @return
   */
  @Bean
  public FilterRegistrationBean registerFilter() {
    FilterRegistrationBean bean = new FilterRegistrationBean();
    bean.addUrlPatterns("/*");
    bean.setFilter(new CrosFilter());
    return bean;
  }
}

```
## 三、使用 Feign

* 创建 API 接口
* 使用 FeignClient 注解
```
package pro.photovoltaic.photovoltaicWeixinService.service;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import pro.photovoltaic.photovoltaicWeixinService.domain.ResultDTO;

@FeignClient(value = "demo-service", url = "127.0.0.1:10115")
public interface HelloServiceFeign {

  @RequestMapping(value = "common/data/idWorker/{workerId}/{datacenterId}/{sequence}")
  public ResultDTO idWorker(@PathVariable int workerId,@PathVariable int datacenterId,@PathVariable int sequence);


}

```

**@FeignClientFeign** 客户端,使用时需要在 application 中添加 @EnableFeignClients 注解.
**value** 服务名称,结合 Spring Cloud 系列组件可通过服务名称进行接口调用
**url** 接口请求地址.
**path**接口请求路径

## 四、使用代码调用 Feign 接口
```
package pro.photovoltaic.photovoltaicWeixinService.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import pro.photovoltaic.photovoltaicWeixinService.domain.ResultDTO;
import pro.photovoltaic.photovoltaicWeixinService.service.HelloServiceFeign;

/**
 * @description:
 * @author: chensifang
 * @create_date 2021年07月13日 17:51
 */
@RestController
public class FeignController {

  @Autowired
  private HelloServiceFeign helloServiceFeign;

  @RequestMapping(value = "test/data/idWorker/{workerId}/{datacenterId}/{sequence}")
  public ResultDTO idWorker(@PathVariable int workerId,@PathVariable int datacenterId,@PathVariable int sequence){

    return helloServiceFeign.idWorker(workerId,datacenterId,sequence);
}

}

```
