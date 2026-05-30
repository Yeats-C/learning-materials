# EJB

## 概念

在 Java 里，EJB 指的是 Enterprise JavaBeans，它是早期 J2EE（现 Jakarta EE） 规范中的一个核心组件，用来简化和标准化企业级应用的开发。

企业级组件  
EJB 是一种服务器端组件，用来封装业务逻辑，运行在应用服务器（如 JBoss、GlassFish、WebLogic）里。

分布式支持  
它天然支持分布式系统，可以让不同客户端通过远程调用访问同一个业务逻辑。

事务管理  
内置事务处理机制，开发者不用手写复杂的事务代码。

安全性  
提供声明式安全控制，方便在企业应用中管理权限。

生命周期管理  
容器负责创建、销毁和管理 EJB 对象，开发者只需关注业务逻辑。

## 类型

Session Bean  
封装业务逻辑，分为 Stateless（无状态）、Stateful（有状态）、Singleton（单例）。

Message-Driven Bean  
用来处理异步消息（如 JMS），常用于消息队列系统。

Entity Bean  
早期用来表示数据库实体，但后来被 JPA 替代。

## 示例

```java
@Stateless
public class OrderServiceBean implements OrderService {
    public void placeOrder(Order order) {
        // 业务逻辑：保存订单、处理支付等
    }
}
```
这里的 @Stateless 表示这是一个无状态的 Session Bean，由容器管理生命周期和事务。


## 总结

EJB 的目标是让开发者专注于业务逻辑，而把 事务、安全、分布式调用 等复杂问题交给容器处理。
不过，随着 Spring 等轻量级框架的流行，EJB 在现代开发中使用得越来越少，但它仍然是理解 Java 企业级开发历史 的重要一环。
