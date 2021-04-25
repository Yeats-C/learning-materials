
一、创建存储过程、事件
# 创建存储过程
create procedure pr_add()
(
   [in|out|inout] 参数 datatype
)
begin
    MySQL 语句;
end;
# 执行存储过程
call pr_add();
call pr_add(10,20);# 带参数
# 带参数类型不一致问题：Illegal mix of collations (utf8mb4_unicode_ci,IMPLICIT) and (utf8mb4_general_ci,IMPLICIT) for operation '='
解决办法：select * from user where mobile = in_mobile COLLATE utf8_unicode_ci;
# 查看SQL
show variables like 'event%';
# 开启SQL
SET GLOBAL event_scheduler = 1;
SET GLOBAL event_scheduler = ON；
# 查看事件执行情况
SELECT * FROM information_schema.EVENTS;
一、创建存储过程、事件
# 创建存储过程
create procedure pr_add()
(
   [in|out|inout] 参数 datatype
)
begin
    MySQL 语句;
end;
# 执行存储过程
call pr_add();
call pr_add(10,20);# 带参数
# 带参数类型不一致问题：Illegal mix of collations (utf8mb4_unicode_ci,IMPLICIT) and (utf8mb4_general_ci,IMPLICIT) for operation '='
解决办法：select * from user where mobile = in_mobile COLLATE utf8_unicode_ci;
# 查看SQL
show variables like 'event%';
# 开启SQL
SET GLOBAL event_scheduler = 1;
SET GLOBAL event_scheduler = ON；
# 查看事件执行情况
SELECT * FROM information_schema.EVENTS;

二、设置表、字段类型
# 主键设为自动增长
alter table yc_customer_type modify type_id int primary key auto_increment ;
# 自动增长默认起始值
alter table yc_customer_type auto_increment = 1;
# 默认时间，在创建时间字段的时候，表示当插入数据的时候，该字段默认值为当前时间
DEFAULT CURRENT_TIMESTAMP
# 更新时间，表示每次更新这条数据的时候，该字段都会更新成当前时间
ON UPDATE CURRENT_TIMESTAMP
# 生成【创建时间】和【更新时间】两个字段，且不需要代码来维护
update yc_plan_model set update_time=CURRENT_TIMESTAMP where update_time is null;
update yc_plan_model set create_time=CURRENT_TIMESTAMP where create_time is null;
alter table yc_plan_model
    MODIFY column  create_time datetime not null default CURRENT_TIMESTAMP,
    MODIFY column  update_time datetime  not null default CURRENT_TIMESTAMP on update CURRENT_TIMESTAMP ;




三、批量插入、更新操作
# 插入数据，采用：on duplicate key update ，有相关数据更新，没有相同数据插入，

四、日期操作：按月生成日、生成一年的日期、根据日获取周开始时间、周结束时间、日期转换函数
# 按月生成日
SELECT
date_add(
DATE_ADD( curdate(), INTERVAL - DAY ( curdate()) + 2 DAY ),
		INTERVAL ( cast( help_topic_id AS signed INTEGER ) - 1 ) DAY 
	) DAY 
FROM
	mysql.help_topic 
WHERE
	help_topic_id < DAY ( last_day( curdate( ) ) ) 
ORDER BY
help_topic_id

# 生成一年的日期
SELECT
date_add(
DATE_ADD( '2020-01-01', INTERVAL - DAY (  '2020-01-01') + 2 DAY ),
		INTERVAL ( cast( help_topic_id AS signed INTEGER ) - 1 ) DAY 
	) DAY 
FROM
	mysql.help_topic 
WHERE
help_topic_id < 426
ORDER BY
help_topic_id

# 从开始日生成结束日期
SELECT
adddate(( date_format( '2019-01-01', '%Y-%m-%d' )), numlist.id ) AS day # 2019-01-01开始日期
FROM
	( 
	SELECT n1.help_topic_id + n10.help_topic_id * 643  AS id 
		FROM mysql.help_topic n1 
		CROSS JOIN mysql.help_topic AS n10 
		CROSS JOIN mysql.help_topic AS n100 
		LIMIT 2000 
) AS numlist
where adddate(( date_format( '2019-01-01', '%Y-%m-%d' )), numlist.id ) <= '2021-04-14' # 从'2019-01-01'至'2021-04-14'

