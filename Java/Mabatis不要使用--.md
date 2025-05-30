# Mabatis不要使用--

最近运维生产反应在生产环境出现了异常日志

java.sql.SQLException: Parameter index out of range (1 > number of parameters, which is 0).

该异常表明MyBatis在解析SQL语句时尝试绑定一个不存在的参数。

代码上下文分析：

-- and business_date_str = #{businessDateStr}

分析原因：

* 使用--注释动态sql或包含#{XXX}的内容可能会导致以下后果：

* 参数仍被绑定： MyBatis不会忽略--注释中的#{XXX}表达式，仍尝试绑定参数

* SQL语法错误： 如果注释中还包含未生效的 AND 或 WHERE ,可能产生非法SQL

* 调试困难： 日志中无法直观看到注释影响，排查复杂


## 正确做法

## 不要在MyBatis中使用--注释符，不要在MyBatis中使用--注释符，可直接删除此行sql，或使用注释 <!-- --> 来包裹含 #{xxx} 的代码片段

这样可以确保：

整个代码块被 XML 解析器完全忽略；

不会触发 MyBatis 对参数的绑定行为；

安全且易于维护；
