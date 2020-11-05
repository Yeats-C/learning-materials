原sql：

DELETE
FROM
    store_organization
WHERE
    store_id IN (
		
        SELECT
            store_id
        FROM
            store_organization
        GROUP BY
            store_id
        HAVING
            count(store_id) > 1
   ) 
AND id NOT IN (


    SELECT
        min(id) as id
    FROM
        store_organization
    GROUP BY
        store_id
    HAVING
        count(store_id) > 1
				
)

思路为：删除store_organization表中 store_id 重复（>1）的数据，只留下id最小的一条

问题：You can't specify target table 'store_organization' for update in FROM clause
      不能在一条sql中先查询结果然后对这个结果进行删除修改操作
      

解决：将SELECT出的结果再通过中间表SELECT一遍，这样就规避了错误



修改后sql：
DELETE
FROM
    store_organization
WHERE
    store_id IN (
		SELECT s.store_id from(
        SELECT
            store_id
        FROM
            store_organization
        GROUP BY
            store_id
        HAVING
            count(store_id) > 1
   ) s )  
AND id NOT IN (
SELECT a.id from (

    SELECT
        min(id) as id
    FROM
        store_organization
    GROUP BY
        store_id
    HAVING
        count(store_id) > 1
				) a
)


需要注意的是，这个问题只出现于MySQL，MSSQL和Oracle不会出现此问题。
