# JDK版本新特性

## 概念

JDK版本新特性 适合按版本建立知识脉络。JDK 5 引入泛型、枚举、注解、自动装箱、增强 for、并发包；JDK 6 改进脚本、编译器 API、性能和监控；JDK 7 引入 try-with-resources、菱形语法、多 catch、NIO.2；JDK 8 引入 Lambda、Stream、Optional、java.time、接口默认方法。

面试和项目升级时，重点关注语法变化、标准库增强、JVM 改进以及对旧代码兼容性的影响。

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

- 只关注语法糖，忽略标准库和 JVM 行为变化。
- 升级 JDK 时没有检查依赖兼容性和非法反射访问。
- Stream/Lambda 滥用导致调试困难或性能不如普通循环。

## 总结

JDK版本新特性 适合做成版本演进表，重点记录语法、类库、JVM、工具链和兼容性变化。
