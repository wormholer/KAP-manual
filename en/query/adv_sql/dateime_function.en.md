### Date and time function

| Function Syntax                          | Decription                               | Example                               | Result                     |
| ---------------------------------------- | ---------------------------------------- | ------------------------------------- | -------------------------- |
| LOCALTIME                                | Returns the current date and time in the session time zone in a value of datatype TIME | ```select LOCALTIME```                | ```21:34:30``              |
|                                          |                                          |                                       |                            |
| LOCALTIMESTAMP                           | Returns the current date and time in the session time zone in a value of datatype TIMESTAMP | ```select LOCALTIMESTAMP```           | ```2017-08-10 21:36:12```  |
| CURRENT_TIME                             | Returns the current time in the session time zone, in a value of datatype TIMESTAMP WITH TIME ZONE | ```select CURRENT_TIME```             | ```21:36:54```             |
| CURRENT_DATE                             | Returns the current date in the session time zone, in a value of datatype DATE | ```select CURRENT_DATE```             | ```2017-08-10```           |
| CURRENT_TIMESTAMP                        | Returns the current date and time in the session time zone, in a value of datatype TIMESTAMP WITH TIME ZONE | ```select CURRENT_TIMESTAMP```        | ``` 2017-08-10 21:37:44``` |
| EXTRACT(timeUnit FROM datetime)          | Extracts and returns the value of a specified datetime field from a datetime value expression |                                       |                            |
| FLOOR(datetime TO timeUnit)              | Rounds *datetime* down to *timeUnit*     |                                       |                            |
| CEIL(datetime TO timeUnit)               | Rounds *datetime* up to *timeUnit*       |                                       |                            |
| YEAR(date)                               | Equivalent to `EXTRACT(YEAR FROM date)`. Returns an integer. | ```select YEAR(CURRENT_DATE)```       | ```2017```                 |
| QUARTER(date)                            | Equivalent to `EXTRACT(QUARTER FROM date)`. Returns an integer between 1 and 4. | ```select QUARTER(CURRENT_DATE)```    | ```3```                    |
| MONTH(date)                              | Equivalent to `EXTRACT(MONTH FROM date)`. Returns an integer between 1 and 12. | ```select MONTH(CURRENT_DATE)```      | ```8```                    |
| WEEK(date)                               | Equivalent to `EXTRACT(WEEK FROM date)`. Returns an integer between 1 and 53. |                                       |                            |
| DAYOFYEAR(date)                          | Equivalent to `EXTRACT(DOY FROM date)`. Returns an integer between 1 and 366. |                                       |                            |
| DAYOFMONTH(date)                         | Equivalent to `EXTRACT(DAY FROM date)`. Returns an integer between 1 and 31. | ```select DAYOFMONTH(CURRENT_DATE)``` | ```10```                   |
| DAYOFWEEK(date)                          | Equivalent to `EXTRACT(DOW FROM date)`. Returns an integer between 1 and 7. |                                       |                            |
| HOUR(date)                               | Equivalent to `EXTRACT(HOUR FROM date)`. Returns an integer between 0 and 23. | ```select HOUR(CURRENT_TIME)```       | ```21```                   |
| MINUTE(date)                             | Equivalent to `EXTRACT(MINUTE FROM date)`. Returns an integer between 0 and 59. | ```select MINUTE(CURRENT_TIME)```     | ```46```                   |
| SECOND(date)                             | Equivalent to `EXTRACT(SECOND FROM date)`. Returns an integer between 0 and 59. | ```select SECOND(CURRENT_TIME)```     | ```40```                   |
| TIMESTAMPADD(timeUnit, integer, datetime) | Returns *datetime* with an interval of (signed) *integer* *timeUnit*s added. Equivalent to `datetime + INTERVAL 'integer' timeUnit` |                                       |                            |
| TIMESTAMPDIFF(timeUnit, datetime, datetime2) | Returns the (signed) number of *timeUnit*intervals between *datetime* and *datetime2*. Equivalent to `(datetime2 - datetime) timeUnit` |                                       |                            |

### 