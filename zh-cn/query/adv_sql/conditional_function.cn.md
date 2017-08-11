### 条件函数

| Function Syntax                          | Decription         | Example                                  | Result         |
| ---------------------------------------- | ------------------ | ---------------------------------------- | -------------- |
| CASE valueWHEN value1 [, value11 ]* THEN result1[ WHEN valueN [, valueN1 ]* THEN resultN ]*[ ELSE resultZ ]END | 简单的CASE语句          | ```select distinct CASE OPS_REGION WHEN 'Beijing'THEN 'BJ' WHEN 'Shanghai' THEN 'SH' WHEN 'Hongkong' THEN 'HK' END from kylin_sales``` | ```HK SH BJ``` |
| CASEWHEN condition1 THEN result1[ WHEN conditionN THEN resultN ]*[ ELSE resultZ ]END | 高级的CASE语句          | ```select distinct CASE  WHEN OPS_REGION='Beijing'THEN 'BJ' WHEN OPS_REGION='Shanghai' THEN 'SH' WHEN OPS_REGION='Hongkong' THEN 'HK' END from kylin_sales``` | ```HK SH BJ``` |
| NULLIF(value, value)                     | 如果两个值相同返回NULL      | For example, `NULLIF(5, 5)` returns NULL; `NULLIF(5, 0)` returns 5. |                |
| COALESCE(value, value [, value ]*)       | 如果第一个值为NULL，则取第二个值 | `COALESCE(NULL, 5)`                      | ```5```        |