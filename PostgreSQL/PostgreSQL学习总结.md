# PostgreSQL学习总结

官网：https://www.postgresql.org/



[SQL Syntax](https://www.postgresql.org/docs/current/sql-syntax.html)


窗口函數`DENSE_RANK`

给key一样的排名，排名函数

DENSE_RANK() OVER (ORDER BY start_date DESC) AS sub_contract_key
WHERE sub_contract_key <= CAST( '10' AS bigint) + 1

但是会导致查询效率变低

to_char(wsn_contract_master.start_date, 'yyyy/MM/dd hh24:mi:ss') as start_date

to_char也会导致效率查询效率变低
