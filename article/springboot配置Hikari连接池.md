# springboot配置Hikari连接池

* 在springboot2.0以后采用的默认数据库连接池就是hikari，而且不需要引入依赖包含在springboot中；

```
spring:
  # Database settings
  datasource:
    url: jdbc:mysql://${mysql.host}/${mysql.database}?characterEncoding=utf-8&serverTimezone=Asia/Shanghai&allowMultiQueries=true
    username: ${mysql.user.name}
    password: ${mysql.user.password}
    # 连接池
    hikari:
      #连接池名
      pool-name: ${mysql.hikari.pool-name}
      #最小空闲连接数
      minimum-idle: ${mysql.hikari.minimum-idle}
      # 空闲连接存活最大时间，默认600000（10分钟）
      idle-timeout: ${mysql.hikari.idle-timeout}
      # 连接池最大连接数，默认是10
      maximum-pool-size: ${mysql.hikari.maximum-pool-size}
      # 此属性控制从池返回的连接的默认自动提交行为,默认值：true
      auto-commit: ${mysql.hikari.auto-commit}
      # 此属性控制池中连接的最长生命周期，值0表示无限生命周期，默认1800000即30分钟
      max-lifetime: ${mysql.hikari.max-lifetime}
      # 数据库连接超时时间,默认30秒，即30000
      connection-timeout: ${mysql.hikari.connection-timeout}
      connection-test-query: ${mysql.hikari.connection-test-query}
```

![image](https://user-images.githubusercontent.com/64882640/190844389-3f3425c9-07e3-485b-87d6-9e37ffce418a.png)
