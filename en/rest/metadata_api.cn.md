## 数据源 REST API
> **提示**
> 
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
> 


   * [返回 Hive 表信息](#get-hive-table)
   * [返回 Hive 表扩展信息](#get-hive-table-extend-info)
   * [返回多个 Hive 表](#get-hive-tables)
   * [加载 Hive 表](#load-hive-tables)

### <span id="get-hive-table">Get Hive Table</span>
`请求方式 GET`

`访问路径 http://host:port/kylin/api/tables/{tableName}`

#### 请求参数
* tableName - `必选` `string` 表名.

#### 响应示例
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

### <span id="get-hive-table-extend-info">返回 Hive 表扩展信息</span>
`请求方式 GET`

`访问路径 http://host:port/kylin/api/tables/{tableName}/exd-map`

#### 请求参数
* tableName - `可选` `string` 表名.

#### 响应示例
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

### <span id="get-hive-tables">返回多个 Hive 表</span>
`请求方式 GET`

`访问路径 http://host:port/kylin/api/tables`

#### 请求参数
* project- `必选` `string` 返回该项目下所有表.
* ext- `可选` `boolean`  要返回扩展信息设置该参数为true.

#### 响应示例
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

### <span id="load-hive-tables">加载 Hive 表</span>
`请求方式 POST`

`访问路径 http://host:port/kylin/api/tables/{tables}/{project}`

#### 请求参数
* tables - `必选` `string` 你想要加载的hive表, 用逗号分隔
* project - `必选` `String`  指定hive表将要加载到哪个项目

#### 响应示例
```
{
    "result.loaded": ["DEFAULT.SAMPLE_07"],
    "result.unloaded": ["sapmle_08"]
}
```