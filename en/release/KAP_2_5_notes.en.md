## KAP 2.5 Release Note

KAP v2.5 is general available recently. This release introduces **multi-level access control and fine granularity**, which enhance enterprises security entirely and satisfie the requirements of enterprise customers and group customers for fine granularity access control and user permissions. Meanwhile, KAP v2.5 enriches intelligent modeling assistance, enables to generate models automatically through SQL patterns, and supports **various storage optimization strategies**. In addition,  introducing a new highlight, **SQL validation**, enable the quick response to the varied business analysis demand.

### Enhanced Enterprise-level Security

**Multi-level Access Control**

KAP v2.5 provides a comprehensive access control system. Control your data access on different granularity as project level, table level, column level and row level.

**Project-level and Table-level Access Control**

Project-level Access Control allows to manage the different-level permissions based on a user's role. In the same project, now you can use Table-level Access Control to differentially configure the access to the source table. Project-level Access Control and Table-level Access Control can offer a unified and flexible permission control that works for different subsidiaries, departments, business units under one system. 

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
