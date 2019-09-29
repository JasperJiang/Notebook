# HDFS

## Introduction to HDFS

### The Hadoop Distributed File System \(HDFS\)

* **HDFS is the storage layer for Hadoop**
* **A filesystem which can store any type of data**
* **Provides inexpensive and reliable storage for massive amounts of data**
  * Data is replicated across computers
* **HDFS performs best with a 'modest' number a large files**
  * Millions, rather than billions, of files
  * Each file typically 100MB or more
* **File in HDFS are 'write once'**
  * Appends are permitted
  * No random writes are allowed

### HDFS Basic Concepts

* **HDFS is a filesystem written in Java**
* **Sits on top of a native filesystem**
* **Scalable**
* **Fault tolerant**
* **Supports efficient processing with MapReduce, Spark, and other frameworks**

## How Files are Stored in HDFS

* **Data files are split into blocks and distributed to data nodes**
* **Each block is replicated on multiple nodes \(default: 3x replication\)**
* **NameNode stores metadata**

![](../.gitbook/assets/image%20%2820%29.png)



## **Getting Data In and Out of HDFS**

* **Hadoop**
  * Copies data between client\(local\) and HDFS \(cluster\)
  * API or command line
* **Ecosystem Project**
  * Flume
    * Collects data from network sources \(e.g. , websites, system logs\)
  * Sqoop
    * Transfers data between HDFS and RDBMSs
* **Business Intelligence Tools**

### Storing and Retrieving Files

![](../.gitbook/assets/image%20%2832%29.png)

![](../.gitbook/assets/image%20%2838%29.png)

![](../.gitbook/assets/image%20%288%29.png)

