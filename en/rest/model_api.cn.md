## 模型 REST API

> **提示**
> 
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
> 


   * [返回多个模型](#list-models)
   * [返回指定模型](#get-model)
   * [克隆模型](#clone-model)
   * [删除模型](#drop-model)

### <span id="list-models">返回多个模型</span>
`请求方式 GET`

`访问路径 http://host:port/kylin/api/models`

#### 请求主体
* offset - `必选` `int` 返回数据起始下标
* limit - `必选` `int ` 分页返回对应每页返回多少
* modelName - `可选` `string` 返回名称等于该关键字的Model
* projectName - `可选` `string` 指定返回该项目下

#### 响应示例
```sh
［
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
  ］
```

### <span id="get-model">返回指定模型</span>
`请求方式 GET`

`访问路径 http://host:port/kylin/api/model/{modelName}`

#### 路径变量
* modelName - `必选` `string` 要获取的model 名称.

### <span id="clone-model">克隆模型</span>
`请求方式 PUT`

`访问路径 http://host:port/kylin/api/models/{modelName}/clone`

#### 路径变量
* modelName - `必选` `string` 被克隆模型名称.

#### 请求主体
* modelName - `必选` `string` 新模型名称.
* project - `必选` `string` 新项目名称 


#### 响应示例
```sh
  {
    message:null,
    modelDescData:null,
    modelName:"kylin_sales_model_clone",
    project:"learn_kylin",
    successful:true,
    uuid:"c9d164f9-3e1a-425f-b41e-591223e6e91a"
}
```

### <span id="drop-model">删除数据模型</span>
`请求方式 DELETE`

`访问路径 http://host:port/kylin/api/model/{modelName}`

#### 路径变量
* modelName - `必选` `string` 数据模型名称.
