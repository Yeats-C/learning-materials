## 一 Java String 类----String字符串常量
String类型和StringBuffer主要性能区别主要在于**String是不可变的对象，因此在每次对String类型进行改变的时候其实都等同于生成了一个新的String对象，
然后将指针指向新的String对象，这样不仅效率低下，而且大量的浪费有限的内存空间，所以经常改变的字符串最好不要用String。**每次生成对象都会对系统性能产生影响，
特别是内存中无引用对象多了以后，还会引起JVM的GC开始工作。
![image](https://github.com/Yeats-C/learning-materials/blob/master/image/20180411091757991.png)

## 二 StringBuffer和StringBuilder类----StringBuffer、StringBuilder字符串变量
**StringBuffer 字符串变量（线程安全）**
**StringBuilder 字符串变量（非线程安全）**

StringBuffer和StringBuilder类的对象能被多次修改，并且不产生新的对象，当会对字符串进行修改时，特别的字符串经常改变时，推荐使用。
