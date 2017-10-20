### Conditional functions

| Function Syntax                          | Decription                               | Example                                  | Result         |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | -------------- |
| CASE valueWHEN value1 [, value11 ]* THEN result1[ WHEN valueN [, valueN1 ]* THEN resultN ]*[ ELSE resultZ ]END | Simple case                              | ```select distinct CASE OPS_REGION WHEN 'Beijing'THEN 'BJ' WHEN 'Shanghai' THEN 'SH' WHEN 'Hongkong' THEN 'HK' END from kylin_sales``` | ```HK SH BJ``` |
| CASEWHEN condition1 THEN result1[ WHEN conditionN THEN resultN ]*[ ELSE resultZ ]END | Searched case                            | ```select distinct CASE  WHEN OPS_REGION='Beijing'THEN 'BJ' WHEN OPS_REGION='Shanghai' THEN 'SH' WHEN OPS_REGION='Hongkong' THEN 'HK' END from kylin_sales``` | ```HK SH BJ``` |
| NULLIF(value, value)                     | Returns NULL if the values are the same. | For example, ```NULLIF(5, 5)``` returns ```NULL```; ```NULLIF(5, 0)``` returns ```5```. |                |
| COALESCE(value, value [, value ]*)       | Provides a value if the first value is null. | ```COALESCE(NULL, 5)```                      | ```5```        |