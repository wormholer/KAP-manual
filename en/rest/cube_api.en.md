## Cube REST API

> **Tip**
>
> Before using API, please make sure that you have read the previous chapter of *Access and Authentication* in advance and know how to add verification information in API. 
>

* [List Cubes](#list-cubes)
* [Get Cube](#get-cube)
* [Get Cube Descriptor](#get-cube-descriptor)
* [Get Data Model](#get-data-model)
* [Build Cube - Date Partition](#build-cube-date-partition)
* [Build Cube - Non Date Partition](#build-cube-non-date-partition)
* [Clone Cube](#clone-cube)
* [Enable Cube](#enable-cube)
* [Disable Cube](#disable-cube)
* [Purge Cube](#purge-cube)
* [Manage Segment](#Manage Segment)

### List Cubes 
`Request Mode GET`

`Access Path http://host:port/kylin/api/cubes`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Request Body
* offset - `optional` `int`, default 0, get data start subscript.
* limit - `optional` `int `, default 10, how many lines would be included in each returned page.
* cubeName - `optional` `string`, returned name is the keyword related Cube.
* exactMatch - `optional` `boolean`, default true, specify whether matching exactly based on cubeName.
* modelName - `optional` `string` returned model name is the keyword's Cube.
* projectName - `optional` `string`, specify the returned Cube under the project.

#### Response Example
```sh
[  
      {
        "uuid": "8372c3b7-a33e-4b69-83dd-0bb8b1f8117e",
        "last_modified": 1508487909245,
        "version": "2.3.0.20500",
        "name": "ci_inner_join_cube",
        "owner": null,
        "descriptor": "ci_inner_join_cube",
        "cost": 50,
        "status": "DISABLED",
        "segments": [],
        "create_time_utc": 0,
        "cuboid_bytes": null,
        "cuboid_bytes_recommend": null,
        "cuboid_last_optimized": 0,
        "project": "default",
        "model": "ci_inner_join_model",
        "is_streaming": false,
        "partitionDateColumn": "TEST_KYLIN_FACT.CAL_DT",
        "partitionDateStart": 0,
        "size_kb": 0,
        "input_records_count": 0,
        "input_records_size": 0,
        "is_draft": false,
        "multilevel_partition_cols": [],
        "total_storage_size_kb": 0
    }
]
```

### Get Cube 
`Request Mode GET`

`Access Path http://host:port/kylin/api/cubes/{cubeName}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path variable
* cubeName - `required` `string`, obtained Cube's name.

### Get Cube Descriptor
dimensions, measures and etc.

`Request Mode GET`

`Access Path http://host:port/kylin/api/cube_desc/{projectName}/{cubeName}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* projectName - `required` `string`, project's name.
* cubeName - `required` `string`, Cube's name.

#### Response Example
```sh
    {
    "code": "000",
    "data": {
        "cube": {
            "uuid": "3819ad72-3929-4dff-b59d-cd89a01238af",
            "last_modified": 1508487309851,
            "version": "2.3.0.20500",
            "name": "ci_inner_join_cube",
            "is_draft": false,
            "model_name": "ci_inner_join_model",
            "description": null,
            "null_string": null,
            "dimensions": [
                {
                    "name": "CAL_DT",
                    "table": "TEST_CAL_DT",
                    "column": "{FK}",
                    "derived": [
                        "WEEK_BEG_DT"
                    ]
                }            
            ],
            "measures": [
                {
                    "name": "TRANS_CNT",
                    "function": {
                        "expression": "COUNT",
                        "parameter": {
                            "type": "constant",
                            "value": "1"
                        },
                        "returntype": "bigint"
                    }
                },
                {
                    "name": "BUYER_CONTACT",
                    "function": {
                        "expression": "EXTENDED_COLUMN",
                        "parameter": {
                            "type": "column",
                            "value": "TEST_ORDER.BUYER_ID",
                            "next_parameter": {
                                "type": "column",
                                "value": "BUYER_ACCOUNT.ACCOUNT_CONTACT"
                            }
                        },
                        "returntype": "extendedcolumn(100)"
                    }
                },
                {
                    "name": "SELLER_CONTACT",
                    "function": {
                        "expression": "EXTENDED_COLUMN",
                        "parameter": {
                            "type": "column",
                            "value": "TEST_KYLIN_FACT.SELLER_ID",
                            "next_parameter": {
                                "type": "column",
                                "value": "SELLER_ACCOUNT.ACCOUNT_CONTACT"
                            }
                        },
                        "returntype": "extendedcolumn(100)"
                    }
                },
                {
                    "name": "TRANS_ID_RAW",
                    "function": {
                        "expression": "RAW",
                        "parameter": {
                            "type": "column",
                            "value": "TEST_KYLIN_FACT.TRANS_ID"
                        },
                        "returntype": "raw"
                    }
                }
            ],
            "dictionaries": [
                {
                    "column": "TEST_KYLIN_FACT.TEST_COUNT_DISTINCT_BITMAP",
                    "builder": "org.apache.kylin.dict.global.SegmentAppendTrieDictBuilder"
                }
            ],
            "rowkey": {
                "rowkey_columns": [
                    {
                        "column": "TEST_KYLIN_FACT.SELLER_ID",
                        "encoding": "int:4",
                        "isShardBy": false
                    },
                    {
                        "column": "TEST_KYLIN_FACT.ORDER_ID",
                        "encoding": "int:4",
                        "isShardBy": false
                    },
                    {
                        "column": "TEST_KYLIN_FACT.CAL_DT",
                        "encoding": "date",
                        "isShardBy": false
                    }                
                 ]
            },
            "hbase_mapping": {
                "column_family": [
                    {
                        "name": "F1",
                        "columns": [
                            {
                                "qualifier": "M",
                                "measure_refs": [
                                    "TRANS_CNT",
                                    "ITEM_COUNT_SUM",
                                    "GMV_SUM",
                                    "GMV_MIN",
                                    "GMV_MAX",
                                    "COMPUTED_COLUMN_MEASURE"
                                ]
                            }
                        ]
                    },
                    {
                        "name": "F2",
                        "columns": [
                            {
                                "qualifier": "M",
                                "measure_refs": [
                                    "SELLER_HLL",
                                    "SELLER_FORMAT_HLL",
                                    "TOP_SELLER",
                                    "TEST_COUNT_DISTINCT_BITMAP"
                                ]
                            }
                        ]
                    },
                    {
                        "name": "F3",
                        "columns": [
                            {
                                "qualifier": "M",
                                "measure_refs": [
                                    "TEST_EXTENDED_COLUMN",
                                    "BUYER_CONTACT",
                                    "SELLER_CONTACT",
                                    "TRANS_ID_RAW",
                                    "PRICE_RAW",
                                    "CAL_DT_RAW"
                                ]
                            }
                        ]
                    }
                ]
            },
            "aggregation_groups": [
                {
                    "includes": [
                        "TEST_KYLIN_FACT.CAL_DT",
                        "TEST_KYLIN_FACT.LEAF_CATEG_ID",
                        "TEST_KYLIN_FACT.LSTG_FORMAT_NAME",
                        "TEST_KYLIN_FACT.LSTG_SITE_ID",
                        "TEST_KYLIN_FACT.SLR_SEGMENT_CD",
                        "TEST_CATEGORY_GROUPINGS.META_CATEG_NAME",
                        "TEST_CATEGORY_GROUPINGS.CATEG_LVL2_NAME",
                        "TEST_CATEGORY_GROUPINGS.CATEG_LVL3_NAME",
                        "TEST_KYLIN_FACT.DEAL_YEAR"
                    ],
                    "select_rule": {
                        "hierarchy_dims": [
                            [
                                "TEST_CATEGORY_GROUPINGS.META_CATEG_NAME",
                                "TEST_CATEGORY_GROUPINGS.CATEG_LVL2_NAME",
                                "TEST_CATEGORY_GROUPINGS.CATEG_LVL3_NAME",
                                "TEST_KYLIN_FACT.LEAF_CATEG_ID"
                            ]
                        ],
                        "mandatory_dims": [],
                        "joint_dims": [
                            [
                                "TEST_KYLIN_FACT.LSTG_FORMAT_NAME",
                                "TEST_KYLIN_FACT.LSTG_SITE_ID",
                                "TEST_KYLIN_FACT.SLR_SEGMENT_CD",
                                "TEST_KYLIN_FACT.DEAL_YEAR"
                            ]
                        ],
                        "dim_cap": 1
                    }
                },
                {
                    "includes": [
                        "TEST_KYLIN_FACT.CAL_DT",
                        "TEST_KYLIN_FACT.LEAF_CATEG_ID",
                        "TEST_KYLIN_FACT.LSTG_FORMAT_NAME",
                        "TEST_KYLIN_FACT.LSTG_SITE_ID",
                        "TEST_KYLIN_FACT.SLR_SEGMENT_CD",
                        "TEST_CATEGORY_GROUPINGS.META_CATEG_NAME",
                        "TEST_CATEGORY_GROUPINGS.CATEG_LVL2_NAME",
                        "TEST_CATEGORY_GROUPINGS.CATEG_LVL3_NAME",
                        "TEST_KYLIN_FACT.SELLER_ID",
                        "SELLER_ACCOUNT.ACCOUNT_BUYER_LEVEL",
                        "SELLER_ACCOUNT.ACCOUNT_SELLER_LEVEL",
                        "SELLER_ACCOUNT.ACCOUNT_COUNTRY",
                        "SELLER_COUNTRY.NAME",
                        "TEST_KYLIN_FACT.ORDER_ID",
                        "TEST_ORDER.TEST_DATE_ENC",
                        "TEST_ORDER.TEST_TIME_ENC",
                        "TEST_ORDER.BUYER_ID",
                        "BUYER_ACCOUNT.ACCOUNT_BUYER_LEVEL",
                        "BUYER_ACCOUNT.ACCOUNT_SELLER_LEVEL",
                        "BUYER_ACCOUNT.ACCOUNT_COUNTRY",
                        "BUYER_COUNTRY.NAME",
                        "TEST_KYLIN_FACT.SELLER_COUNTRY_ABBR",
                        "TEST_KYLIN_FACT.BUYER_COUNTRY_ABBR",
                        "TEST_KYLIN_FACT.SELLER_ID_AND_COUNTRY_NAME",
                        "TEST_KYLIN_FACT.BUYER_ID_AND_COUNTRY_NAME"
                    ],
                    "select_rule": {
                        "hierarchy_dims": [],
                        "mandatory_dims": [
                            "TEST_KYLIN_FACT.CAL_DT"
                        ],
                        "joint_dims": [
                            [
                                "TEST_CATEGORY_GROUPINGS.META_CATEG_NAME",
                                "TEST_CATEGORY_GROUPINGS.CATEG_LVL2_NAME",
                                "TEST_CATEGORY_GROUPINGS.CATEG_LVL3_NAME",
                                "TEST_KYLIN_FACT.LEAF_CATEG_ID"
                            ]
                        ],
                        "dim_cap": 1
                    }
                }
            ],
            "signature": "kNACGfzr3/ozZvEsWLNNtg==",
            "notify_list": null,
            "status_need_notify": [],
            "partition_date_start": 0,
            "partition_date_end": 3153600000000,
            "auto_merge_time_ranges": null,
            "retention_range": 0,
            "engine_type": 100,
            "storage_type": 99,
            "override_kylin_properties": {
                "kylin.cube.algorithm": "LAYER",
                "kylin.storage.hbase.owner-tag": "kylin@kylin.apache.org"
            },
            "cuboid_black_list": [],
            "parent_forward": 3
        }
    },
    "msg": ""
}
```

### Get data model
fact tables, dimension tables and etc.

`Request Mode GET`

`Access Path http://host:port/kylin/api/model_desc/{projectName}/{modelName}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* projectName - `required` `string`, return models under this project. 
* modelName - `required` `string`, data model's name.

#### Response Example
```sh
    {
    "code": "000",
    "data": {
        "model": {
            "uuid": "72ab4ee2-2cdb-4b07-b39e-4c298563ae27",
            "last_modified": 1507691058000,
            "version": "2.3.0.20500",
            "name": "ci_inner_join_model",
            "owner": null,
            "is_draft": false,
            "description": null,
            "fact_table": "DEFAULT.TEST_KYLIN_FACT",
            "lookups": [
                {
                    "table": "DEFAULT.TEST_ORDER",
                    "kind": "FACT",
                    "alias": "TEST_ORDER",
                    "join": {
                        "type": "INNER",
                        "primary_key": [
                            "TEST_ORDER.ORDER_ID"
                        ],
                        "foreign_key": [
                            "TEST_KYLIN_FACT.ORDER_ID"
                        ]
                    }
                },
            "dimensions": [
                {
                    "table": "TEST_KYLIN_FACT",
                    "columns": [
                        "TRANS_ID",
                        "ORDER_ID",
                        "CAL_DT",
                        "LSTG_FORMAT_NAME",
                        "LSTG_SITE_ID",
                        "LEAF_CATEG_ID",
                        "SLR_SEGMENT_CD",
                        "SELLER_ID",
                        "TEST_COUNT_DISTINCT_BITMAP",
                        "DEAL_YEAR",
                        "SELLER_COUNTRY_ABBR",
                        "BUYER_COUNTRY_ABBR",
                        "SELLER_ID_AND_COUNTRY_NAME",
                        "BUYER_ID_AND_COUNTRY_NAME"
                    ]
                }            
            ],
            "metrics": [
                "TEST_KYLIN_FACT.PRICE",
                "TEST_KYLIN_FACT.ITEM_COUNT",
                "TEST_KYLIN_FACT.DEAL_AMOUNT"
            ],
            "filter_condition": null,
            "partition_desc": {
                "partition_date_column": "TEST_KYLIN_FACT.CAL_DT",
                "partition_time_column": null,
                "partition_date_start": 0,
                "partition_date_format": "yyyy-MM-dd",
                "partition_time_format": "HH:mm:ss",
                "partition_type": "APPEND",
                "partition_condition_builder": "org.apache.kylin.metadata.model.PartitionDesc$DefaultPartitionConditionBuilder"
            },
            "capacity": "MEDIUM",
            "multilevel_partition_cols": [],
            "computed_columns": [
                {
                    "tableIdentity": "DEFAULT.TEST_KYLIN_FACT",
                    "tableAlias": "TEST_KYLIN_FACT",
                    "columnName": "DEAL_AMOUNT",
                    "expression": "TEST_KYLIN_FACT.PRICE * TEST_KYLIN_FACT.ITEM_COUNT",
                    "datatype": "decimal",
                    "comment": "deal amount of inner join model (with legacy expression format)"
                },
                {
                    "tableIdentity": "DEFAULT.TEST_KYLIN_FACT",
                    "tableAlias": "TEST_KYLIN_FACT",
                    "columnName": "DEAL_YEAR",
                    "expression": "year(TEST_KYLIN_FACT.CAL_DT)",
                    "datatype": "integer",
                    "comment": "the year of the deal"
                },
                {
                    "tableIdentity": "DEFAULT.TEST_KYLIN_FACT",
                    "tableAlias": "TEST_KYLIN_FACT",
                    "columnName": "BUYER_ID_AND_COUNTRY_NAME",
                    "expression": "CONCAT(BUYER_ACCOUNT.ACCOUNT_ID, BUYER_COUNTRY.NAME)",
                    "datatype": "string",
                    "comment": "synthetically concat buyer's account id and buyer country"
                },
                {
                    "tableIdentity": "DEFAULT.TEST_KYLIN_FACT",
                    "tableAlias": "TEST_KYLIN_FACT",
                    "columnName": "SELLER_ID_AND_COUNTRY_NAME",
                    "expression": "CONCAT(SELLER_ACCOUNT.ACCOUNT_ID, SELLER_COUNTRY.NAME)",
                    "datatype": "string",
                    "comment": "synthetically concat seller's account id and seller country"
                },
                {
                    "tableIdentity": "DEFAULT.TEST_KYLIN_FACT",
                    "tableAlias": "TEST_KYLIN_FACT",
                    "columnName": "BUYER_COUNTRY_ABBR",
                    "expression": "SUBSTR(BUYER_ACCOUNT.ACCOUNT_COUNTRY,0,1)",
                    "datatype": "string",
                    "comment": "first char of country of buyer account"
                },
                {
                    "tableIdentity": "DEFAULT.TEST_KYLIN_FACT",
                    "tableAlias": "TEST_KYLIN_FACT",
                    "columnName": "SELLER_COUNTRY_ABBR",
                    "expression": "SUBSTR(SELLER_ACCOUNT.ACCOUNT_COUNTRY,0,1)",
                    "datatype": "string",
                    "comment": "first char of country of seller account"
                }
            ],
            "project": "default"
        }
    },
    "msg": ""
}
```

### Build Cube - Date Partition
`Request Mode PUT`

`Access Path http://host:port/kylin/api/cubes/{cubeName}/rebuild`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* cubeName - `required` `string`, Cube's name.

#### Request Mode
* startTime - `required` `long`, the timestamp refers to start time corresponding to the data to be calculated, it should be a GMT0 form timestamp, e.g. 1388563200000 for 2014-1-1.
* endTime - `required` `long`, the timestamp refers to end time corresponding to the data to be calculated, it should be a GMT0 form timestamp.
* buildType - `required` `string`, supported calculation types: 'BUILD'.
* mpValues - `optional` `string`, the value of field "more partition" corresponding to model.

#### Response Example
```
{
    "code": "000",
    "data": {
        "uuid": "3e38d217-0c31-4d9b-9e52-57d10b1e7190",
        "last_modified": 1508837365452,
        "version": "2.3.0.20500",
        "name": "BUILD CUBE - mppp_clone1_4142494e - 20171024172711_20171024172711 - GMT+08:00 2017-10-24 17:29:25",
        "type": "BUILD",
        "duration": 0,
        "related_cube": "mppp_clone1_4142494e",
        "related_segment": "889049e8-5a57-41d8-abcd-3a356d57eea0",
        "exec_start_time": 0,
        "exec_end_time": 0,
        "exec_interrupt_time": 0,
        "mr_waiting": 0,
        "steps": [
            {
                "interruptCmd": null,
                "id": "3e38d217-0c31-4d9b-9e52-57d10b1e7190-00",
                "name": "Create Intermediate Flat Hive Table",
                "sequence_id": 0,
                "exec_cmd": null,
                "interrupt_cmd": null,
                "exec_start_time": 0,
                "exec_end_time": 0,
                "exec_wait_time": 0,
                "step_status": "PENDING",
                "cmd_type": "SHELL_CMD_HADOOP",
                "info": {},
                "run_async": false
            }
        ],
        "submitter": "ADMIN",
        "job_status": "PENDING",
        "progress": 0
    },
    "msg": ""
}
```

### Build Cube - Non Date Partition

`Request Mode PUT`

`Access Path http://host:port/kylin/api/cubes/{cubeName}/build_by_offset`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable

- cubeName - `required` `string` Cube's name

#### Request Body

- sourceOffsetStart - `required` `long` , start value
- sourceOffsetEnd - `required` `long`, end value
- buildType - `required` `string`, supported computing type: 'BUILD'
- mpValues - `optional` `string`, the value of more partition field for the corresponding model

### Clone Cube

`Request Mode PUT`

`Access Path http://host:port/kylin/api/cubes/{cubeName}/clone`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* cubeName - `required` `string`, the name of cloned Cube.

#### Request Body
* cubeName - `required` `string`, new Cube's name.
* project - `required` `string`, new project's name. 


#### Response Example
(same as "Enable Cube")

### Enable Cube
`Request Mode PUT`

`Access Path http://host:port/kylin/api/cubes/{cubeName}/enable`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* cubeName - `required` `string`, Cube 's name.

#### Response Example
```sh
{  
   "uuid":"1eaca32a-a33e-4b69-83dd-0bb8b1f8c53b",
   "last_modified":1407909046305,
   "name":"test_kylin_cube_with_slr_ready",
   "owner":null,
   "version":null,
   "descriptor":"test_kylin_cube_with_slr_desc",
   "cost":50,
   "status":"ACTIVE",
   "segments":[  
      {  
         "name":"19700101000000_20140531160000",
         "storage_location_identifier":"KYLIN-CUBE-TEST_KYLIN_CUBE_WITH_SLR_READY-19700101000000_20140531160000_BF043D2D-9A4A-45E9-AA59-5A17D3F34A50",
         "date_range_start":0,
         "date_range_end":1401552000000,
         "status":"READY",
         "size_kb":4758,
         "source_records":6000,
         "source_records_size":620356,
         "last_build_time":1407832663227,
         "last_build_job_id":"2c7a2b63-b052-4a51-8b09-0c24b5792cda",
         "binary_signature":null,
         "dictionaries":{  
            "TEST_CATEGORY_GROUPINGS/CATEG_LVL2_NAME":"/dict/TEST_CATEGORY_GROUPINGS/CATEG_LVL2_NAME/16d8185c-ee6b-4f8c-a919-756d9809f937.dict",
            "TEST_KYLIN_FACT/LSTG_SITE_ID":"/dict/TEST_SITES/SITE_ID/0bec6bb3-1b0d-469c-8289-b8c4ca5d5001.dict",
            "TEST_KYLIN_FACT/SLR_SEGMENT_CD":"/dict/TEST_SELLER_TYPE_DIM/SELLER_TYPE_CD/0c5d77ec-316b-47e0-ba9a-0616be890ad6.dict",
            "TEST_KYLIN_FACT/CAL_DT":"/dict/PREDEFINED/date(yyyy-mm-dd)/64ac4f82-f2af-476e-85b9-f0805001014e.dict",
            "TEST_CATEGORY_GROUPINGS/CATEG_LVL3_NAME":"/dict/TEST_CATEGORY_GROUPINGS/CATEG_LVL3_NAME/270fbfb0-281c-4602-8413-2970a7439c47.dict",
            "TEST_KYLIN_FACT/LEAF_CATEG_ID":"/dict/TEST_CATEGORY_GROUPINGS/LEAF_CATEG_ID/2602386c-debb-4968-8d2f-b52b8215e385.dict",
            "TEST_CATEGORY_GROUPINGS/META_CATEG_NAME":"/dict/TEST_CATEGORY_GROUPINGS/META_CATEG_NAME/0410d2c4-4686-40bc-ba14-170042a2de94.dict"
         },
         "snapshots":{  
            "TEST_CAL_DT":"/table_snapshot/TEST_CAL_DT.csv/8f7cfc8a-020d-4019-b419-3c6deb0ffaa0.snapshot",
            "TEST_SELLER_TYPE_DIM":"/table_snapshot/TEST_SELLER_TYPE_DIM.csv/c60fd05e-ac94-4016-9255-96521b273b81.snapshot",
            "TEST_CATEGORY_GROUPINGS":"/table_snapshot/TEST_CATEGORY_GROUPINGS.csv/363f4a59-b725-4459-826d-3188bde6a971.snapshot",
            "TEST_SITES":"/table_snapshot/TEST_SITES.csv/78e0aecc-3ec6-4406-b86e-bac4b10ea63b.snapshot"
         }
      }
   ],
   "create_time":null,
   "source_records_count":6000,
   "source_records_size":0,
   "size_kb":4758
}
```

### Disable Cube
`Request Mode PUT`

`Access Path http://host:port/kylin/api/cubes/{cubeName}/disable`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* cubeName - `required` `string`, Cube's name.

#### Response Example
(same as "Enable Cube")

### Purge Cube
`Request Mode PUT`

`Access Path http://host:port/kylin/api/cubes/{cubeName}/purge`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* cubeName - `required` `string`, Cube's name.
* mpValues - `optional` `string`, the value of Model Primary Partition.

#### Response Example
(same as "Enable Cube")

### Manage Segment

`Request Mode PUT`

`Access Path http://host:port/kylin/api/cubes/<cubeName>/segments`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable

- cubeName - `required` `string`, Cube's name. 

#### Request Body

- buildType - `required` `string`, MERGE, REFRESH, DROP.
- force - `required` `string` , only true.
- segments - `required` `string`, segment name array.
- mpValues - `optional` `string`, Model Primary Partition value.

#### Curl Example

```
curl -u ADMIN:KYLIN -H "Accept: application/vnd.apache.kylin-v2+json" -H "Content-Type: application/json;charset=utf-8" -X PUT -d '{ "buildType": "DROP", "mpValues": "ABIN", "segments": ["0_1000"] }' "http://localhost:8080/kylin/api/cubes/mptest/segments"
```