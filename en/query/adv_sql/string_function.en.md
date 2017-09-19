## String Function

| Function Syntax                          | Decription                               | Example                                  | Return            |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ----------------- |
| CHAR_LENGTH(string)                      | Returns the number of characters in a character string | ```select char_length(name) from KYLIN_COUNTRY where name='Fiji'``` | ```4```           |
| CHARACTER_LENGTH(string)                 | As CHAR_LENGTH(*string*)                 | ```select CHARACTER_LENGTH(name) from KYLIN_COUNTRY where name='Fiji'``` | ```4```           |
| UPPER(string)                            | Returns a character string converted to upper case | ```select UPPER(name) from KYLIN_COUNTRY where name='Fiji'``` | ```FIJI```        |
| LOWER(string)                            | Returns a character string converted to lower case | ```select LOWER(name) from KYLIN_COUNTRY where name='Fiji'``` | ```fiji```        |
| POSITION(string1 IN string2)             | Returns the position of the first occurrence of *string1* in *string2* | ```select POSITION('ji' in name)  from KYLIN_COUNTRY where name='Fiji'``` | ```3```           |
| POSITION(string1 IN string2 FROM integer) | Returns the position of the first occurrence of *string1* in *string2*starting at a given point (not standard SQL) | ```select POSITION('ji' in name from 1)  from KYLIN_COUNTRY where name='Fiji'``` | ```3```           |
| TRIM( { BOTH \ LEADING\  TRAILING } string1 FROM string2) | Removes the longest string containing only the characters in *string1* from the start/end/both ends of *string1* |                                          |                   |
| OVERLAY(string1 PLACING string2 FROM integer [ FOR integer2 ]) | Replaces a substring of *string1* with *string2* | ```select OVERLAY (name PLACING 'yes' from 2 for 2) from KYLIN_COUNTRY where name='Fiji'``` | ```Fyesi```       |
| SUBSTRING(string FROM integer)           | Returns a substring of a character string starting at a given point | ```Select SUBSTRING(name from 3) from KYLIN_COUNTRY where name='Fiji'``` | ```ji```          |
| SUBSTRING(string FROM integer FOR integer) | Returns a substring of a character string starting at a given point with a given length | ```select SUBSTRING(name from 3 for 2) from KYLIN_COUNTRY where name='Fiji'``` | ``` ji```         |
| INITCAP(string)                          | Returns *string* with the first letter of each word converter to upper case and the rest to lower case. Words are sequences of alphanumeric characters separated by non-alphanumeric characters. | ```select INITCAP('hello world')```      | ```Hello World``` |
| REPLACE(string1,string2, string3 )       | Replace substring *string2* of *string1* with *string3* | `select replace(NAME,'China','Hello') from KYLIN_COUNTRY where NAME='China'` | `Hello`           |

### 