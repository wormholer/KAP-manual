## Metadata REST API
> **Tip**
>
> Before using API, please make sure that you have read the Access and Authentication in advance and know how to add verification information. 
>


* [Get hive table information](#get-hive-table)
* [Get hive table with extended information](#get-hive-table-extend-info)
* [Get multiple hive tables](#get-hive-tables)
* [Load hive tables](#load-hive-tables)

### <span id="get-hive-table">Get Hive Table</span>
`Request Mode GET`

`Access Path http://host:port/kylin/api/tables/{tableName}`

#### Request Parameter
* tableName - `required` `string` table name

#### Response Example
```sh
{
    uuid: "69cc92c0-fc42-4bb9-893f-bd1141c91dbe",
    name: "SAMPLE_07",
    columns: [{
        id: "1",
        name: "CODE",
        datatype: "string"
    }, {
        id: "2",
        name: "DESCRIPTION",
        datatype: "string"
    }, {
        id: "3",
        name: "TOTAL_EMP",
        datatype: "int"
    }, {
        id: "4",
        name: "SALARY",
        datatype: "int"
    }],
    database: "DEFAULT",
    last_modified: 1419330476755
}
```

### <span id="get-hive-table-extend-info">Get Hive Table with Extended Information</span>
`Request Mode GET`

`Access Path http://host:port/kylin/api/tables/{tableName}/exd-map`

#### Request Parameter
* tableName - `optional` `string` table name

#### Response Example
```
{
    "minFileSize": "46055",
    "totalNumberFiles": "1",
    "location": "hdfs://sandbox.hortonworks.com:8020/apps/hive/warehouse/sample_07",
    "lastAccessTime": "1418374103365",
    "lastUpdateTime": "1398176493340",
    "columns": "struct columns { string code, string description, i32 total_emp, i32 salary}",
    "partitionColumns": "",
    "EXD_STATUS": "true",
    "maxFileSize": "46055",
    "inputformat": "org.apache.hadoop.mapred.TextInputFormat",
    "partitioned": "false",
    "tableName": "sample_07",
    "owner": "hue",
    "totalFileSize": "46055",
    "outputformat": "org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat"
}
```

### <span id="get-hive-tables">Get Multiple Hive Tables</span>
`Request Mode GET`

`Access Path http://host:port/kylin/api/tables`

#### Request Parameter
* project- `required` `string` return to all tables included in the project
* ext- `required` `boolean`  if users want to get extending information, then the parameter should be set as true

#### Response Example
```sh
[
 {
    uuid: "53856c96-fe4d-459e-a9dc-c339b1bc3310",
    name: "SAMPLE_08",
    columns: [{
        id: "1",
        name: "CODE",
        datatype: "string"
    }, {
        id: "2",
        name: "DESCRIPTION",
        datatype: "string"
    }, {
        id: "3",
        name: "TOTAL_EMP",
        datatype: "int"
    }, {
        id: "4",
        name: "SALARY",
        datatype: "int"
    }],
    database: "DEFAULT",
    cardinality: {},
    last_modified: 0,
    exd: {
        minFileSize: "46069",
        totalNumberFiles: "1",
        location: "hdfs://sandbox.hortonworks.com:8020/apps/hive/warehouse/sample_08",
        lastAccessTime: "1398176495945",
        lastUpdateTime: "1398176495981",
        columns: "struct columns { string code, string description, i32 total_emp, i32 salary}",
        partitionColumns: "",
        EXD_STATUS: "true",
        maxFileSize: "46069",
        inputformat: "org.apache.hadoop.mapred.TextInputFormat",
        partitioned: "false",
        tableName: "sample_08",
        owner: "hue",
        totalFileSize: "46069",
        outputformat: "org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat"
    }
  }
]
```

### <span id="load-hive-tables">Load Hive Tables</span>
`Request Mode POST`

`Access Path http://host:port/kylin/api/tables/{tables}/{project}`

#### Request Parameter
* tables - `required` `string` tables to be loaded, please use "," to part them
* project - `required` `String`  specifies to which project the hive table will be loaded

#### Response Example
```
{
    "result.loaded": ["DEFAULT.SAMPLE_07"],
    "result.unloaded": ["sapmle_08"]
}
```