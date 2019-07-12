# Hive and Impala

## Hive

Hive is an abstraction layer on top of Hadoop

* Hive uses a SQL-like language called HIveQL

The Hive interpreter uses MapReduce or Spark to actually process the data

JDBC and ODBC drivers are available

Data can be loaded before the table is defined

* Schema-on-Read
* You do not need to know the data's structure prior to loading it

Does not require a developer who knows Java, Scala, Python or other traditional programming languages

Well suited for dealing with structured data, or data which can have a structure applied to it

![](../.gitbook/assets/image%20%289%29.png)

## Impala

**Impala is a high-performance SQL engine**

* Runs on Hadoop clusters
* Does not rely on MapReduce
* Massively-parallel processing \(MPP\)
* Inspired by Google's Dremel project
* Very low latency - typically measured in milliseconds

**Impala supports a dialect of SQL very similar to HIve's**

## **How to Choose Hive and Impala**

**Impala and Hive both provide a way to analyze data on the cluster using SQL**

**Impala is best suited for ad-hoc analytics, and situations where multiple people will be querying the cluster simultaneously**

* Impala is much faster than Hive
* Impala deals with multiple simultaneous users much better

**Hive is well suited for batch processing**

* ETL
* Scheduled workloads

