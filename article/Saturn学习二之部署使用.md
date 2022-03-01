# Saturn学习二之部署使用

## 名词解释

* 域 ： 一个项目（可包含多个服务）
* 作业 ： 一个服务
* Executor ：  执行器

## 1.开发Java作业

## 1.1 添加maven依赖

* 在pom.xml添加dependency

```
<dependency>
    <groupId>com.vip.saturn</groupId>
    <artifactId>saturn-job-api</artifactId>
    <!-- 修改成指定版本 -->
    <version>master-SNAPSHOT</version>
</dependency>  
```

* 以及plugin

```
<plugin>
  <groupId>com.vip.saturn</groupId>
  <artifactId>saturn-plugin</artifactId>
  <!-- 版本与saturn-job-api一致 -->
  <version>master-SNAPSHOT</version>
</plugin>
```

## 1.2 开发第一个Java作业

* 修改现在类或者增加一个新的类，继承自AbstractSaturnJavaJob ，实现 handleJavaJob方法。

```
 public class DemoJob extends AbstractSaturnJavaJob {

        @Override
        public SaturnJobReturn handleJavaJob(final String jobName, final Integer shardItem, final String shardParam, final SaturnJobExecutionContext context) {
        // do what you want here ...
              
        // 返回一个SaturnJobReturn对象，默认返回码是200表示正常的返回
                    return new SaturnJobReturn("我是分片"+shardItem+"的处理结果");
        }
    }
```

handleJavaJob方法是作业调用主入口，当调度周期到达时，Saturn会调用该方法。

传入参数如下：

* jobName：作业名
* shardItem：分片编号（从0开始）分片参数（在Console配置)
* shardParam：分片参数（在Console配置）
* context：调用上下文

### 关于JavaJobReturn

JavaJobReturn是作业结果返回的封装。里面三个成员变量，包括：

* returnCode: 结果码。0代表结果成功，其余值代表失败。默认为0。用户可以根据自己业务的情况设置返回值，但注意，如下返回码是保留字不能使用，包括：0，1，2，9999。
* returnMsg：返回信息。将显示在Console里面。没有默认值。
* errorGroup：异常码。详情参见教程。

