# Java 中 List 分片的 5 种方法

* MySQL 只能执行一定长度的 SQL 语句，但当插入的数据量较多时，会生成一条很长的 SQL，这样程序在执行时就会报错。

* 要解决这个问题，有两种方法：第一，设置 MySQL 可以执行 SQL 的最大长度；第二，将一个大 List 分成 N 个小 List 进行。由于无法准确的界定程序中最大的 SQL 长度，所以最优的解决方案还是第二种。


将一个 List 分成多个小 List 的过程，我们称之为分片，当然也可以叫做“List 分隔”


## 1.Google Guava

```
<!-- google guava 工具类 -->
<!-- https://mvnrepository.com/artifact/com.google.guava/guava -->
<dependency>
  <groupId>com.google.guava</groupId>
  <artifactId>guava</artifactId>
  <version>31.0.1-jre</version>
</dependency>
```
有了 Guava 框架之后，只需要使用 Lists.partition 方法即可实现分片，如下代码所示：
```
import com.google.common.collect.Lists;

import java.util.Arrays;
import java.util.List;

/**
 * Guava 分片
 */
public class PartitionByGuavaExample {
    // 原集合
    private static final List<String> OLD_LIST = Arrays.asList(
            "唐僧,悟空,八戒,沙僧,曹操,刘备,孙权".split(","));

    public static void main(String[] args) {
        // 集合分片
        List<List<String>> newList = Lists.partition(OLD_LIST, 3);
        // 打印分片集合
        newList.forEach(i -> {
            System.out.println("集合长度：" + i.size());
        });
    }
}
```
以上代码的执行结果如下图所示：
![image](https://user-images.githubusercontent.com/64882640/210506525-82a273d6-f4ea-4ae8-8f7d-1614ea5e16ce.png)


## 2.apache commons
先在项目的 pom.xml 中添加框架支持，增加以下配置

```
<!-- apache 集合工具类 -->
<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-collections4 -->
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-collections4</artifactId>
  <version>4.4</version>
</dependency>
```

有了 commons 框架之后，只需要使用 ListUtils.partition 方法即可实现分片，如下代码所示：

```
import org.apache.commons.collections4.ListUtils;

import java.util.Arrays;
import java.util.List;

/**
 * commons.collections4 集合分片
 */
public class PartitionExample {
    // 原集合
    private static final List<String> OLD_LIST = Arrays.asList(
            "唐僧,悟空,八戒,沙僧,曹操,刘备,孙权".split(","));

    public static void main(String[] args) {
        // 集合分片
        List<List<String>> newList = ListUtils.partition(OLD_LIST, 3);
        newList.forEach(i -> {
            System.out.println("集合长度：" + i.size());
        });
    }
}
```

以上代码的执行结果如下图所示：
![image](https://user-images.githubusercontent.com/64882640/210506794-52cf9a6b-c3d4-4807-be99-8f0925fd031a.png)


## 3.Hutool

先在项目的 pom.xml 中添加框架支持，增加以下配置：

```
<!-- 工具类 hutool -->
<!-- https://mvnrepository.com/artifact/cn.hutool/hutool-all -->
<dependency>
  <groupId>cn.hutool</groupId>
  <artifactId>hutool-all</artifactId>
  <version>5.7.14</version>
</dependency>
```

有了 Hutool 框架之后，只需要使用 ListUtil.partition 方法即可实现分片，如下代码所示：

```
import cn.hutool.core.collection.ListUtil;

import java.util.Arrays;
import java.util.List;

public class PartitionByHutoolExample {
    // 原集合
    private static final List<String> OLD_LIST = Arrays.asList(
            "唐僧,悟空,八戒,沙僧,曹操,刘备,孙权".split(","));

    public static void main(String[] args) {
        // 分片处理
        List<List<String>> newList = ListUtil.partition(OLD_LIST, 3);
        newList.forEach(i -> {
            System.out.println("集合长度：" + i.size());
        });
    }
}
```

以上代码的执行结果如下图所示：
![image](https://user-images.githubusercontent.com/64882640/210506958-c845c543-b2a5-4483-b9bf-455a62ddaf12.png)


## 4.JDK

Stream 通过 JDK 8 中的 Stream 来实现分片就无需添加任何框架了，具体的实现代码如下：

```
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

/**
 * JDK Stream Partition
 */
public class PartitionByStreamExample {
    // 原集合
    private static final List<Integer> OLD_LIST = Arrays.asList(
            1, 2, 3, 4, 5, 6);

    public static void main(String[] args) {
        // 集合分片：将大于 3 和小于等于 3 的数据分别分为两组
        Map<Boolean, List<Integer>> newMap = OLD_LIST.stream().collect(
                Collectors.partitioningBy(i -> i > 3)
        );
        // 打印结果
        System.out.println(newMap);
    }
}
```

以上代码的执行结果如下图所示：
![image](https://user-images.githubusercontent.com/64882640/210507178-88b297cb-a369-45f8-8d9d-4d422ddc3f5c.png)

此方式的优点的无需添加任何框架，但缺点是只能实现简单的分片（将一个 List 分为两个），并且要有明确的分片条件。比如本篇案例中设置的分片条件就是数组是否大于 3，如果大于 3 就会被归为一组，否则就会被分到另一组。

## 5.自定义分片

如果你不想引入第三方框架，并且使用 Stream 也无法满足你的需求，你就可以考虑自己写代码来实现分片功能了。因为此方式不常用，所以咱们这里只给出关键方法。

自定义分片功能的关键实现方法是 JDK 自带的 subList 方法，如下图所示

![image](https://user-images.githubusercontent.com/64882640/210507272-7bc3d6ce-5ec0-4276-b159-81fb506640bf.png)

使用示例如下：

```
import java.util.Arrays;
import java.util.List;

public class App {
    private static final List<String> _OLD_LIST = Arrays.asList(
            "唐僧,悟空,八戒,沙僧,曹操,刘备,孙权".split(","));

    public static void main(String[] args) {
        // 集合分隔
        List<String> list = _OLD_LIST.subList(0, 3);
        // 打印集合中的元素
        list.forEach(i -> {
            System.out.println(i);
        });
    }
}
```

以上代码的执行结果如下图所示：

![image](https://user-images.githubusercontent.com/64882640/210507334-56f65247-8efe-46bd-8f42-195e0ddeaa4f.png)

## 总结
本文介绍了 5 种 List 分片的实现方法，其中最方便的实现方式是引入第三方框架，比如 Google 的 Guava、Apache 的 Commons 或者是国产开源的 Hutool 都可以，当然如果你的项目已经包含了以上任意一种，直接使用就行了。如果是简单的分片就可以考虑使用 JDK 的 Stream 或者是 List 内置的 subList 方法来实现分片功能了。


[参考链接](https://zhuanlan.zhihu.com/p/428268862)
