## Apache Zepplin 集成

Apache Zeppelin 是一个开源的数据分析平台，为Apache顶级项目。后端以插件形式支持多种数据处理引擎，如Spark, Flink,Lens等，同时提供了notebook式的UI进行可视化相关的操作。KAP对应开发了自己的Zeppelin模块，现已经合并到Zeppelin主分支中，对应在Zeppelin 0.5.6及后续版本中都可以对接使用KAP,通过Zeppelin访问KAP的数据。

### Zeppelin架构简介
如下图所示，Zeppelin客户端通过HTTP Rest和Websocket两种方式与服务端交互，在server端Zeppelin支持可插拔的Interpreter(解释器)，以Kyin为例，只需要开发Kylin的Interpreter集成进Zeppelin便可以基于Zeppelin客户端与Kylin服务端进行通信，访问Kylin相关数据。

![](/images/integration/zeppelin/zeppelin_arc.png)

### KylinInterpreter工作原理
KylinInterpreter是构建在AP的Rest API之上的，也是一种典型的使用KAP API的场景，KylinInterpreter读取Zeppelin前端针对KAP的配置的 URL，user，password，查询对应的project，limit，以及offset，ispartial，结合前面所讲的查询Rest API，你可能已经明白，这里主要就是拼接查询请求的参数，用户再在前端页面输入SQL，结合配置的参数，就可以通过HTTP POST方式向KAP发送请求，获取数据。
下面是KylinInterpreter的部分代码，结合注释可以明白KylinInterpreter是如何访问KAP API的。
 
```
  public HttpResponse prepareRequest(String sql) throws IOException {
    String KYLIN_PROJECT = getProperty(KYLIN_QUERY_PROJECT);
……
……
//从配置项中读取帐号密码，并基于Base64进行编码
    byte[] encodeBytes = Base64.encodeBase64(new String(getProperty(KYLIN_USERNAME)
        + ":" + getProperty(KYLIN_PASSWORD)).getBytes("UTF-8"));
    //设置请求参数
    String postContent = new String("{\"project\":" + "\"" + KYLIN_PROJECT + "\""
        + "," + "\"sql\":" + "\"" + sql + "\""
        + "," + "\"acceptPartial\":" + "\"" + getProperty(KYLIN_QUERY_ACCEPT_PARTIAL) + "\""
        + "," + "\"offset\":" + "\"" + getProperty(KYLIN_QUERY_OFFSET) + "\""
        + "," + "\"limit\":" + "\"" + getProperty(KYLIN_QUERY_LIMIT) + "\"" + "}");
    postContent = postContent.replaceAll("[\u0000-\u001f]", " ");
    StringEntity entity = new StringEntity(postContent, "UTF-8");
    entity.setContentType("application/json; charset=UTF-8");
    HttpPost postRequest = new HttpPost(getProperty(KYLIN_QUERY_API_URL));
postRequest.setEntity(entity);
//设置请求头信息，加上Basic Authentication信息
    postRequest.addHeader("Authorization", "Basic " + new String(encodeBytes));
    postRequest.addHeader("Accept-Encoding", "UTF-8");

    HttpClient httpClient = HttpClientBuilder.create().build();
    return httpClient.execute(postRequest);
  }
```

Zeppelin的前端有自己的schema，所以KylinInterpreter需要把KAP返回的数据进行适当的转换让Zeppelin前端理解。所以KylinInterpreter主要做的事情就是拼接参数完成向KAP server端段HTTP 请求，然后对返回结果格式化。

### 如何使用Zeppelin访问KAP

首先读者需要到[Zeppelin](http://zeppelin.apache.org/)官网下载0.5.6或之后的版本的二进制包，按照官网提示配置启动之后打开Zeppelin前端页面（官网有非常详细的介绍，这里就不赘述）。
* 配置Interpreter
打开Zeppelin配置页面，点击‘Interpreter’页面，创建针对KAP某个项目的Interpreter配置，如下图。

![](/images/integration/zeppelin/zeppelin_config.png)

* 查询
打开Notebook创建一个新的note，在该note中输入SQL，注意针对KAP的查询需要在SQL前面加上 ‘％kylin’，Zeppelin后端需要知道对应用哪个Interpreter去处理查询。效果如下图，可以拖拽维度和度量灵活获取自己想要的结果。

![](/images/integration/zeppelin/zeppelin_query.png)

* Zeppelin发布功能

Zeppelin中的任何一个查询，你都可以创建一个链接，并且将该链接分享给其他人。
感兴趣的读者可以到Zeppelin官网 http://zeppelin.apache.org/ 了解更多特性。


