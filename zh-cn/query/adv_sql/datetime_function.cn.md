### 日期函数

| Function Syntax                          | Decription                               | Example                                  | Return                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | -------------------------- |
| LOCALTIME                                | 返回当前时区的时间,返回类型为TIME格式                    | ```select LOCALTIME```                   | ```14:34:06```             |
| LOCALTIMESTAMP                           | Returns the current date and time in the session time zone in a value of datatype TIMESTAMP  返回当前时区的日期时间，返回类型为TIMESTAMP | ```select LOCALTIMESTAMP```              | ``` 2017-10-20 14:34:29``` |
| CURRENT_TIME                             | 返回当前时区的时间，返回类型为带时区的TIMESTAMP             | ```select CURRENT_TIME```                | ```14:34:30```             |
| CURRENT_DATE                             | DATE 返回当前时区的日期，返回类型为DATE                 | ```select CURRENT_DATE```                | ```2017-10-20```           |
| CURRENT_TIMESTAMP                        | TIMESTAMP WITH TIME ZONE 返回当前时区的日期时间，返回类型为带时区的TIMESTAMP | ```select CURRENT_TIMESTAMP```           | ```2017-10-20 14:41:09```  |
| EXTRACT(timeUnit FROM datetime)          | 提取返回日期中的指定日期项。 timeunit处可以填写year、month、day、hour、minute、second. | ```select EXTRACT(year FROM date'2012-01-01')``` | ```2012```                 |
| FLOOR(datetime TO timeUnit)              | 以*timeUnit*为单位对*datetime*向下取整。timeunit处可以填写year、month、day、hour、minute、second. |                                          |                            |
| CEIL(datetime TO timeUnit)               | 以*timeUnit*为单位对*datetime*向上取整。timeunit处可以填写year、month、day、hour、minute、second. |                                          |                            |
| YEAR(date)                               | 返回日期中的年份等同于EXTRACT(YEAR FROM date)，返回类型为integrer | ```select YEAR(CURRENT_DATE)```          | ```2017```                 |
| QUARTER(date)                            | Returns an integer between 1 and 4. 返回日期中的季度，返回值为1到4的整数，等同于 ```EXTRACT(QUARTER FROM date)```. | ```select QUARTER(CURRENT_DATE)```       | ```4```                    |
| MONTH(date)                              | 返回日期中的月份，返回值为1到12的整数，等同于```EXTRACT(MONTH FROM date)```. | ```select MONTH(CURRENT_DATE)```         | ```10```                   |
| WEEK(date)                               | 返回日期中的星期是一年的第几个星期，返回值为1到53的整数，等同于```EXTRACT(WEEK FROM date)``` | ```select week(date'2012-01-01')```      | ```52```                   |
| DAYOFYEAR(date)                          | 返回日期是一年中的第几天，返回值为1到366的整数，等同于```EXTRACT(DOY FROM date)```. | ```select DAYOFYEAR(date'2012-01-01')``` | ```1```                    |
| DAYOFMONTH(date)                         | 返回日期是一月中的第几天，返回值为1到31，等同于```EXTRACT(DAY FROM date)``` | ```select DAYOFMONTH(CURRENT_DATE)```    | ```20```                   |
| DAYOFWEEK(date)                          | 返回日期是星期几，返回值为1到7的整数，等同于```EXTRACT(DOW FROM date)```. | ```select DAYOFWEEK(date'2017-10-20')``` | ```6```                    |
| HOUR(date)                               | 返回日期中的小时数，返回值为0到23的整数，等同于 ```EXTRACT(HOUR FROM date)```. | ```select HOUR(CURRENT_TIME)```          | ```15```                   |
| MINUTE(date)                             | 返回日期中的分钟数，返回结果为0到59的整数，等同于```EXTRACT(MINUTE FROM date)``` | ```select MINUTE(CURRENT_TIME)```        | ```7```                    |
| SECOND(date)                             | 返回日期中的秒数，返回结果为0到59的整数，等同于```EXTRACT(SECOND FROM date)```. | ```select SECOND(CURRENT_TIME)```        | ```28```                   |
| TIMESTAMPADD(timeUnit, integer, datetime) | 返回添加了*timeUnit*s为单位的时间integer后的日期*datetime*,返回类型为*datetime*,等同于```datetime + INTERVAL 'integer' timeUnit``` 。 如示例中的函数返回日期part_dt加一个月。 | ```select TIMESTAMPADD(month, 1, date'2012-01-01'),date'2012-01-01'``` | ``` 2012-02-01```          |
| TIMESTAMPDIFF(timeUnit, datetime, datetime2) | 返回*datetime*和*datetime2*的时间差，以timeUnit为单位返回，等同于```(datetime2 - datetime) timeUnit``` | ```select TIMESTAMPDIFF(month, date'2012-01-01', date '2012-02-01')``` | ```1```                    |