# 根据时间区间，自动生成时间列表
SELECT
ADDDATE( '2020-01-01', INTERVAL @i := @i + 1 DAY ) AS calday
FROM (
SELECT a.a
FROM (SELECT 0 AS a UNION ALL SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5 UNION ALL SELECT 6 UNION ALL SELECT 7 UNION ALL SELECT 8 UNION ALL SELECT 9) AS a
CROSS JOIN (SELECT 0 AS a UNION ALL SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5 UNION ALL SELECT 6 UNION ALL SELECT 7 UNION ALL SELECT 8 UNION ALL SELECT 9) AS b
CROSS JOIN (SELECT 0 AS a UNION ALL SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5 UNION ALL SELECT 6 UNION ALL SELECT 7 UNION ALL SELECT 8 UNION ALL SELECT 9) AS c
) a
JOIN ( SELECT @i := - 1 ) r1 
WHERE
@i < DATEDIFF( '2021-01-15', '2020-01-01' )

# 根据日获取周开始时间、周结束时间
SELECT
DATE_FORMAT(
str_to_date(
'2020-12-30',
'%Y-%m-%d %k:%i:%s'
),
'%Y-%m-%d'
) AS 'now day',
-- CONCAT_WS(
-- '-',
--开始周时间
from_unixtime(
UNIX_TIMESTAMP(
str_to_date(
'2020-12-30',
'%Y-%m-%d %k:%i:%s'
)
) - WEEKDAY(
str_to_date(
'2020-12-30',
'%Y-%m-%d %k:%i:%s'
)
) * 86400,
'%Y-%m-%d'
) as startweek,
--结束周时间
from_unixtime(
UNIX_TIMESTAMP(
str_to_date(
'2020-12-30',
'%Y-%m-%d %k:%i:%s'
)
) + (
7 - (
WEEKDAY(
str_to_date(
'2020-12-30',
'%Y-%m-%d %k:%i:%s'
)
) + 1
)
) * 86400,
'%Y-%m-%d'
) as endweek
-- ) AS 'target day'

# 函数
now()返回年月日时分秒
curdate()返回年月日
DATEDIFF(date1,date2)    两个日期相减函数，返回date1-date2相差的天数
MAX(GREATEST(daytemp,nighttemp)) # 获取两个字段的最大值
MIN(LEAST(daytemp,nighttemp)) # 获取两个字段的最小值
group_concat(),实现一个ID对应多个名称时，原本为多行数据，把名称合并成一行，如|1 | 10,20,20|
# 周的第几天，输出：0
select date_format('2020-12-14','%w')-1
# 时间间隔，输出：2020-12-14，间隔3天
select subdate('2020-12-17',3) 
# 周的第一天
select subdate('2021-01-01',date_format('2021-01-01','%w')-1);
# 周的最后一天
select subdate('2021-01-01',date_format('2021-01-01','%w')-7);

# mysql时间与字符串之间相互转换
1.时间转字符串
DATE_FORMAT(日期，格式字符串)
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s');
2.字符串转时间
STR_TO_DATE(字符串，日志格式)
SELECT STR_TO_DATE('2019-01-20 16:01:45', '%Y-%m-%d %H:%i:%s');
3.时间转时间戳
select unix_timestamp(now());
4.字符串转时间戳
select unix_timestamp('2019-01-20');  
5.时间戳转字符串
select from_unixtime(1451997924,'%Y-%d');

