# Truncated incorrect DOUBLE value


```
	INSERT INTO energy_data_warehouse_test.c1101_3_financial_profits (
		enterprise_name,
		organization_code,
		date_type,
		start_time,
		end_time,
		profits_name,
		unit,
		profits_code,
		profits_1_value,
		data_source
		)
		
	SELECT
	a.enterprise_name AS enterprise_name,
	a.organization_code AS organization_code,
	a.date_type,
	DATE_FORMAT( a.start_time, '%Y-%m-%d %k:%i:%s' ) AS start_time,
	DATE_FORMAT( a.end_time, '%Y-%m-%d %k:%i:%s' ) AS end_time,
	a.indicators_name AS profits_name,
	a.unit AS unit,
	a.indicators_code AS profits_code,
	IFNULL( a.financial_situation_1_month, 0 ) AS profits_1_value,
	'AB203表_M_各企业财务状况[ab203_month_financial_situation]' AS data_source 
FROM
	energy_data_terminal_test.ab203_month_financial_situation a 
WHERE
	a.indicators_name IN (
		'营业收入',
		'营业成本',
		'税金及附加',
		'销售费用',
		'管理费用',
		'研发费用',
		'财务费用',
		'资产减值损失',
		'信用减值损失',
		'其他收益',
		'投资收益(损失以“-”号记)',
		'投资收益',
		'净敞口套期收益(损失以"-"号记)',
		'净敞口套期收益',
		'公允价值变动收益(损失以“-”号记)',
		'公允价值变动收益',
		'资产处置收益（损失以' - '号记）',
		'资产处置收益(损失以“-”号记)',
		'营业利润',
		'营业外收入',
		'营业外支出',
		'利润总额',
		'三、应交增值税',
		'应交增值税' 
	)
	-- 	where a.indicators_code in ('301','307','309','312','313','331','317','320','333','330','322','334','321','335','323','325','326','327','402') 
	on DUPLICATE key UPDATE profits_1_value = VALUES(profits_1_value),
		update_time = NOW();
```

* 一直在使用的一条insert语句，突然报这个错误。检查过结果集和数据表，没有发现数据错位的情况。

## 问题判断

* 排除掉数据库环境问题，排除掉数据错位问题，那就应该是sql原因,判断是因为where条件里的中文字符有特殊符号“-”

## 解决

将 in 函数 改为 FIND_IN_SET

```
	INSERT INTO energy_data_warehouse_test.c1101_3_financial_profits_copy1 (
		enterprise_name,
		organization_code,
 		date_type,
		start_time,
		end_time,
		profits_name,
		unit,
 		profits_code,
		profits_1_value,
		data_source
		)
		
	SELECT
	
	a.enterprise_name AS enterprise_name,
	a.organization_code AS organization_code,
 	a.date_type,
 	DATE_FORMAT( a.start_time, '%Y-%m-%d %k:%i:%s' ) AS start_time,
 	DATE_FORMAT( a.end_time, '%Y-%m-%d %k:%i:%s' ) AS end_time,
 	IFNULL(a.indicators_name,'') AS profits_name,
 	IFNULL(a.unit,'') AS unit,
 	a.indicators_code AS profits_code,
 	IFNULL( a.financial_situation_1_month, 0 ) AS profits_1_value,
	'AB203表_M_各企业财务状况[ab203_month_financial_situation]' AS data_source 
FROM
	energy_data_terminal_test.ab203_month_financial_situation a 
WHERE
	FIND_IN_SET(a.indicators_name,'营业收入,营业成本,税金及附加,销售费用,管理费用,研发费用,财务费用,资产减值损失,信用减值损失,其他收益,投资收益,净敞口套期收益,公允价值变动收益,资产处置收益(损失以“-”号记),营业利润,营业外收入,营业外支出,利润总额,三、应交增值税,应交增值税')
 or FIND_IN_SET(a.indicators_code,'301,307,309,312,313,331,317,320,333,330,322,334,321,335,323,325,326,327,402')
	on DUPLICATE key UPDATE profits_1_value = VALUES(profits_1_value),
		update_time = NOW();
```
