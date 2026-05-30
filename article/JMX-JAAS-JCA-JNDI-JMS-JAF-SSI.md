# JMX、JAAS、JCA、JNDI、JMS、JAF、SSI

## 概念

JMX、JAAS、JCA、JNDI、JMS、JAF、SSI 是 JavaWeb 技术体系中的一个知识点。JavaWeb 的核心是请求、响应、会话、协议、服务器、业务处理和数据访问之间的协作。

学习时应把它放在一次 HTTP 请求的链路中理解：浏览器或客户端发起请求，服务器接收并分发，业务代码处理，最后返回页面、JSON 或其他响应。

## 学习重点

- 理解它在 Web 请求链路中的位置。
- 关注协议、状态、编码、性能和安全边界。
- 结合真实项目排查请求失败、响应慢、会话丢失等问题。

## 使用场景

- 面试复习时，用于梳理概念、边界和常见追问。
- 项目开发时，用于判断技术选型、代码写法和排查方向。
- 线上问题处理时，用于快速定位相关模块和可能风险。

## 示例

```java
// 示例：用一个最小入口观察当前知识点的运行方式。
public class Example {
    public static void main(String[] args) {
        System.out.println("learn " + Example.class.getSimpleName());
    }
}
```

## 常见问题

- 这些技术点缩写多，容易只记名字不理解使用场景。
- JNDI/JMS 等配置依赖容器环境，迁移时容易出错。
- 安全相关配置缺少最小权限原则。

## 总结

JMX、JAAS、JCA、JNDI、JMS、JAF、SSI 要放在完整请求链路中理解，重点关注协议、状态、编码、安全、性能以及与后端服务的边界。