# 附日期格式如下：
%M 月名字(January……December)  
%W 星期名字(Sunday……Saturday)  
%D 有英语前缀的月份的日期(1st, 2nd, 3rd, 等等。）  
%Y 年, 数字, 4 位  
%y 年, 数字, 2 位  
%a 缩写的星期名字(Sun……Sat)  
%d 月份中的天数, 数字(00……31)  
%e 月份中的天数, 数字(0……31)  
%m 月, 数字(01……12)  
%c 月, 数字(1……12)  
%b 缩写的月份名字(Jan……Dec)  
%j 一年中的天数(001……366)  
%H 小时(00……23)  
%k 小时(0……23)  
%h 小时(01……12)  
%I 小时(01……12)  
%l 小时(1……12)  
%i 分钟, 数字(00……59)  
%r 时间,12 小时(hh:mm:ss [AP]M)  
%T 时间,24 小时(hh:mm:ss)  
%S 秒(00……59)  
%s 秒(00……59)  
%p AM或PM  
%w 一个星期中的天数(0=Sunday ……6=Saturday ）  
%U 星期(0……52), 这里星期天是星期的第一天  
%u 星期(0……52), 这里星期一是星期的第一 
五、一行转多行
# 数据截取，同时一行转多行
SELECT
substring_index( substring_index( a.type_code, ',', b.help_topic_id + 1 ), ',',- 1 ) AS ID ,b.help_topic_id as ide
FROM
	( SELECT type_code FROM yc_plan_model WHERE plan_model_id = 2 ) a 
JOIN mysql.help_topic b ON b.help_topic_id < (
length( a.type_code ) - length( REPLACE ( a.type_code, ',', '' ) ) + 1);
六、一次数据库查询group_concat报错
MySql数据库查询时（或者insert into tables时报错），使用group_concat报错“Row XXX was cut by GROUP_CONCAT()”，单独查询不会报错，当我要查询的数据更新到另外个表中的字段时，会报这个错，网上查了下是因为GROUP_CONCAT有个最大长度的限制，超过最大长度就会被截断掉，可以通过
查询：SELECT @@global.group_concat_max_len;
修改长度（全局变量）：SET GLOBAL group_concat_max_len=102400;
设置后没有依然报错，最后用（设置会话变量）：SET group_concat_max_len=102400;
六、常用SQL
# 查前10条记录
SELECT  * from yc_iotcloud_gsh_day_report8_effective_h limit 0,10
# 分析函数
VAR_SAMP（Variance Sample）：方差与excel中var相同，该函数返回非空集合的样本变量（忽略 null），VAR_SAMP 进行如下计算：(SUM(expr*expr)-SUM(expr)*SUM(expr)/COUNT(expr))/(COUNT(expr)-1)*/
VAR_POP（Variance Population）：总方差与excel中varp相同，该函数返回非空集合的总体变量（忽略 null），VAR_POP进行如下计算：(SUM(expr2) - SUM(expr)2 / COUNT(expr)) / COUNT(expr)*/
STDDEV：计算当前行关于组的标准偏离
STDDEV_SAMP ： 该函数计算累积样本标准偏离，并返回总体变量的平方根


STDDEV_POP：标准方差

VARIANCE：功能描述：该函数返回表达式的变量，Oracle 计算该变量如下：如果表达式中行数为 1，则返回 0,如果表达式中行数大于 1，则返回 VAR_SAMP*/














SELECT * FROM information_schema.EVENTS;

二、设置表、字段类型
# 主键设为自动增长
alter table yc_customer_type modify type_id int primary key auto_increment ;
# 自动增长默认起始值
alter table yc_customer_type auto_increment = 1;
# 默认时间，在创建时间字段的时候，表示当插入数据的时候，该字段默认值为当前时间
DEFAULT CURRENT_TIMESTAMP
# 更新时间，表示每次更新这条数据的时候，该字段都会更新成当前时间
ON UPDATE CURRENT_TIMESTAMP
# 生成【创建时间】和【更新时间】两个字段，且不需要代码来维护
update yc_plan_model set update_time=CURRENT_TIMESTAMP where update_time is null;
update yc_plan_model set create_time=CURRENT_TIMESTAMP where create_time is null;
alter table yc_plan_model
    MODIFY column  create_time datetime not null default CURRENT_TIMESTAMP,
    MODIFY column  update_time datetime  not null default CURRENT_TIMESTAMP on update CURRENT_TIMESTAMP ;




三、批量插入、更新操作
# 插入数据，采用：on duplicate key update ，有相关数据更新，没有相同数据插入，

四、日期操作：按月生成日、生成一年的日期、根据日获取周开始时间、周结束时间、日期转换函数
# 按月生成日
SELECT
date_add(
DATE_ADD( curdate(), INTERVAL - DAY ( curdate()) + 2 DAY ),
		INTERVAL ( cast( help_topic_id AS signed INTEGER ) - 1 ) DAY 
	) DAY 
FROM
	mysql.help_topic 
WHERE
	help_topic_id < DAY ( last_day( curdate( ) ) ) 
ORDER BY
help_topic_id

# 生成一年的日期
SELECT
date_add(
DATE_ADD( '2020-01-01', INTERVAL - DAY (  '2020-01-01') + 2 DAY ),
		INTERVAL ( cast( help_topic_id AS signed INTEGER ) - 1 ) DAY 
	) DAY 
FROM
	mysql.help_topic 
WHERE
help_topic_id < 426
ORDER BY
help_topic_id

# 从开始日生成结束日期
SELECT
adddate(( date_format( '2019-01-01', '%Y-%m-%d' )), numlist.id ) AS day # 2019-01-01开始日期
FROM
	( 
	SELECT n1.help_topic_id + n10.help_topic_id * 643  AS id 
		FROM mysql.help_topic n1 
		CROSS JOIN mysql.help_topic AS n10 
		CROSS JOIN mysql.help_topic AS n100 
		LIMIT 2000 
) AS numlist
where adddate(( date_format( '2019-01-01', '%Y-%m-%d' )), numlist.id ) <= '2021-04-14' # 从'2019-01-01'至'2021-04-14'

# 根据时间区间，自动生成时间列表
SELECT
ADDDATE( '2020-01-01', INTERVAL @i := @i + 1 DAY ) AS calday
FROM (
SELECT a.a
FROM (SELECT 0 AS a UNION ALL SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5 UNION ALL SELECT 6 UNION ALL SELECT 7 UNION ALL SELECT 8 UNION ALL SELECT 9) AS a
CROSS JOIN (SELECT 0 AS a UNION ALL SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5 UNION ALL SELECT 6 UNION ALL SELECT 7 UNION ALL SELECT 8 UNION ALL SELECT 9) AS b
CROSS JOIN (SELECT 0 AS a UNION ALL SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5 UNION ALL SELECT 6 UNION ALL SELECT 7 UNION ALL SELECT 8 UNION ALL SELECT 9) AS c
) a
JOIN ( SELECT @i := - 1 ) r1 
WHERE
@i < DATEDIFF( '2021-01-15', '2020-01-01' )

# 根据日获取周开始时间、周结束时间
SELECT
DATE_FORMAT(
str_to_date(
'2020-12-30',
'%Y-%m-%d %k:%i:%s'
),
'%Y-%m-%d'
) AS 'now day',
-- CONCAT_WS(
-- '-',
--开始周时间
from_unixtime(
UNIX_TIMESTAMP(
str_to_date(
'2020-12-30',
'%Y-%m-%d %k:%i:%s'
)
) - WEEKDAY(
str_to_date(
'2020-12-30',
'%Y-%m-%d %k:%i:%s'
)
) * 86400,
'%Y-%m-%d'
) as startweek,
--结束周时间
from_unixtime(
UNIX_TIMESTAMP(
str_to_date(
'2020-12-30',
'%Y-%m-%d %k:%i:%s'
)
) + (
7 - (
WEEKDAY(
str_to_date(
'2020-12-30',
'%Y-%m-%d %k:%i:%s'
)
) + 1
)
) * 86400,
'%Y-%m-%d'
) as endweek
-- ) AS 'target day'

# 函数
now()返回年月日时分秒
curdate()返回年月日
DATEDIFF(date1,date2)    两个日期相减函数，返回date1-date2相差的天数
MAX(GREATEST(daytemp,nighttemp)) # 获取两个字段的最大值
MIN(LEAST(daytemp,nighttemp)) # 获取两个字段的最小值
group_concat(),实现一个ID对应多个名称时，原本为多行数据，把名称合并成一行，如|1 | 10,20,20|
# 周的第几天，输出：0
select date_format('2020-12-14','%w')-1
# 时间间隔，输出：2020-12-14，间隔3天
select subdate('2020-12-17',3) 
# 周的第一天
select subdate('2021-01-01',date_format('2021-01-01','%w')-1);
# 周的最后一天
select subdate('2021-01-01',date_format('2021-01-01','%w')-7);

# mysql时间与字符串之间相互转换
1.时间转字符串
DATE_FORMAT(日期，格式字符串)
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s');
2.字符串转时间
STR_TO_DATE(字符串，日志格式)
SELECT STR_TO_DATE('2019-01-20 16:01:45', '%Y-%m-%d %H:%i:%s');
3.时间转时间戳
select unix_timestamp(now());
4.字符串转时间戳
select unix_timestamp('2019-01-20');  
5.时间戳转字符串
select from_unixtime(1451997924,'%Y-%d');

# 附日期格式如下：
%M 月名字(January……December)  
%W 星期名字(Sunday……Saturday)  
%D 有英语前缀的月份的日期(1st, 2nd, 3rd, 等等。）  
%Y 年, 数字, 4 位  
%y 年, 数字, 2 位  
%a 缩写的星期名字(Sun……Sat)  
%d 月份中的天数, 数字(00……31)  
%e 月份中的天数, 数字(0……31)  
%m 月, 数字(01……12)  
%c 月, 数字(1……12)  
%b 缩写的月份名字(Jan……Dec)  
%j 一年中的天数(001……366)  
%H 小时(00……23)  
%k 小时(0……23)  
%h 小时(01……12)  
%I 小时(01……12)  
%l 小时(1……12)  
%i 分钟, 数字(00……59)  
%r 时间,12 小时(hh:mm:ss [AP]M)  
%T 时间,24 小时(hh:mm:ss)  
%S 秒(00……59)  
%s 秒(00……59)  
%p AM或PM  
%w 一个星期中的天数(0=Sunday ……6=Saturday ）  
%U 星期(0……52), 这里星期天是星期的第一天  
%u 星期(0……52), 这里星期一是星期的第一 
五、一行转多行
# 数据截取，同时一行转多行
SELECT
substring_index( substring_index( a.type_code, ',', b.help_topic_id + 1 ), ',',- 1 ) AS ID ,b.help_topic_id as ide
FROM
	( SELECT type_code FROM yc_plan_model WHERE plan_model_id = 2 ) a 
JOIN mysql.help_topic b ON b.help_topic_id < (
length( a.type_code ) - length( REPLACE ( a.type_code, ',', '' ) ) + 1);
六、常用SQL
# 查前10条记录
SELECT  * from yc_iotcloud_gsh_day_report8_effective_h limit 0,10
# 分析函数
VAR_SAMP（Variance Sample）：方差与excel中var相同，该函数返回非空集合的样本变量（忽略 null），VAR_SAMP 进行如下计算：(SUM(expr*expr)-SUM(expr)*SUM(expr)/COUNT(expr))/(COUNT(expr)-1)*/
VAR_POP（Variance Population）：总方差与excel中varp相同，该函数返回非空集合的总体变量（忽略 null），VAR_POP进行如下计算：(SUM(expr2) - SUM(expr)2 / COUNT(expr)) / COUNT(expr)*/
STDDEV：计算当前行关于组的标准偏离
STDDEV_SAMP ： 该函数计算累积样本标准偏离，并返回总体变量的平方根


STDDEV_POP：标准方差

VARIANCE：功能描述：该函数返回表达式的变量，Oracle 计算该变量如下：如果表达式中行数为 1，则返回 0,如果表达式中行数大于 1，则返回 VAR_SAMP*/













