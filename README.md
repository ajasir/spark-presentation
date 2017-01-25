#Introduction To Spark 

Motivation
==========
MapReduce program simplified big data analytics.  People need more complex application that often has multiple Map Reduce Stages such as Iterative machine learning algorithm and iterative graph alogrithms. Other thing they want to do is Interactive data mining, Interactive queries. 

But map reduce programs are slow due to replication and I/O storage since these are very essential for fault tolerence - don't want to loose the data. As a solution we have in-memory data sharing mechanism which is 10-100x faster than network/disk but how to achieve fault tolerence using this ? So the challenge is "How to design a distributed memory abstraction that is both fault-tolerence and efficient ?" 

There are many in-memory storage abstractions for clusters such as RAMCloud,Piccolo etc..( still we are not recommending these. why ?)  But one thing that makes it hard for these to be really fast is an Existing storage abstractions that have interfaces(programming) based on fine-grained updates to mutable states. In all these systems, we got a table of cells, and we can go read and write any cells in that table.  So in order to provide fault tolerence, we require to, either replicate data (in cell level) or we should keep a logs across nodes which means larger I/O operations. This in turn cause expensiveness in data-intensive application and 10 to 100x slower than memory write

So the question comes here "How we can achieve FT without replicating data ?". The answer is RDD (Resilient Distributed Dataset). Resilient means that it can be recomputed in case Spark looses a part of the data. 

RDD overcomes above problems by restricting programming intefaces by two ways. 
1) RDDs should be immutable, partitioned data set across the cluster.
2) RDDs can be built only through coarse-grained operations. Coarse-grained operations are transformations like map, join, filter etc... Coarse-grained operations are applied to all of the elements in the dataset at once. 

Fault tolerence has been done using "lineage". In this we remember the graph of transformation that we used to build each RDD and when we loose data, we just re run the functions (used for transformation) on lost data. Here we log only the transformation function details not the data. 

refer the RDD recovery screenshot

So we got a distributed memory abstraction that is both FT and efficient. But how general is RDD?
RDD support many existing parallel algorithms.
RDD can be used for many cluster programming models such as MapReduce,Pregel(programming model for Large scale graph problems by google) etc..
Supports new apps that the existing models don't, such as interactive queries. 

So we understood RDD is very useful since it is very general. But how do we implement RDD? The answer is Spark. For implementing RDD, Spark is built.


Spark
=====

Spark provides a simple programing interface for RDDs


Definition
----------
Apache Spark is an open source parallel processing framework that enables users to run large-scale data analytics applications across clustered computers. 

Details
--------
Main developers of Spark is Apache Software Foundation, UC Berkeley, AMPLab and Databrick (Matei Zaharia, CTO, who created Spark). 
Written in Scala,Python and Java
Support Linux, Mac OS, Windows
Spark provides an interactive Scala and Python interpreter.

Apache Spark can process data from a variety of data repositories, including the Hadoop Distributed File System (HDFS), NoSQL databases and relational data stores such as Apache Hive.

Spark supports in-memory processing to boost the performance of big data analytics applications, but it can also do conventional disk-based processing when data sets are too large to fit into the available system memory.

As it is very well known, Hadoop MapReduce framework is primarily designed for batch processing and that makes it less suitable for ad-hoc data exploration, machine learning processes and the like.

Spark Programming Interfaces
============================
Sprak programming Interfaces provides the following
1)RDD
2)Operations on RDD (Transformations and Actions)
3)Control of each RDD's partitioning(layout across nodes) and persistence (storage in RAM, in Disks etc) 

RDDs support two types of operations: transformations, which create a new dataset from an existing one, and actions, which return a value to the driver program after running a computation on the dataset.

Example(Log mining)
------------------
Suppose you have a large log file on cluster and you are going to find some patterns on it.  So on cluster you have master node and worker nodes. Master node is used for typing our commands,job scheduling, tracking of lineage for the data sets.

val lines = sc.textFile("hdfs://---") - base RDD. Here we create a RDD of the log file 
val errors = lines.filter(_.startsWith("ERROR")) - Transformed RDD
Suppose the errors are tab seperated and the message is at the second position. Then we are going to create another RDD
var message = errors.map(_.split('\t')(2))
If you want to persist this message for faster access you can do it by 
message.persist() 
Here we persit only message which is less compared to the actual large log files. 

All transformations in Spark are lazy, in that they do not compute their results right away. Instead, they just remember the transformations applied to some base dataset (e.g. a file). The transformations are only computed when an action requires a result to be returned to the driver program. So nothing will be happend on cluster unless we call an action. So now we call an action 

message.filter(_.contains("foo")).count 

when this action called, master will look where the data is on the cluster, and send tasks to worker nodes. Node will compute the action and send back the results like mapreduce. But the nodes will save it in the memory instead of hdfs file system. (persist the action rdd, which contains the result). Next time if you do any actions on the same RDD, it will hit on the memory and get back the result much faster. 
message.filter(_.contains("bar")).count will compute much faster than above

How speed is this ?
Full-text search of wikepeida data of 50GB on 20 machines, spark takes < 1s. Hadoop takes 20 seconds for the same. If scaled to ITB data on 100 machines, Hadoop took 170 seconds where spark took 5-7 seconds


