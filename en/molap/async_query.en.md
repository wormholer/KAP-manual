Async query helps user to execute queries asynchronously, providing a more efficient way for result exporting.

## Issuing async query

**POST /kylin/api/async_query**

#### Request Body

- sql - `required` `string` The text of sql statement.
- offset - `optional` `int` Query offset. If offset is set in sql, offset will be ignored.
- limit - `optional` `int` Query limit. If limit is set in sql, limit will be ignored.
- project - `optional` `string` Project to perform query. Default value is ‘DEFAULT’.

#### Request Sample

```json
{  
   "sql":"select * from p_lineorder",
   "offset":0,
   "limit":50000,
   "acceptPartial":false,
   "project":"ssb"
}
```

#### Response Body

- code - return code for this request, "000" means everything is well handled.
- data - the main response body for the reponse. including three fields:
  - queryID - the query id for to process the query 
  - status - the current status of the query, for query issuing the status will always be "RUNNING"
  - info - additional description message for the status
- msg - ignore this

#### Response Sample

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

#### Curl Example

```
curl -X POST -H "Authorization: Basic QURNSU46S1lMSU4=" -H "Content-Type: application/json" -d '{ "sql":"select * from p_lineorder limit 100", "project":"ssb" }' http://master:7070/kylin/api/async_query
```

## Inquiring async query status

**GET /kylin/api/async_query/{QUERY_ID}/status**

#### Request Path Variable

- Query ID - `required` the query ID returned when issuing query

#### Response Body

- code - return code for this request, "000" means everything is well handled.
- data - the main response body for the reponse. including three fields:
  - queryID - the query id for to process the query 
  - status - the current status of the query, there're four possible status:
    - RUNNING - the query is still running
    - SUCCESSFUL - the query has succeeded
    - FAILED - the query has failed
    - MISSING - the query execution info is cleaned
  - info - additional description message for the status
- msg - ignore this

#### Response Sample (success)

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

#### Response Sample (failure)

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

#### Curl Example

```
curl -X GET -H "Authorization: Basic QURNSU46S1lMSU4=" -H "Content-Type: application/json"  http://master:7070/kylin/api/async_query/43eeb967-e76a-4214-8932-70594d454013/status 
```



## retrieve async query result

**GET /kylin/api/async_query/{QUERY_ID}/result**
Invoke this after you confirm query status being SUCCESSFUL

#### Request Path Variable

- Query ID - `required` the query ID returned when issuing query

#### Response Body

- the API returns a file named result.csv directly

#### Curl Example

```
curl -X GET -H "Authorization: Basic QURNSU46S1lMSU4=" -H "Content-Type: application/json"  http://master:7070/kylin/api/async_query/43eeb967-e76a-4214-8932-70594d454012/result 
```



## Clean up async queries results

**DELETE /kylin/api/async_query**
this API will also clean queries with un-fetched results

#### Response Sample

```json
{  
   "code":"000",
   "data": true,
   "msg":""
}
```

#### Curl Example

```
curl -X DELETE -H "Authorization: Basic QURNSU46S1lMSU4=" -H "Content-Type: application/json"  http://master:7070/kylin/api/async_query
```

