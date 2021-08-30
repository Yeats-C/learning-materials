# BigDecimal
在项目中经常会用到小数的一些计算，而float和double类型的主要设计目标是为了科学计算和工程计算。他们执行二进制浮点运算，这是为了在广域数值范围上提供较为精确的快速近似计算而精心设计的。然而，它们没有提供完全精确的结果，所以不应该被用于要求精确结果的场合。但是，商业计算往往要求结果精确。所以有时候必须要采用BigDecimal。


BigDecimal.setScale()方法用于格式化小数点

setScale(1)表示保留一位小数，默认用四舍五入方式 

setScale(1,BigDecimal.ROUND_DOWN)直接删除多余的小数位，如2.35会变成2.3 

setScale(1,BigDecimal.ROUND_UP)进位处理，2.35变成2.4 

setScale(1,BigDecimal.ROUND_HALF_UP)四舍五入，2.35变成2.4

setScaler(1,BigDecimal.ROUND_HALF_DOWN)四舍五入，2.35变成2.3，如果是5则向下舍

setScaler(1,BigDecimal.ROUND_CEILING)接近正无穷大的舍入

setScaler(1,BigDecimal.ROUND_FLOOR)接近负无穷大的舍入，数字>0和ROUND_UP作用一样，数字<0和ROUND_DOWN作用一样

setScaler(1,BigDecimal.ROUND_HALF_EVEN)向最接近的数字舍入，如果与两个相邻数字的距离相等，则向相邻的偶数舍入。






## java.lang.ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result

今天在写一个JAVA程序的时候出现了异常：java.lang.ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result。

报错语句为：
```
foo.divide(bar));
```

原因：
原来JAVA中如果用BigDecimal做除法的时候一定要在divide方法中传递第二个参数，定义精确到小数点后几位，否则在不整除的情况下，结果是无限循环小数时，就会抛出以上异常。

解决：
```
foo.divide(bar, 2, BigDecimal.ROUND_HALF_UP);
```
