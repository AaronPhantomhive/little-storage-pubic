# PostgreSQL学习总结

官网：https://www.postgresql.org/


## [SQL Syntax](https://www.postgresql.org/docs/current/sql-syntax.html)

### 窗口函数 `DENSE_RANK`

给key一样的排名，排名函数

```sql
DENSE_RANK() OVER (ORDER BY start_date DESC) AS sub_contract_key
WHERE sub_contract_key <= CAST( '10' AS bigint) + 1
```

但是会导致查询效率变低

to_char(wsn_contract_master.start_date, 'yyyy/MM/dd hh24:mi:ss') as start_date

to_char也会导致效率查询效率变低



### 外键约束

```sql
ALTER TABLE tenant_1.b2b_info
ADD FOREIGN KEY (name) REFERENCES tenant_1.b2b_definition(name) ON DELETE CASCADE
```

这段SQL语句的作用是在`b2b_info`表中添加一个名为`name`的外键约束，该约束将`name`字段与`b2b_definition`表中的`name`字段相关联，并且当`b2b_definition`表中相应的行被删除时，`b2b_info`表中相关的行也会被删除。

外键约束用于确保在两个表之间建立引用关系，保证数据的一致性和完整性。
