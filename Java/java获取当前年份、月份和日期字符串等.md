# java获取当前年份、月份和日期字符串等

## Java获取当前年份、月份和日期是通过Calendar类的实例对象来获取的。

* 首先创建一个Calendar类的实例对象，Calendar类属于java.util包。

```
Calendar calendar = Calendar.getInstance();
```

* 获取当前年份、月份和日期等。

```
// 获取当前年
int year = calendar.get(Calendar.YEAR);  
// 获取当前月
int month = calendar.get(Calendar.MONTH) + 1;  
// 获取当前日
int day = calendar.get(Calendar.DATE);  
// 获取当前小时
int hour = calendar.get(Calendar.HOUR_OF_DAY);  
// 获取当前分钟
int minute = calendar.get(Calendar.MINUTE);  
// 获取当前秒
int second = calendar.get(Calendar.SECOND);  
// 获取当前是本周第几天
int dayOfWeek = calendar.get(Calendar.DAY_OF_WEEK);  
// 获取当前是本月第几天
int dayOfMonth = calendar.get(Calendar.DAY_OF_MONTH);  
// 获取当前是本年第几天
int dayOfYear = calendar.get(Calendar.DAY_OF_YEAR);
```

* 获取当月的第一天和最后一天的字符串。

```
SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");  
// 获取当月第一天
calendar = Calendar.getInstance();
calendar.add(Calendar.MONTH, 0);
calendar.set(Calendar.DAY_OF_MONTH, 1);  
String firstday = format.format(calendar.getTime());  
// 获取当月最后一天
calendar = Calendar.getInstance();  
calendar.add(Calendar.MONTH, 1);  
calendar.set(Calendar.DAY_OF_MONTH, 0);  
String lastday = format.format(calendar.getTime());  
// 打印结果字符串
System.out.println("本月第一天和最后一天分别是：" + firstday + " 和 " + lastday + "。");
```

* 另外也可以使用Date类的实例对象配合SimpleDateFormat类的实例对象来获取当前日期字符串。

```
SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
Date date = new Date(); 
System.out.println("当前日期字符串：" + format.format(date) + "。");
```
[ 参考来源](https://www.cnblogs.com/yanggb/p/11142968.html)
