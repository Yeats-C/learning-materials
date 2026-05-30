# Hibernate 和 MyBatis

## 📌 [Hibernate](ca://s?q=Hibernate是什么)
- Hibernate 是一个 **开源的对象关系映射（ORM）框架**。  
- 它通过 **映射 Java 类与数据库表**，让开发者用面向对象的方式操作数据库，而不需要直接编写 SQL。  
- 特点：  
  - **自动生成 SQL**：开发者只需操作对象，Hibernate 会自动生成对应的 SQL。  
  - **缓存机制**：支持一级、二级缓存，提高查询性能。  
  - **事务管理**：与 JTA、JDBC 集成，支持声明式事务。  
  - **跨数据库支持**：通过方言（Dialect）适配不同数据库。  

---

## 📌 [MyBatis](ca://s?q=MyBatis是什么)
- MyBatis 是一个 **半自动化的持久层框架**。  
- 它通过 XML 或注解配置，将 SQL 与 Java 方法绑定，开发者可以直接编写 SQL 并映射到对象。  
- 特点：  
  - **灵活的 SQL 控制**：开发者完全掌握 SQL，适合复杂查询。  
  - **轻量级**：相比 Hibernate，更简单易用，学习成本低。  
  - **映射机制**：支持结果集与对象属性的自动映射。  
  - **集成方便**：常与 Spring 框架结合使用。  

---

## 🔎 核心区别

| 特性 | **[Hibernate](ca://s?q=Hibernate特点)** | **[MyBatis](ca://s?q=MyBatis特点)** |
|------|--------------------------------|--------------------------------|
| **SQL 控制** | 自动生成 SQL，开发者不需关心细节 | 手写 SQL，灵活可控 |
| **学习成本** | 学习曲线较高，需理解 ORM 思想 | 学习成本低，SQL 即可 |
| **性能优化** | 内置缓存机制，适合通用场景 | 性能依赖 SQL 优化，开发者掌控 |
| **适用场景** | 适合数据结构稳定、业务逻辑复杂的系统 | 适合查询复杂、对 SQL 控制要求高的系统 |

---

## 📊 使用示例

### Hibernate
```java
// 定义实体类
@Entity
public class User {
    @Id
    private Long id;
    private String name;
}

// 保存对象
User user = new User();
user.setId(1L);
user.setName("Alice");
session.save(user);  // Hibernate 自动生成 SQL
```
### MyBatis
```xml
<!-- Mapper 配置 -->
<select id="getUser" resultType="User">
  SELECT id, name FROM user WHERE id = #{id}
</select>
```

```java
User user = userMapper.getUser(1);  // 手写 SQL，结果映射到对象
```

## 📊 总结
Hibernate：全自动 ORM，适合对象化开发，减少 SQL 编写，但学习成本高。

MyBatis：半自动 ORM，SQL 灵活可控，适合复杂查询和对性能要求高的场景。
