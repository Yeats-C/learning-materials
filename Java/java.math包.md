# java.math包

## 概念

java.math 提供高精度数值计算能力，核心类是 BigInteger 和 BigDecimal。BigInteger 用于任意精度整数，BigDecimal 用于需要精确小数的场景，例如金额、计费、结算。

BigDecimal 使用时要避免直接传入 double，推荐使用字符串或 valueOf；除法要指定精度和舍入模式；比较大小通常使用 compareTo，而不是 equals，因为 equals 会比较精度。

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

- 用 new BigDecimal(double) 处理金额，产生精度误差。
- BigDecimal 除法不指定精度和舍入模式，运行时抛 ArithmeticException。
- 使用 equals 比较数值，忽略 scale 差异导致 1.0 和 1.00 不相等。

## 总结

java.math 主要解决精度问题，金额和计量类业务优先考虑 BigDecimal，并明确构造方式、舍入策略和比较方式。
