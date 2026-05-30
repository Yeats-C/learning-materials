# java.util包

## 概念

java.util 主要提供集合、日期时间早期 API、随机数、属性文件、定时任务、Optional、Scanner、UUID 等常用工具。集合体系是重点，包括 Collection、List、Set、Queue、Map 以及 Collections 工具类。

实际开发中最常用的是 ArrayList、LinkedList、HashMap、HashSet、TreeMap、PriorityQueue、Arrays、Collections、Optional、Properties。学习时要结合数据结构理解时间复杂度、线程安全和适用场景。

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

- HashMap 遍历时修改集合导致 ConcurrentModificationException。
- 把 ArrayList 当作线程安全集合在多线程中共享。
- 仍使用 Date、Calendar 处理复杂时间逻辑，而没有考虑 java.time。

## 总结

java.util 的重点是集合体系和常用工具类，真正要掌握的是不同实现背后的数据结构、复杂度、线程安全和业务适用场景。
