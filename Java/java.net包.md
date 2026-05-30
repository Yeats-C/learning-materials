# java.net包

## 概念

java.net 提供网络编程基础能力，包括 URL、URI、InetAddress、Socket、ServerSocket、URLConnection 等。它适合理解 TCP/UDP、客户端服务端通信、HTTP 访问和地址解析。

实际项目中直接使用 Socket 的机会不多，但理解它能帮助排查连接超时、端口占用、DNS 解析、HTTP 调用失败等问题。

## 学习重点

- 掌握核心概念和适用场景。
- 理解常见用法、边界条件和容易踩坑的地方。
- 结合代码或项目案例进行验证。

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

- 只看到接口超时，没有区分连接超时、读取超时和 DNS 解析问题。
- Socket 使用后未关闭，导致连接泄漏或端口资源耗尽。
- 把 URI 和 URL 混用，忽略编码、转义和路径规范化差异。

## 总结

java.net 的价值不仅是写 Socket，更是帮助理解网络调用失败时的分层原因：地址、端口、连接、协议、超时和资源释放。
