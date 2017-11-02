## 运算符

KAP 支持以下运算符：

### 比较运算符

| 运算符                  | 描述                                 | 语法                                       | 示例                                       |
| -------------------- | ---------------------------------- | ---------------------------------------- | ---------------------------------------- |
| <                    | 小于                                 | A<B                                      | Profit < Cost                            |
| <=                   | 小于或等于                              | A<=B                                     | Profit <=Cost                            |
| >                    | 大于                                 | A>=B                                     | Profit >Cost                             |
| >=                   | 大于或等于                              | A>=B                                     | Profit >=Cost                            |
| <>                   | 不等于                                | A<>B                                     | Profit1<>Profit2                         |
| IS NULL              | 判断值是否为NULL                         | value IS NULL                            | profit IS NULL                           |
| IS NOT NULL          | 判断值是否不为NULL                        | value IS NOT NULL                        | profit IS NOT NULL                       |
| IS DISTINCT FROM     | 判断两个值是否不相等，其中有值为NULL时当作相等。         | value1 IS DISTINCT FROM value2           | profit1 IS DISTINCT FROM profit2         |
| IS NOT DISTINCT FROM | 判断两个值是否相等，其中有值为NULL时当作相等。          | value1 IS NOT DISTINCT FROM value2       | profit1 IS NOT DISTINCT FROM profit2     |
| BETWEEN              | 如果具体的值大于等于value1且小于等于value2，返回结果为真 | A BETWEEN   value1 AND value2            | profit BETWEEN 1 AND 1000      Date BETWEEN '2016-01-01' AND '2016-12-30' |
| NOT BETWEEN          | 如果具体的值大于等于value1且小于等于value2，返回结果为假 | value1 NOT BETWEEN value2 AND value3     | profit NOT BETWEEN 1 AND 1000      Date NOT BETWEEN '2016-01-01' AND '2016-12-30' |
| LIKE                 | string1是否和string2的样式匹配             | sptring1 LIKE spring2                    | name LIKE '%frank%'                      |
| NOT LIKE             | string1是否和string2的样式不匹配            | string1 NOT LIKE string2 [ ESCAPE string3 ] | name NOT LIKE '%frank%'                  |
| SIMILAR TO           | string1是否和string2按正则表达式匹配          | string1 SIMILAR TO string2               | name SIMILAR TO 'frank'                  |
| NOT SIMILAR TO       | string1是否和string2按正则表达式不匹配         | string1 NOT SIMILAR TO string2           | name NOT SIMILAR TO 'frank'              |

### 逻辑运算符

| 运算符          | 描述                                       | 语法                    | 示例                            |
| ------------ | ---------------------------------------- | --------------------- | ----------------------------- |
| AND          | 是否条件1 *boolean1* 和 条件2 *boolean2* 都为真    | boolean1 AND boolean2 | Name ='frank' AND gender='M'  |
| OR           | 是否条件1 *boolean1* 或 条件2 *boolean2* 为真     | boolean1 OR boolean2  | Name='frank' OR Name='Hentry' |
| NOT          | 是否 *boolean* 不为真; 如果*boolean* 为UNKNOWN则返回UNKNOWN | NOT boolean           | NOT (NAME ='frank')           |
| IS FALSE     | 是否 *boolean* 为假; 如果*boolean* 为UNKNOWN则返回假 | boolean IS FALSE      | Name ='frank' IS FALSE        |
| IS NOT FALSE | 是否 *boolean* 不为假; 如果*boolean* 为UNKNOWN则返回真 | boolean IS NOT FALSE  | Name ='frank' IS NOT FALSE    |
| IS TRUE      | 是否 *boolean* 为真; 如果*boolean* 为UNKNOWN则返回假 | boolean IS TRUE       | Name ='frank' IS TRUE         |
| IS NOT TRUE  | 是否 *boolean* 不为真; 如果*boolean* 为UNKNOWN则返回真 | boolean IS NOT TRUE   | Name ='frank' IS NOT TRUE     |

### 算术运算符

| 运算符  | 描述   | 语法   | 示例                   |
| ---- | ---- | ---- | -------------------- |
| +    | 加号   | A+B  | Cost+Profit          |
| -    | 减号   | A-B  | Revenue- Cost        |
| *    | 称号   | A*B  | Unit_Price* Quantity |
| /    | 除号   | A/B  | Total_Sale/Quantity  |

### 字符串运算符

| 运算符                           | 描述    | 语法                           | 示例                                       |
| ----------------------------- | ----- | ---------------------------- | ---------------------------------------- |
| {% raw %}
||
{% endraw %} | 字符连接符 | {% raw %}
A||B 
{% endraw %} | {% raw %}
First_name||Last_name 
{% endraw %} |
