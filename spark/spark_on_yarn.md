
# Spark On Yarn

### Spark Jars/Archives:
Spark job needs spark libraries on YARN containers. It creates zip folder containing all jars from $SPARK_HOME/jars/ & uploads to hdfs. It adds overhead to bootstrap spark jobs

This can be optimized by copying jars from $SPARK_HOME/jars/ directory (along with any additional dependent jars) to hdfs location e.g. hdfs:///installations/spark/jars/ and specify below config in spark-submit command

```$xslt
spark.yarn.jars=hdfs:///installations/spark/jars/
```

Increase the replication of this hdfs directory to number of data nodes. It further improves bootstrap time by making jars/libraries locally available on each node.
    
```$xslt
hdfs dfs –setrep -w 10 hdfs:///installations/spark/jars/
```