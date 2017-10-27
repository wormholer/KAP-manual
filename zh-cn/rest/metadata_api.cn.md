## 元数据 REST API
> **提示**
>
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
>


* [返回多个 Hive 表](#返回多个hive表)
* [返回 Hive 表信息](#返回hive表信息)
* [加载 Hive 表](#加载hive表)

### 返回多个hive表
`请求方式 GET`

`访问路径 http://host:port/kylin/api/tables`

`Content-Type: application/vnd.apache.kylin-v2+json`
#### 请求参数
* project - `必选` `string` 项目名.
* ext - `可选` `string` 是否返回表的扩展信息.

#### 响应示例
```json
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

### 返回hive表信息
`请求方式 GET`

`访问路径 http://host:port/kylin/api/tables/{project}/{tableName}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### 请求参数
* project - `可选` `string` 项目名.
* tableName - `可选` `string` 表名.

#### 响应示例
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

### 加载Hive表
`请求方式 POST`

`访问路径 http://host:port/kylin/api/tables/load`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### 请求参数
* project - `必选` `String`  指定hive表将要加载到哪个项目
* tables - `必选` `string` 你想要加载的hive表, 用逗号分隔

#### 响应示例
```json
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