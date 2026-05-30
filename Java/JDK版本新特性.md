# JDK版本新特性

## 版本定位

这篇笔记重点整理几个实际项目和面试中最常被问到、也最值得关注的版本：

- JDK 8：老项目存量最多的长期使用版本，核心变化是 Lambda、Stream、新日期时间 API。
- JDK 17：现代 Java 的重要 LTS 基线，很多企业从 8/11 升级时会选择它。
- JDK 21：新一代 LTS，虚拟线程正式可用，是并发模型变化最大的版本之一。
- JDK 26：截至当前资料整理时的最新 Java SE 版本线，适合了解最新语言、库和 JVM 演进方向；生产环境仍建议优先选择厂商支持明确的 LTS 版本。

## 一、JDK 8：函数式编程和现代 Java 的起点

JDK 8 是 Java 生态非常关键的一版，很多后续写法都从这里开始变化。

### 核心特性

- Lambda 表达式：用更简洁的方式传递行为，例如 `(a, b) -> a + b`。
- 函数式接口：只有一个抽象方法的接口，可以配合 Lambda 使用，例如 `Runnable`、`Comparator`、`Function`、`Predicate`。
- Stream API：对集合进行声明式处理，常见操作包括 `filter`、`map`、`sorted`、`collect`。
- 接口默认方法和静态方法：接口可以提供默认实现，解决接口演进的兼容问题。
- Optional：用于表达“可能为空”的返回值，减少直接返回 `null` 的场景。
- 新日期时间 API：`java.time` 包引入 `LocalDate`、`LocalDateTime`、`Instant`、`Duration`、`DateTimeFormatter` 等。
- CompletableFuture：更方便地编排异步任务。
- Base64 API：标准库内置 Base64 编解码能力。
- Parallel Array Sorting：数组并行排序能力。
- Nashorn JavaScript 引擎：当时用于在 JVM 中运行 JavaScript，后续版本中已被移除。

### 常用写法示例

```java
List<String> names = List.of("Tom", "Jerry", "Alice");

List<String> result = names.stream()
        .filter(name -> name.length() > 3)
        .map(String::toUpperCase)
        .sorted()
        .toList();
```

如果项目仍是 JDK 8，最后一行不能用 `toList()`，需要写成：

```java
.collect(Collectors.toList());
```

因为 `Stream.toList()` 是更高版本才加入的。

### 重点理解

- Lambda 不是匿名内部类的完全替代品，`this` 指向、变量捕获、序列化等细节不同。
- Stream 适合表达数据处理流程，但不适合所有循环场景。
- 并行流不是万能优化，默认使用 ForkJoinPool.commonPool，可能和其他并发任务互相影响。
- `Optional` 不建议用于实体字段、方法参数和序列化对象，常用于返回值。
- `java.time` 比 `Date`、`Calendar`、`SimpleDateFormat` 更清晰，也更适合多线程。

### 常见问题

- Stream 链路过长，调试和排查比普通循环困难。
- 在 Stream 中写有副作用的逻辑，例如修改外部集合，容易引发并发或可读性问题。
- `parallelStream()` 盲目使用，数据量小或任务不是 CPU 密集时反而更慢。
- `Optional.get()` 直接调用，没有先判断，和空指针风险本质上差不多。
- `SimpleDateFormat` 在老代码中继续作为静态共享变量使用，仍然会有线程安全问题。

## 二、JDK 17：现代 Java LTS 基线

JDK 17 是很多团队从 JDK 8 升级时的目标版本。它不仅包含 17 本身的特性，也承接了 9 到 17 之间的一系列变化。

### 核心特性

- Sealed Classes：密封类，限制哪些类可以继承或实现某个类型。
- Pattern Matching for `instanceof`：`instanceof` 判断后可以直接得到目标类型变量。
- Text Blocks：多行字符串，适合 SQL、JSON、HTML 等文本。
- Records：用于表达不可变数据载体，减少样板代码。
- Switch Expressions：`switch` 可以作为表达式返回值。
- Strong Encapsulation of JDK Internals：JDK 内部 API 强封装，非法反射访问会更容易暴露问题。
- Enhanced Pseudo-Random Number Generators：增强伪随机数生成器。
- Context-Specific Deserialization Filters：更细粒度的反序列化过滤，提升安全性。
- macOS/AArch64 支持：对 Apple Silicon 更友好。
- Applet API、Security Manager 等进入废弃或移除路径。

### 常用写法示例

```java
record User(Long id, String name) {}

Object value = new User(1L, "Tom");

if (value instanceof User user) {
    System.out.println(user.name());
}
```

Text Blocks 示例：

```java
String sql = """
        SELECT id, name
        FROM user
        WHERE status = 1
        ORDER BY created_time DESC
        """;
```

### 从 JDK 8 升级到 17 要重点关注

