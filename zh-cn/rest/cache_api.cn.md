## 缓存 REST API

> **提示**
>
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
>

### 清理集群缓存
`请求方式 PUT`

`访问路径 http://host:port/kylin/api/cache/announce/{entity}/{cacheKey}/{event}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### 路径变量
* entity - `必选` `string` 'METADATA' 或者 'CUBE'
* cacheKey - `必选` `string` 缓存键值, 比如 Cube 名称.
* event - `必选` `string` 'create', 'update' or 'drop'

### 清理单节点缓存
`请求方式 PUT`

`访问路径 http://host:port/kylin/api/cache/{entity}/{cacheKey}/{event}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### 路径变量
* entity - `必选` `string` 'METADATA' 或者 'CUBE'
* cacheKey - `必选` `string` 缓存键值, 比如 Cube 名称.
* event - `必选` `string` 'create', 'update' or 'drop'