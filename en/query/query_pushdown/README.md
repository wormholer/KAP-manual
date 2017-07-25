# Query Pushdown

KAP supports query pushdown from KAP v2.4. If queries cannot be anwsered by Cube, you can simply leverage query pushdown to redirect the query to Spark SQL or Hive, making a trade-off between query latency and query flexibility to obtain a better experience.

Continue reading:

[Ture on Query Pushdown](pushdown.en.md)

[Spark engine integration](spark.en.md)

[Impala engine integration](impala.en.md)

