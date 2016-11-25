## Query REST API

> **Tip**
>
> Before using API, please make sure that you have read the Access and Authentication in advance and know how to add verification information. 
>

If users access dataset built by KAP, mainly there are two API, one is querying data from Cube, another one is listing all available tables.

* Query
   * [Query Cube](#query)
   * [List queryable tables](#list-queryable-tables)


## Query Data from Cube
`Request Mode POST`

`Access Path http://host:port/kylin/api/query`

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
* columnMetas - metadata information for each column
* results - turned result set
* cube - the Cube corresponding to query
* affectedRowCount -  the total number of rows related to this query 
* isException - whether the query result is exceptional
* ExceptionMessage - turned corresponding exception information
* Duration - query consumed time
* Partial - whether query results are partial return depends on acceptPartial is true or false

#### Response Example
```sh
{  
   "columnMetas":[  
      {  
         "isNullable":1,
         "displaySize":0,
         "label":"CAL_DT",
         "name":"CAL_DT",
         "schemaName":null,
         "catelogName":null,
         "tableName":null,
         "precision":0,
         "scale":0,
         "columnType":91,
         "columnTypeName":"DATE",
         "readOnly":true,
         "writable":false,
         "caseSensitive":true,
         "searchable":false,
         "currency":false,
         "signed":true,
         "autoIncrement":false,
         "definitelyWritable":false
      },
      {  
         "isNullable":1,
         "displaySize":10,
         "label":"LEAF_CATEG_ID",
         "name":"LEAF_CATEG_ID",
         "schemaName":null,
         "catelogName":null,
         "tableName":null,
         "precision":10,
         "scale":0,
         "columnType":4,
         "columnTypeName":"INTEGER",
         "readOnly":true,
         "writable":false,
         "caseSensitive":true,
         "searchable":false,
         "currency":false,
         "signed":true,
         "autoIncrement":false,
         "definitelyWritable":false
      }
   ],
   "results":[  
      [  
         "2013-08-07",
         "32996",
         "15",
         "15",
         "Auction",
         "10000000",
         "49.048952730908745",
         "49.048952730908745",
         "49.048952730908745",
         "1"
      ],
      [  
         "2013-08-07",
         "43398",
         "0",
         "14",
         "ABIN",
         "10000633",
         "85.78317064220418",
         "85.78317064220418",
         "85.78317064220418",
         "1"
      ]
   ],
   "cube":"test_kylin_cube_with_slr_desc",
   "affectedRowCount":0,
   "isException":false,
   "exceptionMessage":null,
   "duration":3451,
   "partial":false
}
```

#### Curl Access Example
```
curl -X POST -H "Authorization: Basic XXXXXXXXX" -H "Content-Type: application/json" -d '{ "sql":"select count(*) from TEST_KYLIN_FACT", "project":"learn_kylin" }' http://YOUR_HOST:7070/kylin/api/query
```


## List Queryable Table
`Request Mode GET`

`Access Path http://host:port/kylin/api/tables_and_columns`

#### Request Parameter
* project - `required` `string` indicates desired tables are from which project 

#### Response Example
```sh
[  
   {  
      "columns":[  
         {  
            "table_NAME":"TEST_CAL_DT",
            "table_SCHEM":"EDW",
            "column_NAME":"CAL_DT",
            "data_TYPE":91,
            "nullable":1,
            "column_SIZE":-1,
            "buffer_LENGTH":-1,
            "decimal_DIGITS":0,
            "num_PREC_RADIX":10,
            "column_DEF":null,
            "sql_DATA_TYPE":-1,
            "sql_DATETIME_SUB":-1,
            "char_OCTET_LENGTH":-1,
            "ordinal_POSITION":1,
            "is_NULLABLE":"YES",
            "scope_CATLOG":null,
            "scope_SCHEMA":null,
            "scope_TABLE":null,
            "source_DATA_TYPE":-1,
            "iS_AUTOINCREMENT":null,
            "table_CAT":"defaultCatalog",
            "remarks":null,
            "type_NAME":"DATE"
         },
         {  
            "table_NAME":"TEST_CAL_DT",
            "table_SCHEM":"EDW",
            "column_NAME":"WEEK_BEG_DT",
            "data_TYPE":91,
            "nullable":1,
            "column_SIZE":-1,
            "buffer_LENGTH":-1,
            "decimal_DIGITS":0,
            "num_PREC_RADIX":10,
            "column_DEF":null,
            "sql_DATA_TYPE":-1,
            "sql_DATETIME_SUB":-1,
            "char_OCTET_LENGTH":-1,
            "ordinal_POSITION":2,
            "is_NULLABLE":"YES",
            "scope_CATLOG":null,
            "scope_SCHEMA":null,
            "scope_TABLE":null,
            "source_DATA_TYPE":-1,
            "iS_AUTOINCREMENT":null,
            "table_CAT":"defaultCatalog",
            "remarks":null,
            "type_NAME":"DATE"
         }
      ],
      "table_NAME":"TEST_CAL_DT",
      "table_SCHEM":"EDW",
      "ref_GENERATION":null,
      "self_REFERENCING_COL_NAME":null,
      "type_SCHEM":null,
      "table_TYPE":"TABLE",
      "table_CAT":"defaultCatalog",
      "remarks":null,
      "type_CAT":null,
      "type_NAME":null
   }
]
```

