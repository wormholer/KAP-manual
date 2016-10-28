## Apache Kylin

Apache Kylin™ is an open source Distributed Analytics Engine designed to provide SQL interface and multi-dimensional analysis (OLAP) on Hadoop supporting extremely large datasets.

Apache Kyiln builds on Hadoop and other distributed computing platforms, making full use of MapReduce parallel processing capabilities and scalable infrastructure, efficiently processing of Big Data, realizing architecture scale according to the size of the data. Apache Kylin as an OLAP engine contains the source of data from the data source (Hive / Kafka, etc.), based on MapReduce to build multi-dimensional cube, and make full use of the HBase column features to distributed storage cube data, providing standard SQL parsing and Query optimization, and ODBC / JDBC driver and REST API and other modules. Pluggable, flexible architecture that allows for more data sources to be plugged into Kylin, as well as support for other technologies as a storage engine. Apache Kylin contains the following core components

**Metadata Management**: It includes model design, Cube design, table structure synchronization, data sampling and analysis and so on.  It support dimension dimension, joint dimension. It can be derived dimension optimization technology, and avoid Cube data expansion. It support a variety of dictionary encoding algorithm to achieve efficient data compression storage.

**Task Engine**: It is used to submit Cube build tasks to the Hadoop platform. It supports various construction mechanisms such as full table construction, incremental construction, and streaming architecture. It supports IO optimizations such as Cube auto-merging, built-in multiple Cube precomputing algorithms and dozens of Job performance tuning parameters, giving full play to MapReduce computing power.

**Storage Node**: The source data of the relational table are pre-calculated and stored in HBase, which is a key database, and supports high-throughput and large-scale concurrent fast reading and writing. HBase efficiently utilizes Fuzzy Key filter technology and Coprocessor parallel processing technology to retrieve parallel data Data, supports query logic under the storage node, achieves the data retrieval problem by the O(N) computing complexity reduced to O(1).

**Query Node**: It is built on top of the Apache Calcite parser, supports for JDBC / ODBC / REST and other protocols and interfaces, supports ANSI SQL, contains most of the SQL function, providing a custom calculation function mechanism, Tableau and other mainstream BI tools perfect docking.

**Web Management**: It is built in user-friendly interface, supports for wizard-based model building, intuitive task monitoring and alarm, and user rights management.

![](images/kylin_arch.jpg)