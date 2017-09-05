## Model REST API

> **Tip**
>
> Before using API, please make sure that you have read the Access and Authentication in advance and know how to add verification information. 
>


* [List Models](#list-models)
* [Get the Model](#get-model)
* [Clone Model](#clone-model)
* [Drop Model](#drop-model)

### List Models
`Request Mode GET`

`Access Path http://host:port/kylin/api/models`

#### Request Body
* offset - `optional` `int` get data start subscript
* limit - `required` `int ` how many lines would be included in each returned page
* modelName - `optional` `string` returned name is the keyword related model
* projectName - `optional` `string` specify the returned project

#### Response Example
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

### Get Model
`Request Mode GET`

`Access Path http://host:port/kylin/api/model/{modelName}`

#### Path Variable
* modelName - `required` `string` Obtained models' name.

### Clone Model
`Request Mode PUT`

`Access Path http://host:port/kylin/api/models/{modelName}/clone`

#### Path Variable
* modelName - `required` `string` The name of the cloned model.

#### Request Body
* modelName - `required` `string` New models' name.
* project - `required` `string` New projects' name. 


#### Response Example
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

### Drop Model 
`Request Mode DELETE`

`Access Path http://host:port/kylin/api/model/{modelName}`

#### Path Variable
* modelName - `required` `string` Data models' name.
