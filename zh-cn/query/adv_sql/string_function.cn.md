## 字符串函数

| Function Syntax                          | Decription                             | Example                                  | Return            |
| ---------------------------------------- | -------------------------------------- | ---------------------------------------- | ----------------- |
| CHAR_LENGTH(string)                      | 返回字符串长度                                | ```select char_length(name) from KYLIN_COUNTRY where name='Fiji'``` | ```4```           |
| CHARACTER_LENGTH(string)                 | 同CHAR_LENGTH(*string*)                 | ```select CHARACTER_LENGTH(name) from KYLIN_COUNTRY where name='Fiji'``` | ```4```           |
| UPPER(string)                            | 返回字符串为全大写                              | ```select UPPER(name) from KYLIN_COUNTRY where name='Fiji'``` | ```FIJI```        |
| LOWER(string)                            | 返回字符串为全小写                              | ```select LOWER(name) from KYLIN_COUNTRY where name='Fiji'``` | ```fiji```        |
| POSITION(string1 IN string2)             | 返回*string1*在*string2*中的位置              | ```select POSITION('ji' in name)  from KYLIN_COUNTRY where name='Fiji'``` | ```3```           |
| POSITION(string1 IN string2 FROM integer) | 返回*string1*在*string2*中从指定位置开始起的位置      | ```select POSITION('ji' in name from 1)  from KYLIN_COUNTRY where name='Fiji'``` | ```3```           |
| TRIM( { BOTH \ LEADING\  TRAILING } string1 FROM string2) | 去掉*string2*开头／结尾／两头最长的一个字符串*string1*   | ```select trim( BOTH 'H' from 'Hello' )```   | ``` ello```           |
| OVERLAY(string1 PLACING string2 FROM integer [ FOR integer2 ]) | 从*string1*第integer位开始将字符替换为*string2*   | ```select OVERLAY (name PLACING 'yes' from 2 for 2) from KYLIN_COUNTRY where name='Fiji'``` | ```Fyesi```       |
| SUBSTRING(string FROM integer)           | 从第integer位开始，取string的部分字符串。            | ```Select SUBSTRING(name from 3) from KYLIN_COUNTRY where name='Fiji'``` | ```ji```          |
| SUBSTRING(string FROM integer1 FOR integer2) | 从第integer1位开始，取string中的integer2 个字符    | ```select SUBSTRING(name from 3 for 2) from KYLIN_COUNTRY where name='Fiji'``` | ``` ji```         |
| INITCAP(string)                          | 将字符串的首字母z换成大写                          | ```select INITCAP('hello world')```      | ```Hello World``` |
| REPLACE(string1,string2, string3 )       | 将字符串*string1*中的字符*string2*替换为*string3* | ```select replace(NAME,'China','Hello') from KYLIN_COUNTRY where NAME='China'``` | ```Hello```           |

###  