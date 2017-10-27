## Model REST API

> **Tip**
>
> Before using API, please make sure that you have read the Access and Authentication in advance and know how to add verification information. 
>


* [List Model](#list-model)
* [Clone Model](#clone-model)
* [Drop Model](#drop-model)

### List Model
`Request Mode GET`

`Access Path http://host:port/kylin/api/models`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Request Body
* offset - `optional` `int`, default 0, get data start subscript.
* limit - `optional` `int `, default 10, how many lines would be included in each returned page.
* modelName - `optional` `string`, returned name is the keyword related model.
* exactMatch - `optional` `boolean`, default true, specify whether matching exactly with modelName.
* projectName - `optional` `string`, specify the returned project.

#### Response Example
```sh
{
    "code": "000",
    "data": {
        "models": [
            {
                "uuid": "63c20e51-c580-49dd-b1dc-7bec621c7b03",
                "last_modified": 1509006427481,
                "version": "2.3.0.20500",
                "name": "m1",
                "owner": "ADMIN",
                "is_draft": false,
                "description": "",
                "fact_table": "DEFAULT.TEST_KYLIN_FACT",
                "lookups": [],
                "dimensions": [
                    {
                        "table": "TEST_KYLIN_FACT",
                        "columns": [
                            "TRANS_ID",
                            "ORDER_ID",
                            "CAL_DT",
                            "LSTG_FORMAT_NAME",
                            "LEAF_CATEG_ID",
                            "LSTG_SITE_ID",
                            "SLR_SEGMENT_CD",
                            "SELLER_ID",
                            "TEST_COUNT_DISTINCT_BITMAP"
                        ]
                    }
                ],
                "metrics": [
                    "TEST_KYLIN_FACT.PRICE",
                    "TEST_KYLIN_FACT.ITEM_COUNT"
                ],
                "filter_condition": "",
                "partition_desc": {
                    "partition_date_column": "TEST_KYLIN_FACT.CAL_DT",
                    "partition_time_column": null,
                    "partition_date_start": 0,
                    "partition_date_format": "yyyy-MM-dd",
                    "partition_time_format": "",
                    "partition_type": "APPEND",
                    "partition_condition_builder": "io.kyligence.kap.cube.mp.MPSqlCondBuilder"
                },
                "capacity": "MEDIUM",
                "multilevel_partition_cols": [
                    "TEST_KYLIN_FACT.LSTG_FORMAT_NAME"
                ],
                "computed_columns": [],
                "project": "default"
            }
        ],
        "size": 1
    },
    "msg": ""
}   
```

### Clone Model
`Request Mode PUT`

`Access Path http://host:port/kylin/api/models/{modelName}/clone`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* modelName - `required` `string`, the name of the cloned model.

#### Request Body
* modelName - `required` `string`, new models' name.
* project - `required` `string`, new projects' name. 


#### Response Example
```sh
 {
    "code": "000",
    "data": {
        "modelDescData": "{\n  \"uuid\" : \"60f4e30e-f50f-4abf-8ad2-4f34233aae21\",\n  \"last_modified\" : 1509010496630,\n  \"version\" : \"2.3.0.20500\",\n  \"name\" : \"m2\",\n  \"owner\" : \"ADMIN\",\n  \"is_draft\" : false,\n  \"description\" : \"\",\n  \"fact_table\" : \"DEFAULT.TEST_KYLIN_FACT\",\n  \"lookups\" : [ ],\n  \"dimensions\" : [ {\n    \"table\" : \"TEST_KYLIN_FACT\",\n    \"columns\" : [ \"TRANS_ID\", \"ORDER_ID\", \"CAL_DT\", \"LSTG_FORMAT_NAME\", \"LEAF_CATEG_ID\", \"LSTG_SITE_ID\", \"SLR_SEGMENT_CD\", \"SELLER_ID\", \"TEST_COUNT_DISTINCT_BITMAP\" ]\n  } ],\n  \"metrics\" : [ \"TEST_KYLIN_FACT.PRICE\", \"TEST_KYLIN_FACT.ITEM_COUNT\" ],\n  \"filter_condition\" : \"\",\n  \"partition_desc\" : {\n    \"partition_date_column\" : \"TEST_KYLIN_FACT.CAL_DT\",\n    \"partition_time_column\" : null,\n    \"partition_date_start\" : 0,\n    \"partition_date_format\" : \"yyyy-MM-dd\",\n    \"partition_time_format\" : \"\",\n    \"partition_type\" : \"APPEND\",\n    \"partition_condition_builder\" : \"io.kyligence.kap.cube.mp.MPSqlCondBuilder\"\n  },\n  \"capacity\" : \"MEDIUM\",\n  \"multilevel_partition_cols\" : [ \"TEST_KYLIN_FACT.LSTG_FORMAT_NAME\" ],\n  \"computed_columns\" : [ ]\n}",
        "uuid": "60f4e30e-f50f-4abf-8ad2-4f34233aae21"
    },
    "msg": ""
}
```

### Drop Model 
`Request Mode DELETE`

`Access Path http://host:port/kylin/api/model/{projectName}/{modelName}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* projectName - `required` `string`, project name.
* modelName - `required` `string`, data models' name.

