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


## 2.部署Saturn Executor

## 2.1 配置环境变量

```
vim /etc/profile
```

![image](https://user-images.githubusercontent.com/64882640/156317812-b7be6ddf-5c5c-4a1f-860e-1843f3316150.png)

进行了上述的配置后，记得刷新配置文件，使配置立即生效。

```
source /etc/profile
```

## 2.2 准备 executor

* 从https://github.com/vipshop/Saturn/releases 中点击链接获取最新版本的'Executor Zip File'，
* 将得到一个saturn-executor-{version}-zip.zip的文件。将其上传并把它解压到一个指定的目录。

![image](https://user-images.githubusercontent.com/64882640/156318401-f8213735-87c9-482e-a3c7-f99f92f5d0ca.png)

![image](https://user-images.githubusercontent.com/64882640/156318432-6a5f6385-06ac-4741-b1b6-dafd11040691.png)

* 注：saturn-executor-3.1.0 里面包含了Saturn任务启动文件，可以把它理解为一个启动器。

## 2.3 部署 java 作业

将开发并打包好的**-app.zip在/saturn-executor-{version}同一级目录进行解压。

* 注：解压后会多出一个 app 的多级目录，我们之前如果有app这个目录，请注意覆盖。

![image](https://user-images.githubusercontent.com/64882640/156318596-c4e15e26-3f32-447c-aaaf-b3da60353cf9.png)

Executor启动时会扫描这个app目录，并加载这个目录下（含子目录)所有的jar包。

![image](https://user-images.githubusercontent.com/64882640/156318655-a1e50320-f6ea-42d9-98a4-90954c82c2c2.png)

## 2.4 启动 executor

![image](https://user-images.githubusercontent.com/64882640/156318918-15f81b5f-f8f2-4840-a875-383f799c30c1.png)


## 3.在Console添加Java作业

## 3.1 配置作业

当IDE启动了Executor后，作业还是不能执行，直到在Console添加和启动相应的Java作业。

在Console添加一个Java作业，作业实现类必须是你所实现的Java作业的className。

![image](https://user-images.githubusercontent.com/64882640/156319457-4d16e5e4-0096-488b-965d-2009f578cc2f.png)

作业类型： 分为Java定时作业和Shell定时作业，这里选择Java定时作业
作业名：作业ID标识，namespace下必须唯一
作业实现类：作业实现类的完整包名+类名
cron表达式：作业定时表达式
作业分片总数：表示并发执行的数量，2代表该作业同时有两个进程在并发执行，每个进程都有自己专门的脚本和参数(这些进程可能同跑在不同机器上的)。
分片序列号/参数对照表：定义每个分片执行的完整脚本路径及参数。这是saturn最重要的参数之一。分片号从0开始，最大为分片总数-1。
作业描述信息 (Optional)：作业描述

## 3.2 启动这个作业

![image](https://user-images.githubusercontent.com/64882640/156319533-25989443-97d8-4127-9e19-cf894ba991ec.png)

如果一切正常会在IDE的console看到作业运行的日志，也可以在“分片”标签看到执行的结果。（当然，前提是作业到点执行了）

![image](https://user-images.githubusercontent.com/64882640/156319622-5cca4da3-9c1f-41d0-8920-42267133d406.png)

