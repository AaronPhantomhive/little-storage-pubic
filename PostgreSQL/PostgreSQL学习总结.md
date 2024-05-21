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



### 主键约束

```sql
ALTER TABLE public.b2b_prop 
ADD CONSTRAINT b2b_prop_pkey PRIMARY KEY (key,tenant_id);
```

- `ALTER TABLE` 用于修改现有表的结构。
- `ADD CONSTRAINT` 是添加约束的子句。
- `(key, tenant_id)` 指定了主键由 `key` 和 `tenant_id` 这两列组合而成。



### 处理插入冲突

```sql
INSERT INTO ${schema}.b2b_prop (key, value, tenant_id) 
VALUES ('mode', '0', ${tenantId}) 
ON CONFLICT (key,tenant_id) DO UPDATE SET value = EXCLUDED.value 
```



### 从一个序列对象中获取下一个值

`nextval` 函数会自动递增序列并返回递增后的新值。

```sql
SELECT nextval('public.b2b_tenant_seq')
```



### 查询命名空间（schema）

```sql
SELECT nspname FROM pg_namespace;
```

- `pg_namespace` 是PostgreSQL系统表的名称，包含了关于数据库中所有命名空间的信息。

可能会看见以下结果：

```
  nspname     
----------------------
  pg_toast
  pg_catalog
  public
  information_schema
  ...
```

- **pg_toast**：用于存储大对象的溢出数据。确保PostgreSQL可以高效地管理超大数据。
- **pg_catalog**：提供了数据库的核心元数据，并且是查询和管理数据库对象信息的基础。包含数据库元数据和系统表。
- **public**：默认的用户schema，通常用于存储用户创建的表和对象。
- **information_schema**：提供了一种标准化方式来访问数据库元数据，方便数据库管理和迁移。
