# XML解析：SAX、DOM、JDOM

## 概念

XML解析：SAX、DOM、JDOM 是 Java 处理 XML 的常见方式。SAX 适合流式读取、内存占用低；DOM 会把文档加载成树，操作方便但占内存；JDOM 更偏向 Java 风格封装。

如果只读取大文件，优先考虑 SAX 或 StAX；如果需要频繁增删改节点，DOM/JDOM 会更直观。

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

- DOM 解析大 XML 导致内存占用过高。
- 开启外部实体解析，存在 XXE 安全风险。
- 命名空间处理不正确，节点查询结果为空。

## 总结

XML解析：SAX、DOM、JDOM 需要根据文档大小、访问方式和安全要求选择解析模型，同时默认防范外部实体和命名空间问题。
