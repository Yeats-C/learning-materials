# java.lang包

## 概念

java.lang 是 Java 默认导入的基础包，包含 Object、String、StringBuilder、Math、System、Runtime、Thread、Class、Throwable 等核心类型。它支撑了 Java 对象模型、字符串处理、异常体系、线程基础和运行时访问。

学习这个包时要把重点放在几个主线：Object 定义所有对象共有能力；String 体现不可变对象和常量池；Throwable 构成异常体系；Class 与反射相关；System 和 Runtime 提供系统级能力；Thread 是并发编程的入口之一。

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

- 把 String、StringBuilder、StringBuffer 混用，忽略不可变对象和线程安全差异。
- 使用 System.currentTimeMillis 做业务时间判断，却没有考虑时钟回拨和时区问题。
- 捕获 Throwable 或 Error，导致 JVM 级错误被错误吞掉。

## 总结

java.lang 是 Java 基础能力的底座，复习时应围绕 Object、String、Throwable、Class、System、Thread 建立主线，而不是零散记类名。
