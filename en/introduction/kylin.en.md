## Apache Kylin

Apache Kylin™ is an open source Distributed Analytics Engine designed to provide SQL interface and multi-dimensional online analytical processing (OLAP) on Hadoop supporting extremely large datasets.

Apache Kyiln builds on Hadoop and other distributed computing platforms, making full use of MapReduce parallel processing capabilities and scalable infrastructure, efficiently processing of Big Data, realizing architecture scale according to the size of data. Apache Kylin as an OLAP engine supports importing the data from the data source (Hive / Kafka, etc.), buidling multi-dimensional cubes based on MapReduce platform, and taking advantages of the HBase's extentable features to distribute the cube index data, providing standard SQL parsing and query optimization, ODBC / JDBC driver and REST API. The flexible architecture that allows for more data sources to be plugged into Kylin, as well as support for other storage engines. Apache Kylin contains the following core components

**Metadata Management**: Includes model design, Cube design, source table synchronization, data sampling and so on.  It supports hierarchy dimension, joint dimension and derived dimension. These design technologies can be used to optimize the cube build and limit the Cube expansion. Supports a variety of dictionary encoding algorithms to achieve efficient data compression.

**Task Engine**: Used to submit Cube build tasks to Hadoop platform. It supports full table build, incremental build and streaming build. Supports Cube auto-merging to optimize the query IO. Supports layered build and in-memory fast build algorithms and dozens of performance tuning parameters, giving full play to MapReduce computing power.

**Storage Node**: The source data will be pre-calculated by task engine and the result index data will be stored into HBase, which is a high-throughput, large-scale concurrent fast reading and writing key-value database. Kylin leverages HBase Fuzzy Key filter and Coprocessor parallel processing technology to retrieve parallel data efficiently. Supports pushing down the filter and aggregation logic into storage node. Kylin reduces the data retrieval complexity from O(N) to O(1).

**Query Node**: Build on top of the Apache Calcite parser, supports for JDBC / ODBC / REST protocols and interfaces, supports ANSI SQL and most SQL functions, provides user defined function mechanism. Seamless integrated with mainstream BI tools, such as Tableau.

**Web Management**: Build in user-friendly web interface, supports wizard-based model building, intuitive task monitoring and alarm, and permission management.

![](images/kylin_arch.jpg)