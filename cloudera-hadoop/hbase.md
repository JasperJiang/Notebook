# HBase

HBase is a NoSQL distributed database

Stores data in HDFS

Scales to support very high throughput for both reads and writes

* Millions of inserts or updates per second

A table can have many thousands of columns

* Handles sparse data well

Designed to store very large amounts of data \(Petabytes+\)

![](../.gitbook/assets/image%20%2814%29.png)

**Use HBase if ...**

* You need random reads
* You need random writes
* You need to do thousands of operations per second on terabytes of data
* Your access patterns are simple and well-known

**Don't use HBase if ...**

* You only append to your dataset and typically read the entire table
* You primarily perform ad-hoc analytics \(ill-defined access patterns\)
* Your data easily fits on one large node



