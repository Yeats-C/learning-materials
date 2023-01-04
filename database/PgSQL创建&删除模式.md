# PostgreSQL 模式（SCHEMA）

PostgreSQL 模式（SCHEMA）可以看着是一个表的集合。
一个模式可以包含视图、索引、数据类型、函数和操作符等。
相同的对象名称可以被用于不同的模式中而不会出现冲突，例如 schema1 和 myschema 都可以包含名为 mytable 的表。
使用模式的优势：

* 允许多个用户使用一个数据库并且不会互相干扰。
* 将数据库对象组织成逻辑组以便更容易管理。
* 第三方应用的对象可以放在独立的模式中，这样它们就不会与其他对象的名称发生冲突。

模式类似于操作系统层的目录，但是模式不能嵌套。

## 创建模式

```
//语法格式如下：
CREATE SCHEMA myschema.mytable (
...
);
```
![image](https://user-images.githubusercontent.com/64882640/210508091-8cb77f29-6231-408e-8032-ed49c2af63bb.png)

## 删除模式

```
//删除一个为空的模式（其中的所有对象已经被删除）：
DROP SCHEMA myschema;
//删除一个模式以及其中包含的所有对象：
DROP SCHEMA myschema CASCADE;
```

![image](https://user-images.githubusercontent.com/64882640/210508170-0e2454d1-b78d-4d33-8b17-5291b9379979.png)


[参考链接](https://blog.csdn.net/qq_41361442/article/details/124784208)
