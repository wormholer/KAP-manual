## 算数函数

| Function Syntax                | Decription                               | Example                                  |
| ------------------------------ | ---------------------------------------- | ---------------------------------------- |
| POWER(numeric1, numeric2)      | 返回以*numeric1*为底，以*numeric2*为幂的指数         | ```POWER(Price,2)```                     |
| ABS(numeric)                   | 返回 *numericd*的绝对值                        | ```ABS(Price)```                         |
| MOD(numeric1, numeric2)        | 返回*numeric1* 除以 *numeric2*的余数.如果*numeric1*为负数 则结果也为负数。 | ```MOD(31,2)```                          |
| SQRT(numeric)                  | 返回 *numeric*开平方。                         | ```SQRT(Price)```                        |
| LN(numeric)                    | 返回以 *e*为底*numeric*的对数                    | ```LN(Price)```                          |
| LOG10(numeric)                 | 返回以10为底的*numeric*的对数                     | ```LOG10(Price)```                       |
| EXP(numeric)                   | 返回以 *e* 为底，以 *numeric*为幂的指数。             | ```EXP(Number)```                        |
| CEIL(numeric)                  | 对*numeric* 向上取整,返回大于或等于*numeric*的最小整数。   | ``CEIL(Price)``                          |
| FLOOR(numeric)                 | 对*numeric* 向下取整, 返回小于或等于*numeric*的最大整数。  | ```FLOOR(Price)```                       |
| RAND([seed])                   | 随机生成0到1（包括0和1）之间的double型的数字，也可以基于*seed*生成随机数字。 | ```RAND() ```             ```RAND(12)``` |
| RAND_INTEGER([seed, ] numeric) | 随机生成0到*numeric*（包括0和*numeric*）之间的double型的数字，也可以基于*seed*生成随机数字。* | ```RAND_INTEGER(50)```       ```RAND_INTEGER(12,50)``` |
| ACOS(numeric)                  | 返回*numeric*d的反余弦                         |                                          |
| ASIN(numeric)                  | 返回*numeric*的反正弦                          |                                          |
| ATAN(numeric)                  | 返回*numeric*d的反正切                         |                                          |
| ATAN2(numeric, numeric)        | 返回*numeric*d的反正切                         |                                          |
| COS(numeric)                   | 返回 *numeric* 的正弦                         | ```COS(5)```                             |
| COT(numeric)                   | 返回 *numeric* 的余切                         | ```COT(5)```                             |
| DEGREES(numeric)               | 将 *numeric* 从 弧度转成角度                     | ```DEGREES(5)```                         |
| RADIANS(numeric)               | 将numeric* 从角度转成弧度                        | ```RADIANS(90)```                        |
| ROUND(numeric1, numeric2)      | 将numeric1* 取 *numeric2 位小数               | ```ROUND(price,2)```                     |
| SIGN(numeric)                  | 返回 *numericd*的符号                         | ```SIGN(Price)```                        |
| SIN(numeric)                   | 返回*numeric* 的正弦                          | ```SIN(price)```                         |
| TAN(numeric)                   | 返回 *numeric*的正切                          | ```TAN(Price)```                         |
| TRUNCATE(numeric1, numeric2)   | 将 *numeric1* 截断到 *numeric2*              | ```TRUNCATE(price,2)```                  |

