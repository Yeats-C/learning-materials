# java.text包

## 概念

java.text 主要用于文本、数字、日期的格式化和解析，常见类包括 DateFormat、SimpleDateFormat、DecimalFormat、MessageFormat。

需要注意 SimpleDateFormat 不是线程安全的，在多线程环境中不要作为共享静态实例使用。新项目中更推荐 java.time 包下的 DateTimeFormatter。

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

- SimpleDateFormat 作为 static 共享对象在多线程中使用。
- 日期格式使用 YYYY 而不是 yyyy，跨年周导致年份异常。
- 解析用户输入时没有明确 Locale 和时区。

## 总结

java.text 常用于老项目维护，新代码优先使用 java.time；维护旧代码时重点关注线程安全、格式符和时区。
