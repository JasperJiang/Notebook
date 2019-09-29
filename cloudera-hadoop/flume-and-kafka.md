# Flume and Kafka

## What are Flume and Kafka

### Flume and Kafka are tools for ingesting event data into Hadoop as that data is being generated

* Log files
* Sensor data
* Streaming data from social media such as Twitter
* Etc.

### Flume is typically easier to configure, but Kafka provides more functionality

* Flume generally provides a path from a data source to HDFS or to a streaming framework such as Spark
* Kafka uses a 'Publish/Subscribe' model
  * Allows data to be consumed by many different systems, including writing to HDFS

### Why using Kafka and Flume?

Flume and Kafka are ideal for aggregating event data from many sources into a centralized location \(HDFS\)

Allow you to process streaming data, as that data is being generated

Well-suited for event driven data

## Sqoop

Sqoop rapidly moves large amounts of data between relational database management systems \(RDBMSs\) and HDFS

* Import tables \(or partial tables\) from an RDBMS into HDFS
* Export data from HDFS to a database table

Uses JDBC to connect to the database

* Works with virtually all standard RDBMSs

Custom 'connectors' for some RDBMSs provide much higher throughput

![](../.gitbook/assets/image%20%2832%29.png)



