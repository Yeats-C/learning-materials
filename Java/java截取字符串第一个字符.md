# java 截取字符串第一个字符

## 使用substring() 方法返回字符串的子字符串。

* 1、语法：

```
（1）public String substring(int beginIndex)。

（2）public String substring(int beginIndex, int endIndex)。
```

* 2、参数：
```
（1）beginIndex -- 起始索引（包括）, 索引从 0 开始。

（2）endIndex -- 结束索引（不包括）。
```

*3、返回值：
```
返回一个新字符串，它是此字符串的一个子字符串。该子字符串从指定的 beginIndex 处开始，一直到索引 endIndex - 1处的字符。因此，该子字符串的长度为 endIndex-beginIndex。
```

*4、substring函数存在的抛出错误：
```
IndexOutOfBoundsException - 如果 beginIndex 为负，或 endIndex 大于此 String 对象的长度，或 beginIndex 大于 endIndex。
```


