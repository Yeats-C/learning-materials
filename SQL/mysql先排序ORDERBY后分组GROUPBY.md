#  MYSQL先排序ORDER BY,后分组 GROUP BY

* 需求：将1-3月数据加总成为第一季度数据 ，其中部分字段需要求和加总，其余字段取最后一个月份的累积值数据

```
-- 第一季度     
SELECT
	`area_name`,
	`area_code`,
	'2' AS `date_type`,
	DATE_FORMAT( start_time, '%Y-01-01' ) AS `start_time`,
	DATE_FORMAT( end_time, '%Y-03-31' ) AS `end_time`,
	`is_range`,
	`indicators_name`,
	`indicators_code`,
	`standard_quantity_unit`,
	sum( standard_quantity_value ) AS `standard_quantity_value`,
	`standard_quantity_1_value`,
	`added_value_unit`,
	sum( added_value ) AS `added_value`,
	`added_1_value`,
	`co2_unit`,
	sum(co2_value) as `co2_value`,
	`remarks`,
	`is_believe`,
	'数据汇总' AS `data_source` 
FROM
	(
	SELECT
		* 
	FROM
		a301_1_area_energy_total_consumption_test
	WHERE
		start_time >= '2020-01-01' 
		AND end_time <= '2022-01-01' 
		AND date_type = '3' 
		AND is_believe = '1' 
		AND MONTH ( start_time ) IN ( 01, 02, 03 ) 
	ORDER BY
		start_time DESC 
		
	) a 
GROUP BY
	YEAR ( start_time ),
	area_code,
	is_range,
	indicators_name,
	indicators_code
```

* 思路没有问题，但是执行的时候group by后第一条数据并不是我们想要的最后一个月份也就是三月数据
* explain 查看执行计划，发现在没有 limit 的情况，会少了一个derived 操作,mysql会认为这时候不需要排序,内部做了优化,所以这种情况下加limit限制就可以了
* 此问题原因是mysql在执行一条sql的时候，会先校验一遍sql的合法性，然后自动优化sql以满足索引为优先，此时内部子查询排序与外部group by的优先级相比，
* group by优先级更高，会默认出现group by的第一条数据，此时子查询里加上limit，会让优化时保持同样的排序规则

```
-- 第一季度     
SELECT
	`area_name`,
	`area_code`,
	'2' AS `date_type`,
	DATE_FORMAT( start_time, '%Y-01-01' ) AS `start_time`,
	DATE_FORMAT( end_time, '%Y-03-31' ) AS `end_time`,
	`is_range`,
	`indicators_name`,
	`indicators_code`,
	`standard_quantity_unit`,
	sum( standard_quantity_value ) AS `standard_quantity_value`,
	`standard_quantity_1_value`,
	`added_value_unit`,
	sum( added_value ) AS `added_value`,
	`added_1_value`,
	`co2_unit`,
	sum(co2_value) as `co2_value`,
	`remarks`,
	`is_believe`,
	'数据汇总' AS `data_source` 
FROM
	(
	SELECT
		* 
	FROM
		a301_1_area_energy_total_consumption_test
	WHERE
		start_time >= '2020-01-01' 
		AND end_time <= '2022-01-01' 
		AND date_type = '3' 
		AND is_believe = '1' 
		AND MONTH ( start_time ) IN ( 01, 02, 03 ) 
	ORDER BY
		start_time DESC 
		LIMIT 99999999 
	) a 
GROUP BY
	YEAR ( start_time ),
	area_code,
	is_range,
	indicators_name,
	indicators_code
```
