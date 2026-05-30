# Spring IoC、AOP与SpringMVC

## 概念

Spring 的核心是 IoC 和 AOP。IoC 把对象创建和依赖管理交给容器，常见方式是构造器注入、setter 注入和字段注入；AOP 用切面封装横切逻辑，例如日志、权限、事务。

SpringMVC 是 Web MVC 框架，核心流程是请求进入 DispatcherServlet，通过 HandlerMapping 找到 Controller，再由 HandlerAdapter 调用方法，最终返回 ModelAndView 或 JSON 响应。

## 学习重点

- 先理解核心模型，再看配置和扩展点。
- 区分框架默认行为和项目自定义行为。
- 排查问题时从日志、配置、依赖版本和运行环境入手。

## 使用场景

- 面试复习时，用于梳理概念、边界和常见追问。
- 项目开发时，用于判断技术选型、代码写法和排查方向。
- 线上问题处理时，用于快速定位相关模块和可能风险。

## 示例

```java
@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

## 常见问题

- 字段注入导致依赖不清晰，测试和构造对象困难。
- 切面顺序不明确，日志、权限、事务执行顺序和预期不一致。
- Controller 承担过多业务逻辑，导致 Web 层和业务层耦合。

## 总结

Spring 的核心价值是管理对象关系和横切逻辑；SpringMVC 负责请求分发，业务复杂度应沉到 service 层并保持边界清晰。
