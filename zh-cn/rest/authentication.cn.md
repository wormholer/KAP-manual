## 访问与安全认证

### 访问
KAP API的访问前缀为 ｀/kylin/api｀, 不管对哪个模块的API访问都需要加上该前缀，比如访问所有的cube的API为 “/kylin/api/cubes”，对应完整的路径为 http://host:port/kylin/api/cubes


### 认证
KAP所有的API都是基于[basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication)认证机制，Basic Authentication是非常简单的一种访问控制机制，将帐号密码基于Base64编码后作为请求头添加到HTTP 请求头中, 后端会读取请求头中的帐号密码信息进行认证。以KAP默认的账户密码ADMIN/KYLIN为例，对应帐号密码编码后结果为“Basic QURNSU46S1lMSU4=“，那么HTTP对应的头信息为“Authorization: Basic QURNSU46S1lMSU4=”。

### 认证要点
* 在HTTP头添加`Authorization`信息
* 或者可以通过`POST http://localhost:7070/kylin/api/user/authentication` 进行认证，一旦认证通过，接下来对API请求基于cookies在HTTP头中免去`Authorization`信息
   
```
POST http://localhost:7070/kylin/api/user/authentication 
Authorization:Basic xxxxJD124xxxGFxxxSDF
Content-Type: application/json;charset=UTF-8
```

这里以javascript和curl为例介绍在访问API时如何添加认证信息。
## Query API示例
```
$.ajaxSetup({
      headers: { 'Authorization': "Basic eWFu**********X***ZA==", 'Content-Type': 'application/json;charset=utf-8' } // use your own authorization code here
    });
    var request = $.ajax({
       url: "http://hostname/kylin/api/query",
       type: "POST",
       data: '{"sql":"select count(*) from SUMMARY;","offset":0,"limit":50000,"acceptPartial":true,"project":"test"}',
       dataType: "json"
    });
    request.done(function( msg ) {
       alert(msg);
    }); 
    request.fail(function( jqXHR, textStatus ) {
       alert( "Request failed: " + textStatus );
  });

```

Javascript如何生成authorization信息(下载"jquery.base64.js" [https://github.com/yckart/jquery.base64.js](https://github.com/yckart/jquery.base64.js)).

```
var authorizationCode = $.base64('encode', 'NT_USERNAME' + ":" + 'NT_PASSWORD');
 
$.ajaxSetup({
   headers: { 
    'Authorization': "Basic " + authorizationCode, 
    'Content-Type': 'application/json;charset=utf-8' 
   }
});
```


## Curl 示例

```
curl -c /path/to/cookiefile.txt -X POST -H "Authorization: Basic XXXXXXXXX" -H 'Content-Type: application/json' http://<host>:<port>/kylin/api/user/authentication
```

访问成功后JSESSIONID会存在cookie文件中， 在接下来的访问中添加上cookie信息:

```
curl -b /path/to/cookiefile.txt -X PUT -H 'Content-Type: application/json' -d '{"startTime":'1423526400000', "endTime":'1423526400', "buildType":"BUILD"}' http://<host>:<port>/kylin/api/cubes/your_cube/rebuild
```

