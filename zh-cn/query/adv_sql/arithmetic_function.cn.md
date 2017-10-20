## 算数函数

| Function Syntax                | Decription                               | Example                                  | Return                                   |
| ------------------------------ | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| POWER(numeric1, numeric2)      | 返回以*numeric1*为底，以*numeric2*为幂的指数         | ```select POWER(2,2)```                  | ```4.0```                                |
| ABS(numeric)                   | 返回 *numericd*的绝对值                        | ```select ABS(2)```                      | ```2```                                  |
| MOD(numeric1, numeric2)        | 返回*numeric1* 除以 *numeric2*的余数.如果*numeric1*为负数 则结果也为负数。 | ```select MOD(31,2)```                   | ```1```                                  |
| SQRT(numeric)                  | 返回 *numeric*开平方。                         | ```select SQRT(2)```                     | ```1.4142135623730951```                 |
| LN(numeric)                    | 返回以 *e*为底*numeric*的对数                    | ```select LN(2)```                       | ```0.6931471805599453```                 |
| LOG10(numeric)                 | 返回以10为底的*numeric*的对数                     | ```select LOG10(2)```                    | ```0.3010299956639812```                 |
| EXP(numeric)                   | 返回以 *e* 为底，以 *numeric*为幂的指数。             | ```select EXP(5)```                      | ```148.4131591025766```                  |
| CEIL(numeric)                  | 对*numeric* 向上取整,返回大于或等于*numeric*的最小整数。   | ``select CEIL(2)``                       | ```2```                                  |
| FLOOR(numeric)                 | 对*numeric* 向下取整, 返回小于或等于*numeric*的最大整数。  | ```select FLOOR(2)```                    | ```2```                                  |
| RAND([seed])                   | 随机生成0到1（包括0和1）之间的double型的数字，也可以基于*seed*生成随机数字。 | ```select RAND()```  ```select RAND(12)``` | ```0.012645349183058374```, ```0.41372242023394334``` |
| RAND_INTEGER([seed, ] numeric) | 随机生成0到*numeric*（包括0和*numeric*）之间的double型的数字，也可以基于*seed*生成随机数字。* | ```select RAND_INTEGER(50)``` ```select RAND_INTEGER(12,50)``` | ```1```, ```12```                        |
| ACOS(numeric)                  | 返回*numeric*d的反余弦                         | ```select ACOS(0.8)```                   | ```0.6435011087932843```                 |
| ASIN(numeric)                  | 返回*numeric*的反正弦                          | ```select ASIN(0.8)```                   | ```0.9272952180016123```                 |
| ATAN(numeric)                  | 返回*numeric*d的反正切                         | ```select ATAN(0.8)```                   | ```0.6747409422235527```                 |
| ATAN2(numeric, numeric)        | 返回*numeric*d的反正切                         | ```select ATAN2(0.2, 0.8)```             | ```0.24497866312686414```                |
| COS(numeric)                   | 返回 *numeric* 的正弦                         | ```select COS(5)```                      | ```0.28366218546322625```                |
| COT(numeric)                   | 返回 *numeric* 的余切                         | ```select COT(5)```                      | ```-0.2958129155327455```                |
| DEGREES(numeric)               | 将 *numeric* 从 弧度转成角度                     | ```select DEGREES(5)```                  | ```286.4788975654116```                  |
| RADIANS(numeric)               | 将numeric* 从角度转成弧度                        | ```select RADIANS(90)```                 | ```1.5707963267948966```                 |
| ROUND(numeric1, numeric2)      | 将numeric1* 取 *numeric2 位小数               | ```select ROUND(5,2)```                  | ```5```                                  |
| SIGN(numeric)                  | 返回 *numericd*的符号                         | ```select SIGN(5)```                     | ```1```                                  |
| SIN(numeric)                   | 返回*numeric* 的正弦                          | ```select SIN(5)```                      | ```-0.9589242746631385```                |
| TAN(numeric)                   | 返回 *numeric*的正切                          | ```select TAN(5)```                      | ```-3.380515006246586```                 |
| TRUNCATE(numeric1, numeric2)   | 将 *numeric1* 截断到 *numeric2*              | ```select TRUNCATE(5,2)```               | ```5```                                  |

