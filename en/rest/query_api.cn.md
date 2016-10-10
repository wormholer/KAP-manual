## 查询 REST API

> **提示**
> 
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
> 

访问KAP构建的数据集，主要两个API，一个是直接查询Cube数据，一个是列出所有可以查询的表。

* 查询
   * [查询Cube数据](#query)
   * [列出可查询的表](#list-queryable-tables)


## <span id="query">查询Cube数据</span>
`请求方式 POST`

`访问路径 http://host:port/kylin/api/query`

#### 请求主体
* sql - `必选` `string` 查询的sql.
* offset - `可选` `int` 查询默认从第一行返回结果，可以设置改参数设置返回数据从哪一行开始往后返回
* limit - `可选` `int` •  加上limit参数后会从offset开始返回对应的行数，不足limt以实际行数为准
* acceptPartial - `可选` `bool` 
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
* affectedRowCount - 这个查询关系到的总行树.
* isException - 这个查询返回是否是异常.
* ExceptionMessage - 返回异常对应的内容.
* Duration - 查询消耗时间
* Partial - 这个查询结果是否为部分返回，这个取决于请求参数中的 acceptPartial 为true或者false.

#### 响应示例
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

#### Curl 访问示例
```
curl -X POST -H "Authorization: Basic XXXXXXXXX" -H "Content-Type: application/json" -d '{ "sql":"select count(*) from TEST_KYLIN_FACT", "project":"learn_kylin" }' http://YOUR_HOST:7070/kylin/api/query
```


## <span id="list-queryable-tables">列出可查询的表</span>
`请求方式 GET`

`访问路径 http://host:port/kylin/api/tables_and_columns`

#### 请求参数
* project - `必选` `string` 说明对应要列出哪个Project下的表 

#### 响应示例
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

