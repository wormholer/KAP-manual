## Operator

KAP suppports below operators:

### Comparison Operator 

| Opeartor             | Decription                               | Syntax                                   | Example                                  |
| -------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| <                    | Less than                                | A<B                                      | Profit < Cost                            |
| <=                   | Less than or equal                       | A<=B                                     | Profit <=Cost                            |
| >                    | Greater than                             | A>=B                                     | Profit >Cost                             |
| >=                   | Greater than or equal                    | A>=B                                     | Profit >=Cost                            |
| <>                   | Not Equal                                | A<>B                                     | Profit1<>Profit2                         |
| IS NULL              | Whether *value* is null                  | value IS NULL                            | profit IS NULL                           |
| IS NOT NULL          | Whether *value* is not null              | value IS NOT NULL                        | profit IS NOT NULL                       |
| IS DISTINCT FROM     | Whether two values are not equal, treating null values as the same | value1 IS DISTINCT FROM value2           | profit1 IS DISTINCT FROM profit2         |
| IS NOT DISTINCT FROM | Whether two values are equal, treating null values as the same | value1 IS NOT DISTINCT FROM value2       | profit1 IS NOT DISTINCT FROM profit2     |
| BETWEEN              | Return true if the specified value is greater than or equal to value1 and less than or equal to value2 | A BETWEEN   value1 AND value2            | profit BETWEEN 1 AND 1000      Date BETWEEN '2016-01-01' AND '2016-12-30' |
| NOT BETWEEN          | Whether *value1* is less than *value2* or greater than *value3* | value1 NOT BETWEEN value2 AND value3     | profit NOT BETWEEN 1 AND 1000      Date NOT BETWEEN '2016-01-01' AND '2016-12-30' |
| LIKE                 | Whether string1 matches pattern string2  | sptring1 LIKE sptring2                   | name LIKE '%frank%'                      |
| NOT LIKE             | Whether *string1* does not match pattern *string2* | string1 NOT LIKE string2 [ ESCAPE string3 ] | name NOT LIKE '%frank%'                  |
| SIMILAR TO           | Whether string1 matches regular expression string2 | string1 SIMILAR TO string2               | name SIMILAR TO 'frank'                  |
| NOT SIMILAR TO       | Whether *string1* does not match regular expression *string2* | string1 NOT SIMILAR TO string2           | name NOT SIMILAR TO 'frank'              |

###Logical operator

| Opeartor     | Decription                               | Syntax                | Example                       |
| ------------ | ---------------------------------------- | --------------------- | ----------------------------- |
| AND          | Whether *boolean1* and *boolean2* are both TRUE | boolean1 AND boolean2 | Name ='frank' AND gender='M'  |
| OR           | Whether *boolean1* is TRUE or *boolean2* is TRUE | boolean1 OR boolean2  | Name='frank' OR Name='Hentry' |
| NOT          | Whether *boolean* is not TRUE; returns UNKNOWN if *boolean* is UNKNOWN | NOT boolean           | NOT (NAME ='frank')           |
| IS FALSE     | Whether *boolean* is FALSE; returns FALSE if *boolean* is UNKNOWN | boolean IS FALSE      | Name ='frank' IS FALSE        |
| IS NOT FALSE | Whether *boolean* is not FALSE; returns TRUE if *boolean* is UNKNOWN | boolean IS NOT FALSE  | Name ='frank' IS NOT FALSE    |
| IS TRUE      | Whether *boolean* is TRUE; returns FALSE if *boolean* is UNKNOWN | boolean IS TRUE       | Name ='frank' IS TRUE         |
| IS NOT TRUE  | Whether *boolean* is not TRUE; returns TRUE if *boolean* is UNKNOWN | boolean IS NOT TRUE   | Name ='frank' IS NOT TRUE     |

###Arithmetic operator

| Opeartor | Decription                               | Syntax | Example              |
| -------- | ---------------------------------------- | ------ | -------------------- |
| +        | Plus                                     | A+B    | Cost+Profit          |
| -        | Minus.                                   | A-B    | Revenue- Cost        |
| *        | Times. Returns *numeric1* multiplied by *numeric2* | A*B    | Unit_Price* Quantity |
| /        | Devide                                   | A/B    | Total_Sale/Quantity  |

### String operator

| Opeartor | Decription         | Syntax | Example                 |
| -------- | ------------------ | ------ | ----------------------- |
| \|\|     | string \|\| string | A\|\|B | First_name\|\|Last_name |