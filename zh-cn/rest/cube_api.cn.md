## Cube REST API

> **提示**
> 
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
> 


   * [返回多个Cube](#list-cubes)
   * [返回指定cube](#get-cube)
   * [返回Cube描述信息(维度, 度量，等)](#get-cube-descriptor)
   * [返回数据模型 (事实表及维度表等信息)](#get-data-model)
   * [构建 cube](#build-cube)
   * [克隆 cube](#clone-cube)
   * [禁用 cube](#disable-cube)
   * [清理 cube](#purge-cube)
   * [启用 cube](#enable-cube)

### <span id="list-cubes">返回多个Cube</span>
`请求方式 GET`

`访问路径 http://host:port/kylin/api/cubes`

#### 请求主体
* offset - `必选` `int` 返回数据起始下标
* limit - `必选` `int ` 分页返回对应每页返回多少
* cubeName - `可选` `string` 返回名称等于该关键字的Cube
* modelName - `可选` `string` 返回对应模型名称等于该关键字的Cube
* projectName - `可选` `string` 指定返回该项目下Cube

#### 响应示例
```sh
[  
   {  
      "uuid":"1eaca32a-a33e-4b69-83dd-0bb8b1f8c53b",
      "last_modified":1407831634847,
      "name":"test_kylin_cube_with_slr_empty",
      "owner":null,
      "version":null,
      "descriptor":"test_kylin_cube_with_slr_desc",
      "cost":50,
      "status":"DISABLED",
      "segments":[  
      ],
      "create_time":null,
      "source_records_count":0,
      "source_records_size":0,
      "size_kb":0
   }
]
```

### <span id="get-cube">返回指定cube</span>
`请求方式 GET`

`访问路径 http://host:port/kylin/api/cubes/{cubeName}`

#### 路径变量
* cubeName - `必选` `string` 要获取的Cube 名称.

### <span id="get-cube-descriptor">返回Cube描述信息(维度, 度量，等)</span>
`请求方式 GET`

`访问路径 http://host:port/kylin/api/cube_desc/{cubeName}/desc`

#### 路径变量
* cubeName - `必选` `string` Cube 名称.

#### 响应示例
```sh
    {
  "uuid" : "a24ca905-1fc6-4f67-985c-38fa5aeafd92",
 
  "name" : "test_kylin_cube_with_slr_desc",
  "description" : null,
  "dimensions" : [ {
    "name" : "CAL_DT",
    "table" : "EDW.TEST_CAL_DT",
    "column" : "{FK}",
    "derived" : [ "WEEK_BEG_DT" ]
  }, {
    "name" : "CATEGORY",
    "table" : "DEFAULT.TEST_CATEGORY_GROUPINGS",
    "column" : "{FK}",
    "derived" : [ "USER_DEFINED_FIELD1", "USER_DEFINED_FIELD3", "UPD_DATE", "UPD_USER" ]
  }, {
    "name" : "CATEGORY_HIERARCHY",
    "table" : "DEFAULT.TEST_CATEGORY_GROUPINGS",
    "column" : "META_CATEG_NAME",
    "derived" : null
  }, {
    "name" : "CATEGORY_HIERARCHY",
    "table" : "DEFAULT.TEST_CATEGORY_GROUPINGS",
    "column" : "CATEG_LVL2_NAME",
    "derived" : null
  }, {
    "name" : "CATEGORY_HIERARCHY",
    "table" : "DEFAULT.TEST_CATEGORY_GROUPINGS",
    "column" : "CATEG_LVL3_NAME",
    "derived" : null
  }, {
    "name" : "LSTG_FORMAT_NAME",
    "table" : "DEFAULT.TEST_KYLIN_FACT",
    "column" : "LSTG_FORMAT_NAME",
    "derived" : null
  }, {
    "name" : "SITE_ID",
    "table" : "EDW.TEST_SITES",
    "column" : "{FK}",
    "derived" : [ "SITE_NAME", "CRE_USER" ]
  }, {
    "name" : "SELLER_TYPE_CD",
    "table" : "EDW.TEST_SELLER_TYPE_DIM",
    "column" : "{FK}",
    "derived" : [ "SELLER_TYPE_DESC" ]
  }, {
    "name" : "SELLER_ID",
    "table" : "DEFAULT.TEST_KYLIN_FACT",
    "column" : "SELLER_ID",
    "derived" : null
  } ],
  "measures" : [ {
    "name" : "GMV_SUM",
    "function" : {
      "expression" : "SUM",
      "parameter" : {
        "type" : "column",
        "value" : "PRICE",
        "next_parameter" : null
      },
      "returntype" : "decimal(19,4)"
    },
    "dependent_measure_ref" : null
  }, {
    "name" : "GMV_MIN",
    "function" : {
      "expression" : "MIN",
      "parameter" : {
        "type" : "column",
        "value" : "PRICE",
        "next_parameter" : null
      },
      "returntype" : "decimal(19,4)"
    },
    "dependent_measure_ref" : null
  }, {
    "name" : "GMV_MAX",
    "function" : {
      "expression" : "MAX",
      "parameter" : {
        "type" : "column",
        "value" : "PRICE",
        "next_parameter" : null
      },
      "returntype" : "decimal(19,4)"
    },
    "dependent_measure_ref" : null
  }, {
    "name" : "TRANS_CNT",
    "function" : {
      "expression" : "COUNT",
      "parameter" : {
        "type" : "constant",
        "value" : "1",
        "next_parameter" : null
      },
      "returntype" : "bigint"
    },
    "dependent_measure_ref" : null
  }, {
    "name" : "ITEM_COUNT_SUM",
    "function" : {
      "expression" : "SUM",
      "parameter" : {
        "type" : "column",
        "value" : "ITEM_COUNT",
        "next_parameter" : null
      },
      "returntype" : "bigint"
    },
    "dependent_measure_ref" : null
  } ],
  "rowkey" : {
    "rowkey_columns" : [ {
      "column" : "seller_id",
      "encoding" : "int:4",
      "isShardBy" : true
    }, {
      "column" : "cal_dt",
      "encoding" : "dict"
    }, {
      "column" : "leaf_categ_id",
      "encoding" : "fixed_length:18"
    }, {
      "column" : "meta_categ_name",
      "encoding" : "dict"
    }, {
      "column" : "categ_lvl2_name",
      "encoding" : "dict"
    }, {
      "column" : "categ_lvl3_name",
      "encoding" : "dict"
    }, {
      "column" : "lstg_format_name",
      "encoding" : "fixed_length:12"
    }, {
      "column" : "lstg_site_id",
      "encoding" : "dict"
    }, {
      "column" : "slr_segment_cd",
      "encoding" : "dict"
    } ]
  },
  "signature" : null,
  "last_modified" : 1448959801271,
  "model_name" : "test_kylin_inner_join_model_desc",
  "null_string" : null,
  "hbase_mapping" : {
    "column_family" : [ {
      "name" : "f1",
      "columns" : [ {
        "qualifier" : "m",
        "measure_refs" : [ "gmv_sum", "gmv_min", "gmv_max", "trans_cnt", "item_count_sum" ]
      } ]
    } ]
  },
  "aggregation_groups" : [ {
    "includes" : [ "cal_dt", "categ_lvl2_name", "categ_lvl3_name", "leaf_categ_id", "lstg_format_name", "lstg_site_id", "meta_categ_name", "seller_id", "slr_segment_cd" ],
    "select_rule" : {
      "hierarchy_dims" : [ [ "META_CATEG_NAME", "CATEG_LVL2_NAME", "CATEG_LVL3_NAME" ] ],
      "mandatory_dims" : ["seller_id"],
      "joint_dims" : [ [ "lstg_format_name", "lstg_site_id", "slr_segment_cd" ] ]
    }
  } ],
  "notify_list" : null,
  "status_need_notify" : [ ],
  "auto_merge_time_ranges" : null,
  "retention_range" : 0,
  "engine_type" : 2,
  "storage_type" : 2,
  "partition_date_start" : 0
}
```

### <span id="get-data-model">返回数据模型 (事实表及维度表等信息)</span>
`请求方式 GET`

`访问路径 http://host:port/kylin/api/model/{modelName}`

#### 路径变量
* modelName - `必选` `string` 数据模型名称.

#### 响应示例
```sh
{
 
  "uuid" : "ff527b94-f860-44c3-8452-93b17774c647",
  "name" : "test_kylin_inner_join_model_desc",
  "lookups" : [ {
    "table" : "EDW.TEST_CAL_DT",
    "join" : {
      "type" : "inner",
      "primary_key" : [ "CAL_DT" ],
      "foreign_key" : [ "CAL_DT" ]
    }
  }, {
    "table" : "DEFAULT.TEST_CATEGORY_GROUPINGS",
    "join" : {
      "type" : "inner",
      "primary_key" : [ "LEAF_CATEG_ID", "SITE_ID" ],
      "foreign_key" : [ "LEAF_CATEG_ID", "LSTG_SITE_ID" ]
    }
  }, {
    "table" : "EDW.TEST_SITES",
    "join" : {
      "type" : "inner",
      "primary_key" : [ "SITE_ID" ],
      "foreign_key" : [ "LSTG_SITE_ID" ]
    }
  }, {
    "table" : "EDW.TEST_SELLER_TYPE_DIM",
    "join" : {
      "type" : "inner",
      "primary_key" : [ "SELLER_TYPE_CD" ],
      "foreign_key" : [ "SLR_SEGMENT_CD" ]
    }
  } ],
  "dimensions": [
    {
      "table": "default.test_kylin_fact",
      "columns": [
        "lstg_format_name",
        "LSTG_SITE_ID",
        "SLR_SEGMENT_CD",
        "TRANS_ID",
        "CAL_DT",
        "LEAF_CATEG_ID",
        "SELLER_ID"
      ]
    },
    {
      "table": "default.test_category_groupings",
      "columns": [
        "leaf_categ_id",
        "site_id",
        "USER_DEFINED_FIELD1",
        "USER_DEFINED_FIELD3",
        "UPD_DATE",
        "UPD_USER",
        "meta_categ_name",
        "categ_lvl2_name",
        "categ_lvl3_name"
      ]
    },
    {
      "table": "edw.test_sites",
      "columns": [
        "site_id",
        "site_name",
        "cre_user"
      ]
    },
    {
      "table": "edw.test_seller_type_dim",
      "columns": [
        "seller_type_cd",
        "seller_type_desc"
      ]
    },
    {
      "table": "edw.test_cal_dt",
      "columns": [
        "cal_dt",
        "week_beg_dt"
      ]
    }
  ],
  "metrics": [
  "PRICE",
  "ITEM_COUNT",
  "SELLER_ID"
  ],
  "last_modified" : 1422435345352,
  "fact_table" : "DEFAULT.TEST_KYLIN_FACT",
  "filter_condition" : null,
  "partition_desc" : {
    "partition_date_column" : "DEFAULT.TEST_KYLIN_FACT.cal_dt",
    "partition_date_start" : 0,
    "partition_type" : "APPEND"
  }
}
```

### <span id="build-cube">构建 cube</span>
`请求方式 PUT`

`访问路径 http://host:port/kylin/api/cubes/{cubeName}/rebuild`

#### 路径变量
* cubeName - `必选` `string` Cube 名称.

#### 请求主体
* startTime - `必选` `long` 要计算的数据对应起始时间对应的timestamp，应为GMT0格式的
timestamp , e.g. 1388563200000 for 2014-1-1
* endTime - `必选` `long` 要计算的数据对应起始时间对应的timestamp，应为GMT0格式的
timestamp
* buildType - `必选` `string` 支持的计算类型: 'BUILD', 'MERGE', 'REFRESH'

#### 响应示例
```
{  
   "uuid":"c143e0e4-ac5f-434d-acf3-46b0d15e3dc6",
   "last_modified":1407908916705,
   "name":"test_kylin_cube_with_slr_empty - 19700101000000_20140731160000 - BUILD - PDT 2014-08-12 22:48:36",
   "type":"BUILD",
   "duration":0,
   "related_cube":"test_kylin_cube_with_slr_empty",
   "related_segment":"19700101000000_20140731160000",
   "exec_start_time":0,
   "exec_end_time":0,
   "mr_waiting":0,
   "steps":[  
      {  
         "interruptCmd":null,
         "name":"Create Intermediate Flat Hive Table",
         "sequence_id":0,
         "exec_cmd":"hive -e \"DROP TABLE IF EXISTS kylin_intermediate_test_kylin_cube_with_slr_desc_19700101000000_20140731160000_c143e0e4_ac5f_434d_acf3_46b0d15e3dc6;\nCREATE EXTERNAL TABLE IF NOT EXISTS kylin_intermediate_test_kylin_cube_with_slr_desc_19700101000000_20140731160000_c143e0e4_ac5f_434d_acf3_46b0d15e3dc6\n(\nCAL_DT date\n,LEAF_CATEG_ID int\n,LSTG_SITE_ID int\n,META_CATEG_NAME string\n,CATEG_LVL2_NAME string\n,CATEG_LVL3_NAME string\n,LSTG_FORMAT_NAME string\n,SLR_SEGMENT_CD smallint\n,SELLER_ID bigint\n,PRICE decimal\n)\nROW FORMAT DELIMITED FIELDS TERMINATED BY '\\177'\nSTORED AS SEQUENCEFILE\nLOCATION '/tmp/kylin-c143e0e4-ac5f-434d-acf3-46b0d15e3dc6/kylin_intermediate_test_kylin_cube_with_slr_desc_19700101000000_20140731160000_c143e0e4_ac5f_434d_acf3_46b0d15e3dc6';\nSET mapreduce.job.split.metainfo.maxsize=-1;\nSET mapred.compress.map.output=true;\nSET mapred.map.output.compression.codec=com.hadoop.compression.lzo.LzoCodec;\nSET mapred.output.compress=true;\nSET mapred.output.compression.codec=com.hadoop.compression.lzo.LzoCodec;\nSET mapred.output.compression.type=BLOCK;\nSET mapreduce.job.max.split.locations=2000;\nSET hive.exec.compress.output=true;\nSET hive.auto.convert.join.noconditionaltask = true;\nSET hive.auto.convert.join.noconditionaltask.size = 300000000;\nINSERT OVERWRITE TABLE kylin_intermediate_test_kylin_cube_with_slr_desc_19700101000000_20140731160000_c143e0e4_ac5f_434d_acf3_46b0d15e3dc6\nSELECT\nTEST_KYLIN_FACT.CAL_DT\n,TEST_KYLIN_FACT.LEAF_CATEG_ID\n,TEST_KYLIN_FACT.LSTG_SITE_ID\n,TEST_CATEGORY_GROUPINGS.META_CATEG_NAME\n,TEST_CATEGORY_GROUPINGS.CATEG_LVL2_NAME\n,TEST_CATEGORY_GROUPINGS.CATEG_LVL3_NAME\n,TEST_KYLIN_FACT.LSTG_FORMAT_NAME\n,TEST_KYLIN_FACT.SLR_SEGMENT_CD\n,TEST_KYLIN_FACT.SELLER_ID\n,TEST_KYLIN_FACT.PRICE\nFROM TEST_KYLIN_FACT\nINNER JOIN TEST_CAL_DT\nON TEST_KYLIN_FACT.CAL_DT = TEST_CAL_DT.CAL_DT\nINNER JOIN TEST_CATEGORY_GROUPINGS\nON TEST_KYLIN_FACT.LEAF_CATEG_ID = TEST_CATEGORY_GROUPINGS.LEAF_CATEG_ID AND TEST_KYLIN_FACT.LSTG_SITE_ID = TEST_CATEGORY_GROUPINGS.SITE_ID\nINNER JOIN TEST_SITES\nON TEST_KYLIN_FACT.LSTG_SITE_ID = TEST_SITES.SITE_ID\nINNER JOIN TEST_SELLER_TYPE_DIM\nON TEST_KYLIN_FACT.SLR_SEGMENT_CD = TEST_SELLER_TYPE_DIM.SELLER_TYPE_CD\nWHERE (test_kylin_fact.cal_dt < '2014-07-31 16:00:00')\n;\n\"",
         "interrupt_cmd":null,
         "exec_start_time":0,
         "exec_end_time":0,
         "exec_wait_time":0,
         "step_status":"PENDING",
         "cmd_type":"SHELL_CMD_HADOOP",
         "info":null,
         "run_async":false
      },
      {  
         "interruptCmd":null,
         "name":"Extract Fact Table Distinct Columns",
         "sequence_id":1,
         "exec_cmd":" -conf C:/kylin/Kylin/server/src/main/resources/hadoop_job_conf_medium.xml -cubename test_kylin_cube_with_slr_empty -input /tmp/kylin-c143e0e4-ac5f-434d-acf3-46b0d15e3dc6/kylin_intermediate_test_kylin_cube_with_slr_desc_19700101000000_20140731160000_c143e0e4_ac5f_434d_acf3_46b0d15e3dc6 -output /tmp/kylin-c143e0e4-ac5f-434d-acf3-46b0d15e3dc6/test_kylin_cube_with_slr_empty/fact_distinct_columns -jobname Kylin_Fact_Distinct_Columns_test_kylin_cube_with_slr_empty_Step_1",
         "interrupt_cmd":null,
         "exec_start_time":0,
         "exec_end_time":0,
         "exec_wait_time":0,
         "step_status":"PENDING",
         "cmd_type":"JAVA_CMD_HADOOP_FACTDISTINCT",
         "info":null,
         "run_async":true
      },
      {  
         "interruptCmd":null,
         "name":"Load HFile to HBase Table",
         "sequence_id":12,
         "exec_cmd":" -input /tmp/kylin-c143e0e4-ac5f-434d-acf3-46b0d15e3dc6/test_kylin_cube_with_slr_empty/hfile/ -htablename KYLIN-CUBE-TEST_KYLIN_CUBE_WITH_SLR_EMPTY-19700101000000_20140731160000_11BB4326-5975-4358-804C-70D53642E03A -cubename test_kylin_cube_with_slr_empty",
         "interrupt_cmd":null,
         "exec_start_time":0,
         "exec_end_time":0,
         "exec_wait_time":0,
         "step_status":"PENDING",
         "cmd_type":"JAVA_CMD_HADOOP_NO_MR_BULKLOAD",
         "info":null,
         "run_async":false
      }
   ],
   "job_status":"PENDING",
   "progress":0.0
}
```

### <span id="clone-cube">克隆Cube</span>
`请求方式 PUT`

`访问路径 http://host:port/kylin/api/cubes/{cubeName}/clone`

#### 路径变量
* cubeName - `必选` `string` 被克隆Cube名称.

#### 请求主体
* cubeName - `必选` `string` 新Cube名称.
* project - `必选` `string` 新项目名称 


#### 响应示例
(同 "启用 Cube")

### <span id="enable-cube">启用 Cube</span>
`请求方式 PUT`

`访问路径 http://host:port/kylin/api/cubes/{cubeName}/enable`

#### 路径变量
* cubeName - `必选` `string` Cube 名称.

#### 响应示例
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

### <span id="disable-cube">禁用 Cube</span>
`请求方式 PUT`

`访问路径 http://host:port/kylin/api/cubes/{cubeName}/disable`

#### 路径变量
* cubeName - `必选` `string` Cube 名称.

#### 响应示例
(同 "启用 Cube")

### <span id="purge-cube">清理 Cube</span>
`请求方式 PUT`

`访问路径 http://host:port/kylin/api/cubes/{cubeName}/purge`

#### 路径变量
* cubeName - `必选` `string` Cube 名称.

#### 响应示例
(同 "启用 Cube")
