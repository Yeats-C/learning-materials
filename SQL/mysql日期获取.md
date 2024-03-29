# MySQL日期获取：本月第一天、本月最后一天、上月第一天、上月最后一天、下月第一天、下月最后一天.....

## interval的说明：
```
1、当函数使用时，即interval(),为比较函数，如：interval(10,1,3,5,7); 结果为4；
       
   原理：10为被比较数，后面1,3,5,7为比较数，将后面四个依次与10比较，看后面数字组有多少个少于10，则返回其个数。前提是后面数字组为从小到大排列，否则返回结果0。
	   
2、当关键词使用时，表示为设置时间间隔，常用在date_add()与date_sub()函数里，如：interval 1 day ，解释为将时间间隔设置为1天。
```

## 本月第一天
```
select date_add(curdate(), interval - day(curdate()) + 1 day);
```

## 本月最后一天
```
select last_day(curdate());
```

## 上月第一天
```
select date_add(curdate()-day(curdate())+1,interval -1 month);
```

## 上月最后一天
```
select last_day(date_sub(now(),interval 1 month));
```

## 下月第一天
```
select date_add(curdate()-day(curdate())+1,interval 1 month);
```

## 下月最后一天
```
select last_day(date_sub(now(),interval -1 month));
```

## 本月天数
```
select day(last_day(curdate()));
```

## 上月今天的当前日期
 ```
 select unix_timestamp(date_sub(now(),interval 1 month));
 ```
## 上月今天的当前时间（时间戳）
```
select unix_timestamp(date_sub(now(),interval 1 month));
```

## 获取当前时间与上个月之间的天数
```
select datediff(curdate(), date_sub(curdate(), interval 1 month));
```
