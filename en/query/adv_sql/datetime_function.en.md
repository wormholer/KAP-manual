### Date and Time Function

| Function Syntax                          | Decription                               | Example                                  |                            |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | -------------------------- |
| LOCALTIME                                | Returns the current date and time in the session time zone in a value of datatype TIME | ```select LOCALTIME```                   | ```14:34:06```             |
| LOCALTIMESTAMP                           | Returns the current date and time in the session time zone in a value of datatype TIMESTAMP | ```select LOCALTIMESTAMP```              | ``` 2017-10-20 14:34:29``` |
| CURRENT_TIME                             | Returns the current time in the session time zone, in a value of datatype TIMESTAMP WITH TIME ZONE | ```select CURRENT_TIME```                | ```14:34:30```             |
| CURRENT_DATE                             | Returns the current date in the session time zone, in a value of datatype DATE | ```select CURRENT_DATE```                | ```2017-10-20```           |
| CURRENT_TIMESTAMP                        | Returns the current date and time in the session time zone, in a value of datatype TIMESTAMP WITH TIME ZONE | ```select CURRENT_TIMESTAMP```           | ```2017-10-20 14:41:09```  |
| EXTRACT(timeUnit FROM datetime)          | Extracts and returns the value of a specified datetime field from a datetime value expression | ```select EXTRACT(year FROM date'2012-01-01')``` | ```2012```                 |
| FLOOR(datetime TO timeUnit)              | Rounds *datetime* down to *timeUnit*     |                                          |                            |
| CEIL(datetime TO timeUnit)               | Rounds *datetime* up to *timeUnit*       |                                          |                            |
| YEAR(date)                               | Equivalent to ```EXTRACT(YEAR FROM date)```. Returns an integer. | ```select YEAR(CURRENT_DATE)```          | ```2017```                 |
| QUARTER(date)                            | Equivalent to ```EXTRACT(QUARTER FROM date)```. Returns an integer between 1 and 4. | ```select QUARTER(CURRENT_DATE)```       | ```4```                    |
| MONTH(date)                              | Equivalent to ```EXTRACT(MONTH FROM date)```. Returns an integer between 1 and 12. | ```select MONTH(CURRENT_DATE)```         | ```10```                   |
| WEEK(date)                               | Equivalent to ```EXTRACT(WEEK FROM date)```. Returns an integer between 1 and 53. | ```select week(date'2012-01-01')```      | ```52```                   |
| DAYOFYEAR(date)                          | Equivalent to ```EXTRACT(DOY FROM date)```. Returns an integer between 1 and 366. | ```select DAYOFYEAR(date'2012-01-01')``` | ```1```                    |
| DAYOFMONTH(date)                         | Equivalent to ```EXTRACT(DAY FROM date)```. Returns an integer between 1 and 31. | ```select DAYOFMONTH(CURRENT_DATE)```    | ```20```                   |
| DAYOFWEEK(date)                          | Equivalent to ```EXTRACT(DOW FROM date)```. Returns an integer between 1 and 7. | ```select DAYOFWEEK(date'2017-10-20')``` | ```6```                    |
| HOUR(date)                               | Equivalent to ```EXTRACT(HOUR FROM date)```. Returns an integer between 0 and 23. | ```select HOUR(CURRENT_TIME)```          | ```15```                   |
| MINUTE(date)                             | Equivalent to ```EXTRACT(MINUTE FROM date)```. Returns an integer between 0 and 59. | ```select MINUTE(CURRENT_TIME)```        | ```7```                    |
| SECOND(date)                             | Equivalent to ```EXTRACT(SECOND FROM date)```. Returns an integer between 0 and 59. | ```select SECOND(CURRENT_TIME)```        | ```28```                   |
| TIMESTAMPADD(timeUnit, integer, datetime) | Returns *datetime* with an interval of (signed) *integer* *timeUnit*s added. Equivalent to ```datetime + INTERVAL 'integer' timeUnit``` | ```select TIMESTAMPADD(month, 1, date'2012-01-01'),date'2012-01-01'``` | ``` 2012-02-01```          |
| TIMESTAMPDIFF(timeUnit, datetime, datetime2) | Returns the (signed) number of *timeUnit*intervals between *datetime* and *datetime2*. Equivalent to ```(datetime2 - datetime) timeUnit``` | ```select TIMESTAMPDIFF(month, date'2012-01-01', date '2012-02-01')``` | ```1```                    |
