# springboot运行/打包时动态指定配置文件application-*.yml

## 一、 resources下新建application.yml、application-dev.yml、application-test.yml、application-pro.yml

* application.yml 项目统一配置，不管是开发环境、测试环境或生产环境，都无需更改的配置,如Zipkin配置，mybatis配置、shiro配置、jwt配置等

* application-dev.yml 开发环境，配置数据库连接、redis连接、ftp、fastdfs等等

* application-test.yml 测试环境，配置数据库连接、redis连接、ftp、fastdfs等等

* application-pro.yml 生产环境，配置数据库连接、redis连接、ftp、fastdfs等等

![image](https://user-images.githubusercontent.com/64882640/124584317-ab8c1380-de86-11eb-9df4-d087264af4a9.png)

## 二、 在web项目的pom.xml文件里，增加如下配置

```
<profiles>
    <!--开发环境 -->
    <profile>
      <id>dev</id>
      <properties>
        <profiles.active>dev</profiles.active>
      </properties>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
    </profile>
    <!--测试环境 -->
    <profile>
      <id>test</id>
      <properties>
        <profiles.active>test</profiles.active>
      </properties>
    </profile>
    <!--生产环境 -->
    <profile>
      <id>pro</id>
      <properties>
        <profiles.active>pro</profiles.active>
      </properties>
    </profile>
  </profiles>
```

## 三、 在web项目的pom.xml文件build节点下，增加如下配置
```
<build>
   <resources>
            <resource>
                <directory>src/main/resources</directory>
                <!--用于替换resources里的变量-->
                <filtering>true</filtering>
            </resource>
        </resources>
</build>

```

## 四、在application.yml里增加如下配置
```
spring:
  zipkin:
    enabled: false
    base-url: http://localhost:9411
  profiles:
#    active: @profiles.active@
    active: dev
```

* 其中spring.profiles.active：代表当前激活的配置文件，根据上面的配置，会出现三个值：dev、test和pro，在运行/打包时会自动将@profiles.active@替换为这三个值
* 默认是dev,开发时可直接写死dev（不会替换）

## 五、附-application.yml配置
```
spring.application.name: photovoltaic-weixin-service
server.port: 10113

# Sleuth Zipkin Settings
spring.sleuth.sampler.probability: 100
spring:
  zipkin:
    enabled: false
    base-url: http://localhost:9411
  profiles:
#    active: @profiles.active@
    active: dev
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss #jackson对响应回去的日期参数进行格式化
    time-zone: GMT+8
    serialization:
      write-dates-as-timestamps: false
# mybatis plus配置
mybatis-plus:
  configuration:
    map-underscore-to-camel-case: true
    auto-mapping-behavior: full
#    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  mapper-locations: classpath*:mapper/*Mapper.xml
  global-config:
    # 逻辑删除配置
    db-config:
      # 删除前
      logic-not-delete-value: 1
      # 删除后
      logic-delete-value: 0
# Actuator Settings
management:
  endpoints:
    web:
      exposure:
        include: health
  endpoint:
    health:
      show-details: always

```

## 六、附-application-dev.yml配置
```
spring:
  # Database settings
  datasource:
    url: jdbc:mysql://${mysql.host}/${mysql.database}?characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: ${mysql.user.name}
    password: ${mysql.user.password}
  redis:
    # redis数据库索引(默认为0)，我们使用索引为3的数据库，避免和其他数据库冲突
    database: 0
    # redis服务器地址（默认为loaclhost）
    host: 192.168.199.160
    # redis端口（默认为6379）
    port: 6379
    # redis访问密码（默认为空）
    password:
    # redis连接超时时间（单位毫秒）
    timeout: 0
    # redis连接池配置
    pool:
      # 最大可用连接数（默认为8，负数表示无限）
      max-active: 100
      # 最大空闲连接数（默认为8，负数表示无限）
      max-idle: 20
      # 最小空闲连接数（默认为0，该值只有为正数才有用）
      min-idle: 5
      # 从连接池中获取连接最大等待时间（默认为-1，单位为毫秒，负数表示无限）
      max-wait: -1

# MySQL Settings
mysql:
  host: 192.168.199.150:3306
  database: pro_photovoltaic
  user:
    name: root
    password: Abc123456#


```
