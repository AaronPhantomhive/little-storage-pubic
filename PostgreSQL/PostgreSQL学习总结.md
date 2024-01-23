# PostgreSQL学习总结

官网：https://www.postgresql.org/



[SQL Syntax](https://www.postgresql.org/docs/current/sql-syntax.html)


窗口函數`DENSE_RANK`

DENSE_RANK() OVER (ORDER BY start_date DESC) AS sub_contract_key
WHERE sub_contract_key <= CAST( '10' AS bigint) + 1 
