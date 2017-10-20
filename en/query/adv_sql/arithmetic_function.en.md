##Arithmetic Function

| Function Syntax                | Decription                               | Example                                  | Return                                   |
| ------------------------------ | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| POWER(numeric1, numeric2)      | Returns *numeric1* raised to the power of *numeric2* | ```select POWER(2,2)```                  | ```4.0```                                |
| ABS(numeric)                   | Returns the absolute value of *numeric*  | ```select ABS(2)```                      | ```2```                                  |
| MOD(numeric, numeric)          | Returns the remainder (modulus) of *numeric1* divided by *numeric2*. The result is negative only if *numeric1* is negative | ```select MOD(31,2)```                   | ```1```                                  |
| SQRT(numeric)                  | Returns the square root of *numeric*     | ```select SQRT(2)```                     | ```1.4142135623730951```                 |
| LN(numeric)                    | Returns the natural logarithm (base *e*) of *numeric* | ```select LN(2)```                       | ```0.6931471805599453```                 |
| LOG10(numeric)                 | Returns the base 10 logarithm of *numeric* | ```select LOG10(2)```                    | ```0.3010299956639812```                 |
| EXP(numeric)                   | Returns *e* raised to the power of *numeric* | ```select EXP(5)```                      | ```148.4131591025766```                  |
| CEIL(numeric)                  | Rounds *numeric* up, returning the smallest integer that is greater than or equal to *numeric* | ``select CEIL(2)``                       | ```2```                                  |
| FLOOR(numeric)                 | Rounds *numeric* down, returning the largest integer that is less than or equal to *numeric* | ```select FLOOR(2)```                    | ```2```                                  |
| RAND([seed])                   | Generates a random double between 0 and 1 inclusive, optionally initializing the random number generator with *seed* | ```select RAND() ```             ```select RAND(12)``` | ```0.012645349183058374```, ```0.41372242023394334``` |
| RAND_INTEGER([seed, ] numeric) | Generates a random integer between 0 and *numeric* - 1 inclusive, optionally initializing the random number generator with *seed* | ```select RAND_INTEGER(50)```       ```select RAND_INTEGER(12,50)``` | ```1```, ```12```                        |
| ACOS(numeric)                  | Returns the arc cosine of *numeric*      | ```select ACOS(0.8)```                   | ```0.6435011087932843```                 |
| ASIN(numeric)                  | Returns the arc sine of *numeric*        | ```select ASIN(0.8)```                   | ```0.9272952180016123```                 |
| ATAN(numeric)                  | Returns the arc tangent of *numeric*     | ```select ATAN(0.8)```                   | ```0.6747409422235527```                 |
| ATAN2(numeric, numeric)        | Returns the arc tangent of the *numeric*coordinates | ```select ATAN2(0.2, 0.8)```             | ```0.24497866312686414```                |
| COS(numeric)                   | Returns the cosine of *numeric*          | ```select COS(5)```                      | ```0.28366218546322625```                |
| COT(numeric)                   | Returns the cotangent of *numeric*       | ```select COT(5)```                      | ```-0.2958129155327455```                |
| DEGREES(numeric)               | Converts *numeric* from radians to degrees | ```select DEGREES(5)```                  | ```286.4788975654116```                  |
| RADIANS(numeric)               | Converts *numeric* from degrees to radians | ```select RADIANS(90)```                 | ```1.5707963267948966```                 |
| ROUND(numeric1, numeric2)      | Rounds *numeric1* to *numeric2* places right to the decimal point | ```select ROUND(5,2)```                  | ```5```                                  |
| SIGN(numeric)                  | Returns the signum of *numeric*          | ```select SIGN(5)```                     | ```1```                                  |
| SIN(numeric)                   | Returns the sine of *numeric*            | ```select SIN(5)```                      | ```-0.9589242746631385```                |
| TAN(numeric)                   | Returns the tangent of *numeric*         | ```select TAN(5)```                      | ```-3.380515006246586```                 |
| TRUNCATE(numeric1, numeric2)   | Truncates *numeric1* to *numeric2* places right to the decimal point | ```select TRUNCATE(5,2)```               | ```5```                                  |

