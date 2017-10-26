## 查询 REST API

> **提示**
>
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
>

访问KAP构建的数据集，主要两个API，一个是直接查询Cube数据，一个是列出所有可以查询的表。

* 查询
   * [查询Cube数据](#查询cube数据)
   * [列出可查询的表](#列出可查询的表)


### 查询Cube数据
`请求方式 POST`

`访问路径 http://host:port/kylin/api/query`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### 请求主体
* sql - `必选` `string` 查询的sql.
* offset - `可选` `int` 查询默认从第一行返回结果，可以设置改参数设置返回数据从哪一行开始往后返回
* limit - `可选` `int` •  加上limit参数后会从offset开始返回对应的行数，不足limt以实际行数为准

* acceptPartial - `可选` `bool` (该参数在KAP2.1之后已无效 )
  默认是“true”，如果为true，那么当前实际最多会返回一百万行数据，如果要返回超过一百万行的结果集，那该参数需要设置为“false”
* project - `可选` `string` 
  默认为 ‘DEFAULT’，在实际使用时，如果对应查询的项目不是“DEFAULT”，需要设置为自己的项目


#### 请求示例
```sh
{  
   "sql":"select * from TEST_KYLIN_FACT",
   "offset":0,
   "limit":50000,
   "acceptPartial":false,
   "project":"DEFAULT"
}
```

#### 响应信息
* columnMetas - 每个列的元数据信息.
* results - 返回的结果集.
* cube - 这个查询对应使用的CUBE.
* affectedRowCount - 这个查询关系到的总行数.
* isException - 这个查询返回是否是异常.
* ExceptionMessage - 返回异常对应的内容.
* totalScanCount - 总记录数
* totalScanBytes - 总字节数
* hitExceptionCache - 是否来自执行失败的结果缓存
* storageCacheUsed - 是否来自执行成功的结果缓存
* Duration - 查询消耗时间
* Partial - 这个查询结果是否为部分返回，这个取决于请求参数中的 acceptPartial 为true或者false.
* pushDown - 是否启用查询下压

#### 响应示例
```json
{
    "code":"000",
    "data":{
        "columnMetas":[
            {
                "isNullable":1,
                "displaySize":19,
                "label":"TRANS_ID",
                "name":"TRANS_ID",
                "schemaName":"DEFAULT",
                "catelogName":null,
                "tableName":"TEST_KYLIN_FACT",
                "precision":19,
                "scale":0,
                "columnType":-5,
                "columnTypeName":"BIGINT",
                "readOnly":true,
                "signed":true,
                "writable":false,
                "autoIncrement":false,
                "caseSensitive":true,
                "searchable":false,
                "currency":false,
                "definitelyWritable":false
            }
        ],
        "results":[
            [
                "0",
                "27",
                "2012-01-01",
                "ABIN",
                "223",
                "0",
                "12",
                "10000843",
                "TEST322"
            ]
        ],
        "cube":"CUBE[name=mp_cube1]",
        "affectedRowCount":0,
        "isException":false,
        "exceptionMessage":null,
        "duration":1046,
        "totalScanCount":2014,
        "totalScanBytes":30210,
        "hitExceptionCache":false,
        "storageCacheUsed":false,
        "partial":false,
        "pushDown":false
    },
    "msg":""
}
```

#### Curl 访问示例
```
curl -X POST -H "Authorization: Basic XXXXXXXXX" -H "Content-Type: application/json" -d '{ "sql":"select count(*) from TEST_KYLIN_FACT", "project":"learn_kylin" }' http://YOUR_HOST:7070/kylin/api/query
```


### 列出可查询的表
`请求方式 GET`

`访问路径 http://host:port/kylin/api/tables_and_columns`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### 请求参数
* project - `必选` `string` 说明对应要列出哪个Project下的表 

#### 响应示例
```json
{
    "code": "000",
    "data": [
        {
            "columns": [
                {
                    "type": [
                        "DIMENSION"
                    ],
                    "column_NAME": "MINUTE_START",
                    "data_TYPE": 93,
                    "column_SIZE": 0,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": 0,
                    "ordinal_POSITION": 1,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "STREAMING_TABLE",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "TIMESTAMP(0)"
                },
                {
                    "type": [
                        "DIMENSION"
                    ],
                    "column_NAME": "HOUR_START",
                    "data_TYPE": 93,
                    "column_SIZE": 0,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": 0,
                    "ordinal_POSITION": 2,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "STREAMING_TABLE",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "TIMESTAMP(0)"
                },
                {
                    "type": [
                        "DIMENSION"
                    ],
                    "column_NAME": "DAY_START",
                    "data_TYPE": 91,
                    "column_SIZE": -1,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": -1,
                    "ordinal_POSITION": 3,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "STREAMING_TABLE",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "DATE"
                },
                {
                    "type": [
                        "DIMENSION"
                    ],
                    "column_NAME": "ITM",
                    "data_TYPE": 12,
                    "column_SIZE": 256,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": 256,
                    "ordinal_POSITION": 4,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "STREAMING_TABLE",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "VARCHAR(256) CHARACTER SET \"UTF-16LE\" COLLATE \"UTF-16LE$en_US$primary\""
                },
                {
                    "type": [
                        "DIMENSION"
                    ],
                    "column_NAME": "SITE",
                    "data_TYPE": 12,
                    "column_SIZE": 256,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": 256,
                    "ordinal_POSITION": 5,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "STREAMING_TABLE",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "VARCHAR(256) CHARACTER SET \"UTF-16LE\" COLLATE \"UTF-16LE$en_US$primary\""
                },
                {
                    "type": [
                        "MEASURE"
                    ],
                    "column_NAME": "GMV",
                    "data_TYPE": 3,
                    "column_SIZE": 19,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 6,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": 19,
                    "ordinal_POSITION": 6,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "STREAMING_TABLE",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "DECIMAL(19, 6)"
                },
                {
                    "type": [
                        "MEASURE"
                    ],
                    "column_NAME": "ITEM_COUNT",
                    "data_TYPE": -5,
                    "column_SIZE": -1,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": -1,
                    "ordinal_POSITION": 7,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "STREAMING_TABLE",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "BIGINT"
                }
            ],
            "type": [
                "FACT"
            ],
            "table_NAME": "STREAMING_TABLE",
            "table_SCHEM": "DEFAULT",
            "table_CAT": "defaultCatalog",
            "table_TYPE": "TABLE",
            "remarks": null,
            "type_CAT": null,
            "type_SCHEM": null,
            "type_NAME": null,
            "self_REFERENCING_COL_NAME": null,
            "ref_GENERATION": null
        },
        {
            "columns": [
                {
                    "type": [
                        "DIMENSION"
                    ],
                    "column_NAME": "TRANS_ID",
                    "data_TYPE": -5,
                    "column_SIZE": -1,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": -1,
                    "ordinal_POSITION": 1,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "TEST_KYLIN_FACT",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "BIGINT"
                },
                {
                    "type": [
                        "DIMENSION",
                        "FK"
                    ],
                    "column_NAME": "ORDER_ID",
                    "data_TYPE": -5,
                    "column_SIZE": -1,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": -1,
                    "ordinal_POSITION": 2,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "TEST_KYLIN_FACT",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "BIGINT"
                },
                {
                    "type": [
                        "DIMENSION",
                        "FK"
                    ],
                    "column_NAME": "CAL_DT",
                    "data_TYPE": 91,
                    "column_SIZE": -1,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": -1,
                    "ordinal_POSITION": 3,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "TEST_KYLIN_FACT",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "DATE"
                },
                {
                    "type": [
                        "DIMENSION"
                    ],
                    "column_NAME": "LSTG_FORMAT_NAME",
                    "data_TYPE": 12,
                    "column_SIZE": 256,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": 256,
                    "ordinal_POSITION": 4,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "TEST_KYLIN_FACT",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "VARCHAR(256) CHARACTER SET \"UTF-16LE\" COLLATE \"UTF-16LE$en_US$primary\""
                },
                {
                    "type": [
                        "DIMENSION",
                        "FK"
                    ],
                    "column_NAME": "LEAF_CATEG_ID",
                    "data_TYPE": -5,
                    "column_SIZE": -1,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": -1,
                    "ordinal_POSITION": 5,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "TEST_KYLIN_FACT",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "BIGINT"
                },
                {
                    "type": [
                        "DIMENSION",
                        "FK"
                    ],
                    "column_NAME": "LSTG_SITE_ID",
                    "data_TYPE": 4,
                    "column_SIZE": -1,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": -1,
                    "ordinal_POSITION": 6,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "TEST_KYLIN_FACT",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "INTEGER"
                },
                {
                    "type": [
                        "DIMENSION",
                        "FK"
                    ],
                    "column_NAME": "SLR_SEGMENT_CD",
                    "data_TYPE": 5,
                    "column_SIZE": -1,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": -1,
                    "ordinal_POSITION": 7,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "TEST_KYLIN_FACT",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "SMALLINT"
                },
                {
                    "type": [
                        "DIMENSION",
                        "FK"
                    ],
                    "column_NAME": "SELLER_ID",
                    "data_TYPE": 4,
                    "column_SIZE": -1,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": -1,
                    "ordinal_POSITION": 8,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "TEST_KYLIN_FACT",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "INTEGER"
                },
                {
                    "type": [
                        "DIMENSION"
                    ],
                    "column_NAME": "TEST_COUNT_DISTINCT_BITMAP",
                    "data_TYPE": 12,
                    "column_SIZE": 256,
                    "buffer_LENGTH": -1,
                    "decimal_DIGITS": 0,
                    "num_PREC_RADIX": 10,
                    "nullable": 1,
                    "column_DEF": null,
                    "sql_DATA_TYPE": -1,
                    "sql_DATETIME_SUB": -1,
                    "char_OCTET_LENGTH": 256,
                    "ordinal_POSITION": 9,
                    "is_NULLABLE": "YES",
                    "scope_CATLOG": null,
                    "scope_SCHEMA": null,
                    "scope_TABLE": null,
                    "source_DATA_TYPE": -1,
                    "is_AUTOINCREMENT": "",
                    "table_NAME": "TEST_KYLIN_FACT",
                    "table_SCHEM": "DEFAULT",
                    "table_CAT": "defaultCatalog",
                    "remarks": null,
                    "type_NAME": "VARCHAR(256) CHARACTER SET \"UTF-16LE\" COLLATE \"UTF-16LE$en_US$primary\""
                }
            ],
            "type": [
                "FACT"
            ],
            "table_NAME": "TEST_KYLIN_FACT",
            "table_SCHEM": "DEFAULT",
            "table_CAT": "defaultCatalog",
            "table_TYPE": "TABLE",
            "remarks": null,
            "type_CAT": null,
            "type_SCHEM": null,
            "type_NAME": null,
            "self_REFERENCING_COL_NAME": null,
            "ref_GENERATION": null
        }
    ],
    "msg": ""
}
```

