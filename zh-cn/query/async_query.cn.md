## 异步查询

异步查询可以帮助用户异步地执行SQL查询，提供更高效的数据导出方式。

### 发起异步查询

**POST /kylin/api/async_query**

#### 请求主体

- sql - ```必须``` ```string``` sql查询语句
- offset - ```可选``` ```int```  查询的偏移，如果在查询语句中已经有偏移，该值会被忽略
- limit - ```可选``` ```int``` 查询限制，如果在查询语句中已经有限制，该值会被忽略
- project - ```可选``` ```string``` 选中的项目，默认为"DEFAULT"

#### 请求示例

```json
{  
   "sql":"select * from p_lineorder",
   "offset":0,
   "limit":50000,
   "acceptPartial":false,
   "project":"ssb"
}
```

#### 返回主体

- code - 请求的返回码，"000"意味着没有异常发生
- data - 返回的主体，包含三个部分：
  - queryID - 查询的ID
  - status - 查询的当前状态，对于新发起的查询，该值总是"RUNNING"
  - info - 对状态的额外描述
- msg - 可忽略

#### 返回示例

```json
{  
   "code":"000",
   "data":{  
      "queryID":"43eeb967-e76a-4214-8932-70594d454013",
      "status":"RUNNING",
      "info":"still running"
   },
   "msg":""
}
```

#### Curl实例

```
curl -X POST -H "Authorization: Basic QURNSU46S1lMSU4=" -H "Content-Type: application/json" -d '{ "sql":"select * from p_lineorder limit 100", "project":"ssb" }' http://master:7070/kylin/api/async_query
```

### 查询异步查询状态

**GET /kylin/api/async_query/{QUERY_ID}/status**

#### 请求路径参数

- Query ID - `必须` 发起查询时收到的查询ID

#### 返回主体

- code - 请求的返回码，"000"意味着没有异常发生
- data - 返回的主体，包含三个部分：
  - queryID - 查询的ID
  - status -查询的当前状态，有四种可能的值：
    - RUNNING - 查询仍在进行中
    - SUCCESSFUL - 查询已经成功
    - FAILED - 查询已经失败
    - MISSING - 查询相关信息已经被清除
  - info - 对状态的额外描述
- msg - 可忽略

#### 返回示例 (成功)

```json
{  
   "code":"000",
   "data":{  
      "queryID":"43eeb967-e76a-4214-8932-70594d454013",
      "status":"SUCCESSFUL",
      "info":"await fetching results"
   },
   "msg":""
}
```

#### 返回示例 (失败)

```json
{  
   "code":"000",
   "data":{  
      "queryID":"43eeb967-e76a-4214-8932-70594d454012",
      "status":"FAILED",
      "info":"Project 'XXX' does not exist;"
   },
   "msg":""
}
```

#### Curl示例

```
curl -X GET -H "Authorization: Basic QURNSU46S1lMSU4=" -H "Content-Type: application/json"  http://master:7070/kylin/api/async_query/43eeb967-e76a-4214-8932-70594d454013/status 
```



### 获取异步查询结果

**GET /kylin/api/async_query/{QUERY_ID}/result**

请确认查询状态为SUCCESSFUL之后再调用此接口

#### 请求路径参数

- Query ID - `必须` 发起查询时收到的查询ID

#### 返回主体

- 此接口直接返回一个名为result.csv的文件

#### Curl示例

```
curl -X GET -H "Authorization: Basic QURNSU46S1lMSU4=" -H "Content-Type: application/json"  http://master:7070/kylin/api/async_query/43eeb967-e76a-4214-8932-70594d454012/result 
```



### 清理异步查询结果

**DELETE /kylin/api/async_query**

该接口可能会删除还没有被获取结果的查询

#### 返回示例

```json
{  
   "code":"000",
   "data": true,
   "msg":""
}
```

#### Curl示例

```
curl -X DELETE -H "Authorization: Basic QURNSU46S1lMSU4=" -H "Content-Type: application/json"  http://master:7070/kylin/api/async_query
```

