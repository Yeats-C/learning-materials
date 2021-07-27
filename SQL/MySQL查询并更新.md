# Mysql查询并更新
```
UPDATE week_enterprise_energy_consumption mp
INNER JOIN ( SELECT enterprise_name, organization_code FROM day_enterprise_energy_product_consumption mp ) mp2 
SET mp.organization_code = mp2.organization_code 
WHERE
	mp.enterprise_name = mp2.enterprise_name
```

* 查询更新同一张表，MySql是不允许的，所以这里用INNER JOIN 引入内部关联表即可完后查询更新


```
SELECT DISTINCT
	( enterprise_name ),
	organization_code 
FROM
	week_enterprise_energy_consumption
```

* 可通过DISTINCT查询所有去重后数据
