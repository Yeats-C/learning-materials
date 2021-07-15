# SpringCloud之Feign集成并独立使用
Spring boot 单独使用 Feign 发起网络请求.

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
