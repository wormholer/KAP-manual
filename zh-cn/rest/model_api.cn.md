## 模型 REST API

> **提示**
>
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
>


* [返回模型](#返回模型)
* [克隆模型](#克隆模型)
* [删除模型](#删除数据模型)

### 返回模型
`请求方式 GET`

`访问路径 http://host:port/kylin/api/models`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### 请求主体
* offset - `可选` `int` 默认0 返回数据起始下标
* limit - `可选` `int ` 默认10 分页返回对应每页返回多少
* modelName - `可选` `string` 返回名称等于该关键字的Model
* exactMatch - `可选` `boolean` 默认true 是否对modelName完全匹配
* projectName - `可选` `string` 指定返回该项目下

#### 响应示例
```json
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

### 克隆模型
`请求方式 PUT`

`访问路径 http://host:port/kylin/api/models/{modelName}/clone`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### 路径变量
* modelName - `必选` `string` 被克隆模型名称.

#### 请求主体
* modelName - `必选` `string` 新模型名称.
* project - `必选` `string` 新项目名称 


#### 响应示例
```json
 {
    "code": "000",
    "data": {
        "modelDescData": "{\n  \"uuid\" : \"60f4e30e-f50f-4abf-8ad2-4f34233aae21\",\n  \"last_modified\" : 1509010496630,\n  \"version\" : \"2.3.0.20500\",\n  \"name\" : \"m2\",\n  \"owner\" : \"ADMIN\",\n  \"is_draft\" : false,\n  \"description\" : \"\",\n  \"fact_table\" : \"DEFAULT.TEST_KYLIN_FACT\",\n  \"lookups\" : [ ],\n  \"dimensions\" : [ {\n    \"table\" : \"TEST_KYLIN_FACT\",\n    \"columns\" : [ \"TRANS_ID\", \"ORDER_ID\", \"CAL_DT\", \"LSTG_FORMAT_NAME\", \"LEAF_CATEG_ID\", \"LSTG_SITE_ID\", \"SLR_SEGMENT_CD\", \"SELLER_ID\", \"TEST_COUNT_DISTINCT_BITMAP\" ]\n  } ],\n  \"metrics\" : [ \"TEST_KYLIN_FACT.PRICE\", \"TEST_KYLIN_FACT.ITEM_COUNT\" ],\n  \"filter_condition\" : \"\",\n  \"partition_desc\" : {\n    \"partition_date_column\" : \"TEST_KYLIN_FACT.CAL_DT\",\n    \"partition_time_column\" : null,\n    \"partition_date_start\" : 0,\n    \"partition_date_format\" : \"yyyy-MM-dd\",\n    \"partition_time_format\" : \"\",\n    \"partition_type\" : \"APPEND\",\n    \"partition_condition_builder\" : \"io.kyligence.kap.cube.mp.MPSqlCondBuilder\"\n  },\n  \"capacity\" : \"MEDIUM\",\n  \"multilevel_partition_cols\" : [ \"TEST_KYLIN_FACT.LSTG_FORMAT_NAME\" ],\n  \"computed_columns\" : [ ]\n}",
        "uuid": "60f4e30e-f50f-4abf-8ad2-4f34233aae21"
    },
    "msg": ""
}
```

### 删除数据模型
`请求方式 DELETE`

`访问路径 http://host:port/kylin/api/model/{projectName}/{modelName}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### 路径变量
* projectName - `必选` `string` 项目名称.
* modelName - `必选` `string` 数据模型名称.
