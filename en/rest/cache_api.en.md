## Cache REST API

> **Tip**
>
> Before using API, please make sure that you have read the Access and Authentication in advance and know how to add verification information. 
>


### Purge Cache
`Request Mode PUT`

`Access Path http://host:port/kylin/api/cache/{type}/{name}/{action}`

#### Path Parameter
* type - `required` `string` 'METADATA' or 'CUBE'
* name - `required` `string` cache key, such as Cube name
* action - `required` `string` 'create', 'update' or 'drop'