- 模块系统从 JDK 9 开始引入，虽然普通项目不一定主动模块化，但依赖和反射行为会受影响。
- `javax.*` 到 `jakarta.*` 的迁移不是 JDK 本身导致的，但常常和 Spring Boot、Tomcat、Jakarta EE 升级一起出现。
- 老框架可能依赖 JDK 内部类，例如 `sun.misc.Unsafe` 或反射访问私有字段，需要升级依赖。
- GC 默认策略已经变化，JDK 9 之后默认 GC 是 G1。
- PermGen 早已被 Metaspace 替代，排查内存问题时不要再用老参数思路。
- Nashorn、Applet、RMI Activation、Security Manager 等老能力已经不适合作为新项目依赖。

### 常见问题

- 依赖版本太旧，启动时报非法反射、类找不到或模块访问异常。
- Maven/Gradle 编译版本、运行版本、IDE 配置版本不一致。
- 使用 Lombok、ASM、ByteBuddy、CGLIB、Spring 老版本时，与 JDK 17 字节码或模块封装不兼容。
- 升级只改 JDK，不配套升级 CI、Docker 基础镜像、监控 Agent 和构建插件。
- 只测业务接口，不做启动参数、GC、内存和压测对比。

## 三、JDK 21：虚拟线程正式可用的新 LTS

JDK 21 是非常值得关注的 LTS，因为它让虚拟线程正式成为 Java 标准能力。对于大量阻塞式 I/O 的服务端应用，这是一次重要变化。

### 核心特性

- Virtual Threads：虚拟线程正式发布，用更低成本承载大量并发阻塞任务。
- Sequenced Collections：为有顺序的集合提供统一 API，例如获取第一个、最后一个元素。
- Record Patterns：配合 record 做模式解构。
- Pattern Matching for `switch`：增强 `switch` 的类型匹配能力。
- Generational ZGC：分代 ZGC，改善低延迟 GC 在对象分代场景下的表现。
- Key Encapsulation Mechanism API：密钥封装机制 API。
- String Templates：预览特性，用于更安全、结构化地处理字符串模板。
- Structured Concurrency：预览特性，用于更清晰地表达多个并发子任务的生命周期关系。
- Scoped Values：预览特性，提供线程内/调用链上下文传递的新方式。

### 虚拟线程示例

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    Future<String> future = executor.submit(() -> {
        Thread.sleep(1000);
        return "done";
    });

    System.out.println(future.get());
}
```

虚拟线程适合大量阻塞式任务，例如：

- HTTP/RPC 调用。
- 数据库查询。
- 文件或网络 I/O。
- 等待外部系统响应的业务流程。

但它不等于无限并发。数据库连接池、下游接口、限流、锁竞争仍然是瓶颈。

### Sequenced Collections 示例

```java
SequencedCollection<String> list = new ArrayList<>();
list.add("A");
list.add("B");

