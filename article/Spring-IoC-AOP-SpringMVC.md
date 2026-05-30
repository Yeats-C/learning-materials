# Spring IoC、AOP 与 SpringMVC 指的是什么

## 📌 [Spring IoC](ca://s?q=Spring_IoC是什么)
- **IoC（Inversion of Control，控制反转）** 是 Spring 的核心思想之一。  
- 通过 **依赖注入（Dependency Injection, DI）** 将对象的创建与依赖关系交由容器管理，而不是由代码主动控制。  
- 特点：  
  - 降低耦合度。  
  - 提高代码的可维护性与可测试性。  
  - 由 Spring 容器负责对象的生命周期与依赖管理。  

---

## 📌 [Spring AOP](ca://s?q=Spring_AOP是什么)
- **AOP（Aspect-Oriented Programming，面向切面编程）** 是一种补充 OOP 的编程思想。  
- 在不修改业务逻辑代码的情况下，通过切面实现横切关注点（如日志、事务、安全）。  
- 特点：  
  - 使用 **切点（Pointcut）** 定义拦截位置。  
  - 使用 **通知（Advice）** 定义增强逻辑。  
  - 常见应用：日志记录、性能监控、事务管理。  

---

## 📌 [SpringMVC](ca://s?q=SpringMVC是什么)
- **SpringMVC** 是 Spring 框架中的 **Web 层框架**，基于 MVC 模式实现。  
- 特点：  
  - **Model**：封装数据与业务逻辑。  
  - **View**：展示数据（通常是 JSP、Thymeleaf）。  
  - **Controller**：处理请求，调用业务逻辑并返回结果。  
- 通过 **DispatcherServlet** 作为前端控制器，统一分发请求。  
- 支持 RESTful 风格接口，常用于 Web 应用开发。  

---

## 🔎 核心特点对比

| 概念 | **作用** | **特点** | **应用场景** |
|------|----------|----------|--------------|
| **IoC** | 控制反转，依赖注入 | 降低耦合度，容器管理对象 | Bean 管理、依赖注入 |
| **AOP** | 面向切面编程 | 横切关注点，增强逻辑 | 日志、事务、安全 |
| **SpringMVC** | Web 层框架 | 基于 MVC 模式，前端控制器 | Web 应用、REST API |

---

## 💡 总结
- **Spring IoC**：解决对象依赖管理问题，提升可维护性。  
- **Spring AOP**：解决横切逻辑问题，提升代码复用性与可扩展性。  
- **SpringMVC**：解决 Web 层请求处理问题，提供 MVC 架构支持。  
- 三者结合构成了 **Spring 框架的核心体系**，支持从底层依赖管理到 Web 层开发的完整解决方案。  
