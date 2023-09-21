`MERGE INTO`语句可以根据条件判断，来执行不同的[[Database/DDL]]语句

```sql
MERGE INTO target_table 
USING source_table 
ON <search_condition> 
   WHEN MATCHED THEN 
        UPDATE SET col1 = value1, col2 = value2,... 
        WHERE <update_condition> 
        [DELETE WHERE <delete_condition>] 
   WHEN NOT MATCHED THEN 
        INSERT (col1,col2,...) values(value1,value2,...) 
        WHERE <insert_condition>;
```