System.out.println(list.getFirst());
System.out.println(list.getLast());
```

### 从 JDK 17 升级到 21 要重点关注

- 如果项目是典型 Spring Web + JDBC/MyBatis 阻塞式服务，虚拟线程值得评估。
- 使用虚拟线程时，不要把数据库连接池、HTTP 连接池也无限放大。
- ThreadLocal 在虚拟线程场景下仍能用，但要注意上下文传播和内存占用。
- synchronized 阻塞、native 调用等场景可能影响虚拟线程调度效果。
- 预览特性不建议在生产核心代码中直接依赖，除非团队明确接受升级成本。

### 常见问题

- 误以为虚拟线程可以替代限流、连接池和容量规划。
- 把 CPU 密集任务也大量丢给虚拟线程，结果没有性能收益。
- 老版本框架或 Agent 对虚拟线程支持不完整，监控和链路追踪显示异常。
- 使用 ThreadLocal 保存大对象，虚拟线程数量上来后内存压力变大。
- 没有区分“提升并发承载能力”和“提升单个请求执行速度”。

## 四、最新版 JDK 26：关注语言、运行时和平台继续演进

JDK 26 是当前需要关注的最新版本线。它的意义不一定是马上用于生产，而是观察 Java 未来方向：语言模式匹配继续增强，HTTP/3、AOT、GC、结构化并发、Vector API 等继续推进。

### 主要特性方向

- Prepare to Make Final Mean Final：强化 `final` 语义的未来准备。
- Remove the Applet API：移除 Applet API，继续清理历史包袱。
- Ahead-of-Time Object Caching with Any GC：AOT 对象缓存能力扩展到任意 GC。
- HTTP/3 for the HTTP Client API：标准 HTTP Client API 支持 HTTP/3。
- G1 GC: Improve Throughput by Reducing Synchronization：通过减少同步改进 G1 吞吐。
- PEM Encodings of Cryptographic Objects：加密对象 PEM 编码支持继续预览。
- Structured Concurrency：结构化并发继续预览。
- Lazy Constants：惰性常量继续预览。
- Vector API：继续孵化，面向 SIMD/向量化计算场景。
- Primitive Types in Patterns, `instanceof`, and `switch`：基本类型参与模式匹配继续预览。

### 重点理解

JDK 26 的重点不是“所有项目马上升级”，而是看清 Java 的演进方向：

- 网络：HTTP Client 继续向 HTTP/3 演进。
- 并发：结构化并发继续完善，配合虚拟线程形成更清晰的并发模型。
- 性能：AOT、G1、Vector API 继续改进启动、吞吐和计算能力。
- 语言：模式匹配继续扩展，目标是减少样板代码，提高类型判断表达力。
- 安全与历史清理：Applet 等老 API 被移除，旧技术债继续减少。

### 生产环境是否建议直接用最新版

一般不建议只因为“版本最新”就直接上生产。更稳的选择是：

- 存量老项目：先从 JDK 8 评估升级到 17 或 21。
- 新项目：优先考虑 JDK 21 LTS。
- 对虚拟线程有明确收益的服务：重点评估 JDK 21。
- 对最新语言/性能特性感兴趣：可以在测试环境或工具项目中验证 JDK 26。
- 对稳定性、供应商支持、合规要求高的系统：优先选择 LTS 和长期支持发行版。

## 五、版本对比速查

| 版本 | 定位 | 最值得关注的变化 | 项目建议 |
| --- | --- | --- | --- |
| JDK 8 | 经典存量版本 | Lambda、Stream、Optional、java.time、接口默认方法 | 老项目常见，但新项目不建议继续以 8 为基线 |
| JDK 17 | 现代 LTS 基线 | Record、Text Blocks、Sealed Classes、模式匹配、强封装 | 适合从 8/11 升级的稳妥目标 |
| JDK 21 | 新一代 LTS | Virtual Threads、Sequenced Collections、Record Patterns、Generational ZGC | 新项目优先考虑，尤其适合服务端应用 |
| JDK 26 | 最新版本线 | HTTP/3、AOT 对象缓存、G1 改进、结构化并发预览、Vector API | 适合跟踪新特性，生产采用要看支持周期 |

## 六、升级路线建议

### 从 JDK 8 升级

推荐路线：

```text
JDK 8 -> JDK 17 -> JDK 21
```

不建议在大型老项目中直接从 8 跳到最新版本，除非测试覆盖、依赖治理和发布回滚都很成熟。

### 升级前检查清单

- 检查 Maven/Gradle 插件版本。
- 检查 Spring、MyBatis、Hibernate、Lombok、Mockito、ByteBuddy、ASM 等依赖兼容性。
- 检查 Docker 基础镜像和 CI 构建镜像。
- 检查 JVM 启动参数，删除废弃参数。
- 检查非法反射访问和 JDK 内部 API 使用。
- 检查字符集、时区、TLS、安全策略变化。
- 做单元测试、集成测试、压测和灰度发布。
- 对比 GC 日志、接口延迟、内存占用和启动时间。

## 七、面试回答思路

如果被问“JDK 8、17、21 有什么区别”，可以这样答：

```text
JDK 8 是 Java 现代语法的起点，引入 Lambda、Stream、Optional 和 java.time；
JDK 17 是现代 LTS 基线，包含 record、sealed class、text block、模式匹配等，并且 JDK 内部 API 强封装更严格；
JDK 21 是新一代 LTS，最重要的是虚拟线程正式可用，同时有 Sequenced Collections、Record Patterns、Pattern Matching for switch 和 Generational ZGC；
最新版 JDK 26 继续推进 HTTP/3、AOT、G1、结构化并发、Vector API 和模式匹配等方向。
生产选型不只看新特性，还要看 LTS、依赖兼容性、运行稳定性和厂商支持周期。
```

## 八、常见误区

- 只关注语法新特性，忽略依赖、构建、镜像、Agent 和运行参数。
- 认为高版本一定更快，实际性能要看业务模型、GC、依赖和配置。
- 虚拟线程和响应式编程混为一谈，二者解决问题的方式不同。
- 使用预览特性写核心业务代码，后续升级时语法或 API 可能变化。
- 升级 JDK 时没有同步升级测试框架和字节码增强工具。
- 没有保留回滚方案，升级失败后影响发布节奏。

## 参考资料

- OpenJDK JDK 17: https://openjdk.org/projects/jdk/17/
- OpenJDK JDK 21: https://openjdk.org/projects/jdk/21/
- OpenJDK JDK 26: https://openjdk.org/projects/jdk/26/
- Oracle JDK 8 What's New: https://www.oracle.com/java/technologies/javase/8-whats-new.html
- Oracle Java 8 Lambda and Streams API notes: https://docs.oracle.com/javase/8/docs/technotes/guides/language/lambda_api_jdk8.html
