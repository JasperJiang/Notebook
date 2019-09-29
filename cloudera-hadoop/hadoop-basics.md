# Hadoop Basics

## What is Apache Hadoop

Hadoop is a software frame work for storing, processing, and analyzing "big data"

* Distributed
* Scalable
* Fault-tolerant
* Open source

### The Hadoop Distributed File System \(HDFS\)

* Any type of file can be stored in HDFS
* Data is split into chunks and replicated as it is written
  * Provides resiliency and high availability
  * Handled automatically by Hadoop

### YARN \(Yet Another Resource Negotiator\)

* Manages the processing resources of the Hadoop cluster
* Schedules jobs
* Runs processing frameworks

### MapReduce

* A distributed processing framework

### Hadoop is Scalable

* Adding nodes \(machines\) adds capacity proportionally
* Increasing load results in a graceful decline in performance
  * Not failure of the system

![](../.gitbook/assets/image%20%2822%29.png)

### Hadoop is Fault Tolerance

* Node failure is inevitable
* What happens?
  * System continues to function
  * Master re-assigns work to a different node
  * Data replication means there is no loss of data
  * Nodes which recover rejoin the cluster automatically

### Examples of Hadoop ecosystem projects

![](../.gitbook/assets/image%20%2825%29.png)



