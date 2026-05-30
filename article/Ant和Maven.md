# Java 中的 Ant 与 Maven

## 📌 基本概念
- **[Ant](ca://s?q=Java_Ant是什么)**  
  Apache Ant 是一个早期的 Java 构建工具，基于 XML 配置文件（`build.xml`），通过定义 **任务（Task）** 来执行编译、打包、部署等操作。它类似于脚本化的自动化工具，灵活但需要开发者手动编写大量配置。

- **[Maven](ca://s?q=Java_Maven是什么)**  
  Apache Maven 是一个更现代的项目管理和构建工具，基于约定优于配置的思想。它使用 `pom.xml` 文件来定义项目结构、依赖和插件，自动化处理依赖下载、构建生命周期和发布流程。

---

## 🔎 核心区别

| 特性 | **[Ant](ca://s?q=Ant特点)** | **[Maven](ca://s?q=Maven特点)** |
|------|-----------------------------|---------------------------------|
| **配置方式** | 基于 XML 脚本，开发者手动编写任务 | 基于 POM（Project Object Model），声明式配置 |
| **依赖管理** | 无内置依赖管理，需要手动维护 JAR 包 | 内置依赖管理，自动下载并维护版本 |
| **构建生命周期** | 没有统一生命周期，完全由开发者定义 | 内置标准生命周期（compile、test、package、install、deploy） |
| **学习成本** | 灵活但繁琐，学习曲线较高 | 约定优于配置，学习成本较低 |
| **社区与生态** | 较老，使用逐渐减少 | 主流工具，生态丰富，广泛应用 |

---

## 📊 使用示例

### Ant 构建文件（build.xml）
```xml
<project name="Demo" default="compile">
  <target name="compile">
    <javac srcdir="src" destdir="build/classes"/>
  </target>
</project>
