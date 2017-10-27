## Query REST API

> **Tip**
>
> Before using API, please make sure that you have read the Access and Authentication in advance and know how to add verification information. 
>

If users access dataset built by KAP, mainly there are two API, one is querying data from Cube, another one is listing all available tables.

* Query
   * [Query Cube](#query-data-from-cube)
   * [List queryable tables](#list-queryable-tables)

## Query Data from Cube
`Request Mode POST`

`Access Path http://host:port/kylin/api/query`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Request Body
* sql - `required` `string` 

  Sql query sentences.

* offset - `optional` `int` 

  Query defaults to send results back from the first line, which could be changed to return from any line desired.

* limit - `optional` `int` 

  If limit parameter is chosen, query results would turn with corresponding lines from offset; if actual lines are less than limit set, then results would be just as actual lines.

* acceptPartial - `optional` `bool` 
  Mandatory is "true". If let it be true, then query results turned would be one billion lines at most; if users need more than one billion lines, then this parameter should be set as "false".

* project - `optional` `string` 
  Mandatory setting is "DEFAULT". In practice, if the query project is not "DEFAULT", then users need to set it as the project desired.


#### Request Example
```sh
{  
   "sql":"select * from TEST_KYLIN_FACT",
   "offset":0,
   "limit":50000,
   "acceptPartial":false,
   "project":"DEFAULT"
}
```

#### Response Information
* columnMetas - metadata information for each column.
* results - turned result set.
* cube - the Cube corresponding to query.
* affectedRowCount -  the total number of rows related to this query. 
* isException - whether the query result is exceptional.
* ExceptionMessage - turned corresponding exception information.
* totalScanCount - total counts.
* totalScanBytes - total bytes.
* hitExceptionCache - whether from the result cache which executes failed.
* storageCacheUsed - whether from the result cache which executes successfully.
* Duration - query consumed time.
* Partial - whether query results are partial return depends on acceptPartial is true or false.
* pushDown - whether enable the action of push down.

#### Response Example
```sh
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

#### Curl Access Example
```
curl -X POST -H "Authorization: Basic XXXXXXXXX" -H "Content-Type: application/json" -d '{ "sql":"select count(*) from TEST_KYLIN_FACT", "project":"learn_kylin" }' http://YOUR_HOST:7070/kylin/api/query
```


## List Queryable Tables
`Request Mode GET`

`Access Path http://host:port/kylin/api/tables_and_columns`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Request Parameter
* project - `required` `string`, indicates desired tables are from which project.

#### Response Example
```sh
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

