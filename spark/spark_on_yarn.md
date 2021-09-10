
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

### Spark History Server:

Every SparkContext launches a Web UI, by default on port 4040 (in case of multiple spark context 4041, 4042, etc), that displays useful information about the application. 

Note that this information is only available for the duration of the application by default. To view the web UI after the fact, spark history server is needed.

Spark conf required for history server:
```$xslt
spark.eventLog.dir=hdfs:///installations/spark/history
spark.eventLog.enabled=true
spark.yarn.historyServer.address=http://127.0.0.1:18080
spark.history.fs.logDirectory=hdfs:///installations/spark/history
spark.history.fs.cleaner.enabled=true
spark.history.fs.cleaner.maxAge=30d
spark.history.fs.cleaner.interval=7d
```
(More configuration [here](https://spark.apache.org/docs/latest/monitoring.html))

#### Start History Server
```$xslt
$SPARK_HOME/sbin/start-history-server.sh
```

#### Stop History Server
```$xslt
$SPARK_HOME/sbin/stop-history-server.sh
```

#### Access History Server
This creates a web interface at http://<server-url>:18080 by default, listing incomplete and completed applications and attempts.
```$xslt
http://127.0.0.1:18080/
```