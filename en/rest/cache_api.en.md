## Cache REST API

> **Tip**
>
> Before using API, please make sure that you have read the Access and Authentication in advance and know how to add verification information. 
>


### Purge Cluster Cache
`Request Mode PUT`

`Access Path http://host:port/kylin/api/cache/announce/{entity}/{cacheKey}/{event}`

`Content-Type: application/vnd.apache.kylin-v2+json`


### Path Variable
- entity - `required` `string` 'METADATA' or 'CUBE'.
- cacheKey - `required` `string` cache key, such as Cube name.
- event - `required` `string` 'create', 'update' or 'drop'.


### Purge Single Node Cache

`Request Mode PUT`

`Access Path http://host:port/kylin/api/cache/{entity}/{cacheKey}/{event}`

`Content-Type: application/vnd.apache.kylin-v2+json`

### Path Variable

- entity - `required` `string`, 'METADATA' or 'CUBE'.
- cacheKey - `required` `string`, cache key, such as Cube name.
- event - `required` `string`, 'create', 'update' or 'drop'.