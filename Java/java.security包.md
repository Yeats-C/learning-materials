# java.security包

## 概念

java.security 提供 Java 安全体系的基础能力，包括消息摘要、签名、密钥、权限、证书、安全随机数等。常见使用点有 MessageDigest、SecureRandom、Signature、KeyPairGenerator。

业务开发中常见场景包括密码摘要、接口签名、token 随机值生成、证书校验和加密体系集成。

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

- 密码只做 MD5 或 SHA 摘要，没有加盐和迭代。
- 生成 token 使用 Random 而不是 SecureRandom。
- 签名验签时字符集、排序、大小写不一致导致验签失败。

## 总结

java.security 的重点是安全边界和不可预测性，摘要、签名、随机数、证书都要明确算法、输入格式和密钥管理方式。
