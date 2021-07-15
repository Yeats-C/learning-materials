# Dubbo与Feign对比
* 前言 TODO

## 一、共同点
Dubbo 与 Feign 都是用作服务与服务之间互相调用。
Dubbo 与 Feign 都依赖注册中心、负载均衡。

## 二、区别

### 1、协议

**Dubbo：**

* 支持多传输协议(Dubbo、Rmi、http、redis等等)，可以根据业务场景选择最佳的方式。非常灵活。
* 默认的Dubbo协议：利用Netty，TCP传输，单一、异步、长连接，适合数据量小、高并发和服务提供者远远少于消费者的场景。

**Feign：**

* 基于Http传输协议，短连接，不适合高并发的访问

### 2、负载均衡

**Dubbo：**

* 支持4种算法（随机、轮询、活跃度、Hash一致性），而且算法里面引入权重的概念。
* 配置的形式不仅支持代码配置，还支持Dubbo控制台灵活动态配置。
* 负载均衡的算法可以精准到某个服务接口的某个方法。

**Feign：**
* 只支持N种策略：轮询、随机、ResponseTime加权。
* 负载均衡算法是Client级别的。

### 3、容错策略

**Dubbo：**

* 支持多种容错策略：failover、failfast、brodecast、forking等，也引入了retry次数、timeout等配置参数。

**Feign：**

* 利用熔断机制来实现容错的，处理的方式不一样。

### 4、实际使用

**Dubbo：**

* RPC

**Feign：**

* REST API。用HTTP确实不太看好它，奈何是Spring Cloud生态一环。


参考：[远程调用 Dubbo 与 Feign 的区别](https://blog.csdn.net/riemann_/article/details/108762693)
