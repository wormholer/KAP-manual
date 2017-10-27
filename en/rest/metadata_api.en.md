## Metadata REST API
> **Tip**
>
> Before using API, please make sure that you have read the Access and Authentication in advance and know how to add verification information. 
>


* [Get multiple Hive tables](#get-multiple-hive-tables)
* [Get Hive table information](#get-hive-table-information)
* [Load Hive tables](#load-hive-tables)

### Get multiple Hive tables
`Request Mode GET`

`Access Path http://host:port/kylin/api/tables`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Request Parameter
* project - `required` `string`, project name.
* ext - `optional` `string`, specify if table's extension information is returned.

#### Response Example
```sh
{
    "code": "000",
    "data": [
        {
            "uuid": "e286e39e-40d7-44c2-8fa2-41b365632882",
            "last_modified": 1507690958000,
            "version": "2.3.0.20500",
            "name": "TEST_COUNTRY",
            "columns": [
                {
                    "id": "1",
                    "name": "COUNTRY",
                    "datatype": "varchar(256)",
                    "index": "T"
                },
                {
                    "id": "2",
                    "name": "LATITUDE",
                    "datatype": "double"
                },
                {
                    "id": "3",
                    "name": "LONGITUDE",
                    "datatype": "double"
                },
                {
                    "id": "4",
                    "name": "NAME",
                    "datatype": "varchar(256)",
                    "index": "T"
                }
            ],
            "source_type": 0,
            "table_type": null,
            "database": "DEFAULT",
            "exd": {},
            "cardinality": {
                "CRE_DATE": 31,
                "SITE_ID": 270,
                "SITES_UPD_DATE": 3,
                "SITE_DOMAIN_CODE": 2,
                "EOA_EMAIL_CSTMZBL_SITE_YN_ID": 2,
                "SITE_CNTRY_ID": 225,
                "DFAULT_LSTG_CURNCY": 31,
                "CRE_USER": 8,
                "SITE_NAME": 271,
                "SITES_UPD_USER": 2
            }
        }
    ],
    "msg": ""
}
```

### Get Hive table information
`Request Mode GET`

`Access Path http://host:port/kylin/api/tables/{project}/{tableName}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Request Parameter
* project - `optional` `string`, project name.
* tableName - `optional` `string`, table name.

#### Response Example
```
{
    "code": "000",
    "data": {
        "uuid": "e286e39e-40d7-44c2-8fa2-41b365522771",
        "last_modified": 1508819599000,
        "version": "2.3.0.20500",
        "name": "TEST_KYLIN_FACT",
        "columns": [
            {
                "id": "1",
                "name": "TRANS_ID",
                "datatype": "bigint",
                "data_gen": "ID"
            },
            {
                "id": "2",
                "name": "ORDER_ID",
                "datatype": "bigint",
                "index": "T"
            },
            {
                "id": "3",
                "name": "CAL_DT",
                "datatype": "date",
                "data_gen": "FK,order",
                "index": "T"
            },
            {
                "id": "4",
                "name": "LSTG_FORMAT_NAME",
                "datatype": "varchar(256)",
                "data_gen": "FP-GTC|FP-non GTC|ABIN|Auction|Others",
                "index": "T"
            },
            {
                "id": "5",
                "name": "LEAF_CATEG_ID",
                "datatype": "bigint",
                "data_gen": "FK,null,nullstr=0",
                "index": "T"
            },
            {
                "id": "6",
                "name": "LSTG_SITE_ID",
                "datatype": "integer",
                "index": "T"
            },
            {
                "id": "7",
                "name": "SLR_SEGMENT_CD",
                "datatype": "smallint",
                "data_gen": "FK,pk=EDW.TEST_SELLER_TYPE_DIM_TABLE.SELLER_TYPE_CD",
                "index": "T"
            },
            {
                "id": "8",
                "name": "SELLER_ID",
                "datatype": "integer",
                "data_gen": "RAND||10000000|10001000",
                "index": "T"
            },
            {
                "id": "9",
                "name": "PRICE",
                "datatype": "decimal(19,4)",
                "data_gen": "RAND|.##|-100|1000"
            },
            {
                "id": "10",
                "name": "ITEM_COUNT",
                "datatype": "integer",
                "data_gen": "RAND"
            },
            {
                "id": "11",
                "name": "TEST_COUNT_DISTINCT_BITMAP",
                "datatype": "varchar(256)",
                "data_gen": "RAND"
            }
        ],
        "source_type": 0,
        "table_type": null,
        "data_gen": "1",
        "database": "DEFAULT"
    },
    "msg": ""
}
```

### Load Hive tables
`Request Mode POST`

`Access Path http://host:port/kylin/api/tables/load`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Request Parameter
* project - `required` `string`, specify which project the hive table will be loaded to.
* table - `required` `string`, the hive table to be loaded, seperated by comma.

#### Response Example
```sh
{
    "code": "000",
    "data": {
        "result.loaded": [
            "DEFAULT.TEST_KYLIN_FACT"
        ],
        "result.unloaded": []
    },
    "msg": ""
}
```
