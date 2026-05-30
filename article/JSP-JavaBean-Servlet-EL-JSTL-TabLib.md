# JSP、JavaBean、Servlet、EL、JSTL、TagLib

## 📌 [JSP](ca://s?q=JSP是什么)
- 全称 **JavaServer Pages**。  
- 一种基于 Java 的动态网页技术，可以在 HTML 中嵌入 Java 代码。  
- 常用于生成动态内容，结合 Servlet 一起工作。

## 📌 [JavaBean](ca://s?q=JavaBean是什么)
- 一种符合特定规范的 Java 类（需有无参构造方法、属性私有、提供 getter/setter）。  
- 用来封装数据和业务逻辑，常作为 JSP 与 Servlet 之间的数据载体。  
- 在 MVC 模式中通常充当 **Model**。

## 📌 [Servlet](ca://s?q=Servlet是什么)
- 一种运行在服务器端的 Java 程序，用来处理客户端请求并生成响应。  
- 是 JSP 的底层支持技术，JSP 最终会被编译成 Servlet。  
- 在 MVC 模式中通常充当 **Controller**。

## 📌 [EL 表达式](ca://s?q=EL表达式是什么)
- 全称 **Expression Language**。  
- 用于在 JSP 页面中简化数据访问，替代复杂的 Java 代码。  
- 例如：`${user.name}` 可以直接访问作用域中的对象属性。

## 📌 [JSTL](ca://s?q=JSTL是什么)
- 全称 **JavaServer Pages Standard Tag Library**。  
- 提供一组标准标签库，用来简化 JSP 页面开发。  
- 包含核心标签（流程控制）、格式化标签、SQL 标签、XML 标签等。  
- 例如 `<c:forEach>` 用来遍历集合。

## 📌 [TagLib](ca://s?q=TagLib是什么)
- 全称 **Tag Library**，即标签库。  
- JSP 中的扩展机制，可以自定义标签来封装复杂逻辑。  
- JSTL 就是一个标准的 TagLib，而开发者也可以编写自定义标签库。

---

## 💡 总结
- **JSP** → 动态网页技术。  
- **JavaBean** → 封装数据与逻辑的组件。  
- **Servlet** → 处理请求和响应的服务器端程序。  
- **EL** → 简化 JSP 中的数据访问。  
- **JSTL** → 标准标签库，简化 JSP 开发。  
- **TagLib** → 标签库机制，支持自定义标签。  

它们共同构成了 **Java Web 开发的基础技术体系**，在 MVC 模式中分别承担不同角色，帮助开发者快速构建动态 Web 应用。
