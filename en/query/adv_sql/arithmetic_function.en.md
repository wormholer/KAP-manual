##Arithmetic function

| Function Syntax                | Decription                               | Example                                  |
| ------------------------------ | ---------------------------------------- | ---------------------------------------- |
| POWER(numeric1, numeric2)      | Returns *numeric1* raised to the power of *numeric2* | ```POWER(Price,2)```                     |
| ABS(numeric)                   | Returns the absolute value of *numeric*  | ```ABS(Price)```                         |
| MOD(numeric, numeric)          | Returns the remainder (modulus) of *numeric1* divided by *numeric2*. The result is negative only if *numeric1* is negative | ```MOD(31,2)```                          |
| SQRT(numeric)                  | Returns the square root of *numeric*     | ```SQRT(Price)```                        |
| LN(numeric)                    | Returns the natural logarithm (base *e*) of *numeric* | ```LN(Price)```                          |
| LOG10(numeric)                 | Returns the base 10 logarithm of *numeric* | ```LOG10(Price)```                       |
| EXP(numeric)                   | Returns *e* raised to the power of *numeric* | ```EXP(Number)```                        |
| CEIL(numeric)                  | Rounds *numeric* up, returning the smallest integer that is greater than or equal to *numeric* | ``CEIL(Price)``                          |
| FLOOR(numeric)                 | Rounds *numeric* down, returning the largest integer that is less than or equal to *numeric* | ```FLOOR(Price)```                       |
| RAND([seed])                   | Generates a random double between 0 and 1 inclusive, optionally initializing the random number generator with *seed* | ```RAND() ``` ```RAND(12)```             |
| RAND_INTEGER([seed, ] numeric) | Generates a random integer between 0 and *numeric* - 1 inclusive, optionally initializing the random number generator with *seed* | ```RAND_INTEGER(50)```       ```RAND_INTEGER(12,50)``` |
| ACOS(numeric)                  | Returns the arc cosine of *numeric*      |                                          |
| ASIN(numeric)                  | Returns the arc sine of *numeric*        |                                          |
| ATAN(numeric)                  | Returns the arc tangent of *numeric*     |                                          |
| ATAN2(numeric, numeric)        | Returns the arc tangent of the *numeric*coordinates |                                          |
| COS(numeric)                   | Returns the cosine of *numeric*          | ```COS(5)```                             |
| COT(numeric)                   | Returns the cotangent of *numeric*       | ```COT(5)```                             |
| DEGREES(numeric)               | Converts *numeric* from radians to degrees | ```DEGREES(5)```                         |
| RADIANS(numeric)               | Converts *numeric* from degrees to radians | ```RADIANS(90)```                        |
| ROUND(numeric1, numeric2)      | Rounds *numeric1* to *numeric2* places right to the decimal point | ```ROUND(price,2)```                     |
| SIGN(numeric)                  | Returns the signum of *numeric*          | ```SIGN(Price)```                        |
| SIN(numeric)                   | Returns the sine of *numeric*            | ```SIN(price)```                         |
| TAN(numeric)                   | Returns the tangent of *numeric*         | ```TAN(Price)```                         |
| TRUNCATE(numeric1, numeric2)   | Truncates *numeric1* to *numeric2* places right to the decimal point | ```TRUNCATE(price,2)```                  |

