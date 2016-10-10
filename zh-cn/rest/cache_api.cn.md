## 缓存 REST API

> **提示**
> 
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
> 


### 清理缓存
`请求方式 PUT`

`访问路径 http://host:port/kylin/api/cache/{type}/{name}/{action}`

#### 路径变量
* type - `必选` `string` 'METADATA' 或者 'CUBE'
* name - `必选` `string` 缓存键值, 比如 Cube 名称.
* action - `必选` `string` 'create', 'update' or 'drop'