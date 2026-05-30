# JUnit 指的是什么

## 📌 [JUnit](ca://s?q=JUnit是什么)
- **JUnit** 是一个 **Java 单元测试框架**，用于编写和运行可重复的测试。  
- 它帮助开发者验证代码的正确性，确保修改或重构不会破坏已有功能。  
- 是 **测试驱动开发（TDD）** 的核心工具之一，也是 Java 项目中最常用的测试框架。

## 🔎 核心特性
- **[注解驱动](ca://s?q=JUnit注解驱动)**：通过 `@Test`、`@Before`、`@After` 等注解定义测试方法和生命周期。  
- **[断言机制](ca://s?q=JUnit断言机制)**：使用 `assertEquals`、`assertTrue` 等断言方法验证结果。  
- **[测试套件](ca://s?q=JUnit测试套件)**：支持将多个测试类组合成一个测试套件统一运行。  
- **[集成工具](ca://s?q=JUnit集成工具)**：与 IDE（Eclipse、IntelliJ）、构建工具（Maven、Gradle）无缝集成。  

## 📊 使用示例
```java
import org.junit.Test;
import static org.junit.Assert.*;

public class CalculatorTest {
    @Test
    public void testAdd() {
        Calculator calc = new Calculator();
        int result = calc.add(2, 3);
        assertEquals(5, result); // 验证结果是否为 5
    }
}
```

@Test → 声明这是一个测试方法。

assertEquals → 验证预期值与实际值是否一致

## 📊 总结

JUnit 是 Java 开发中进行单元测试的标准工具。

它通过 注解、断言、测试套件 等机制，帮助开发者快速验证代码逻辑。

在现代开发中，JUnit 常与 Maven、Gradle、CI/CD 工具 结合使用，成为保障软件质量的重要一环。
