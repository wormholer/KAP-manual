## KAP 2.5 Release Notes

KAP V2.5 was released recently. This release introduces a new feature of **multi-level and fine granularity access control**, which enables enterprises to enhance their security entirely and satisfies the requirements of enterprise customers and group customers for fine granularity access control to data security and user permissions. Meanwhile, KAP V2.5 enriches intelligent modeling experiences, enables to generate models automatically through SQL, and supports **various storage optimization strategies**. In addition, new-added function, **SQL validation**, allows the rapid response to the changing business analysis requirements.

### Enhanced Enterprise-level Security

**Multi-level Access Control**

The new released KAP V2.5 provides the concise and complete access control system. Permissions can be managed from coarse to granular differentially. 

**Project-level and Table-level Access Control**

Project-level Access Control allows to control the different-level permissions based on a user's role. In the same project, now users can use Table-level Access Control to differentially configure the access permission to the source table. Project-level Access Control and Table-level Access Control can offer a unified and flexible permission control that works for different subsidiaries, departments, business units under one system. 

**Row/Column-level Access Control**

Fine granularity row/column access control grains the permission to cell level, satisfies the requirements for enterprise-level users to isolate the business data and the customer personal information.

### Intelligent Modeling Experiences 

**Model Recommendation**

The new release supports automatic model generation through SQL. Once a source table is imported, the model will be generated through SQL automatically, which achieves one-click generation from SQL to model. 

**Rapid SQL Validation**

Without buliding, KAP V2.5 can rapidly validate if a model meets the needs of business query SQL after modeling, thus responding to the changing business analysis requirements fastly. 

**Various Cube Optimization Strategies**

Cube optimizer offers various strategies. Priority Strategy takes full advantages of data's logical relations to optimize Cube, thus satisfying the needs of flexible query scenarios. Business Priority Strategy is business oriented, which can accelerate the specified SQL and support the general report query modes with the minimum storage cost. Mixed Optimization Strategy supports two or more requirements and can meet the needs of various optimization scenarios. 

### Other Improvements

The new release also provides much richer sample data and streaming data model, facilitating to rapidly validate the integration with streaming datasource. 

KyAnalyzer reads the KAP license directly and does not require a separate license file.

### Hadoop Distribution Support

  Certificated distributions：

  	Cloudera CDH 5.7+
  	
  	MapR 5.2.1

  Compatible distributions：

  	Apache Hadoop 2.2+，HBase 0.98+，Hive 0.14+

  	Hortonworks HDP 2.2+

  	Azure HDInsight 3.4~3.6 

  	AWS EMR 5.1~5.7

  	Huawei FusionInsight C50/C60+

### Download

KAP V2.5 is available for download. Please visit [KAP Products](http://en.kyligence.io/assets/views/products) for more information.