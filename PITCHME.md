##<span style="color:#0b8ff2">R</span>esilient <span style="color:#0b8ff2">D</span>istributed <span style="color:#0b8ff2">D</span>atasets
<hr>
A Fault-Tolerant Abstraction for In-Memory Cluster Computing

#HSLIDE
##<span style="color:#0b8ff2;text-align:left">Map Reduce</span>
<hr>

#HSLIDE
<p style="text-align:left">Map Reduce greatly simplified “Big Data” analysis on large, unreliable clusters</p>
<br>
<p style="text-align:left">But as soon as it got popular, users wanted more</p>

* Complex application that often has multiple Map Reduce Stages
	  
	<small>(Ex:-  Iterative machine learning algorithms  & Iterative graph alogrithms)</small>
    
* Interactive data mining


#HSLIDE
## Why Not <span style="color:#0b8ff2;text-align:left">Map Reduce</span> ?
<hr>

#HSLIDE
<p style="text-align:left">Complex apps and interactive queries both need one thing that MapReduce lacks:</p>
<br>
<p style="text-align:left">Efficient primitives for <span style="color:#0b8ff2"><b>Data Sharing</b></span></p>
<br>
<p style="text-align:left; border:solid 1px red; padding: 25px;">Unfortunately, in MapReduce, the only way to share data across jobs is stable storage such as dfs, which is <span style="color:#0b8ff2"><b>slow</b></span> ! </p>  <!-- .element: class="fragment" data-fragment-index="1" -->
    
#HSLIDE
![Map Reduce data sharing diagram ](/home/557920/Documents/Training Material/Spark/images/mapreduce_datasharing.jpg)
<p style="text-align:left; border:solid 1px red; padding: 10px;"> <span style="color:#0b8ff2"><b>Slow</b></span> due to replication and disk I/O, but necessary for fault tolerance </p>  <!-- .element: class="fragment" data-fragment-index="1" -->

#HSLIDE
##<span style="color:#0b8ff2;text-align:left">Goal: In-Memory Data Sharing </span>
<hr>

#HSLIDE
![In Memory Data Sharing ](/home/557920/Documents/Training Material/Spark/images/in-memmory.jpg)
<p style="text-align:left; border:solid 1px red; padding: 10px;">10-100x faster than network/disk, but how to achieve <span style="color:#0b8ff2;text-align:left"><b>Fault Tolerence</b></span> </p>  <!-- .element: class="fragment" data-fragment-index="1" -->

#HSLIDE
##<span style="color:#0b8ff2;text-align:left">Challenge </span>?
<hr>

#HSLIDE
####"How to design a distributed memory abstraction that is both <span style="color:#0b8ff2;text-align:left">Fault-Tolerant</span> and <span style="color:#0b8ff2;text-align:left">Efficient</span> ?" 

#HSLIDE
<p style="text-align:left">There are many in-memory storage abstractions for clusters such as  RAMCloud, Piccolo etc..</p>
<p style="text-align:left">Still we are not using these. <span style="color:#0b8ff2;text-align:left">Why</span> ?</p>  <!-- .element: class="fragment" data-fragment-index="1" -->
* It has interfaces(programming) based on fine-grained updates to mutable states <!-- .element: class="fragment" data-fragment-index="2" -->
* In all these systems, we got a table of cells, and  we can go read and write any cells in that table <!-- .element: class="fragment" data-fragment-index="2" -->

#HSLIDE
* So in order to provide fault tolerance, we require to, either
     * replicate data in cell level  <!-- .element: class="fragment" data-fragment-index="1" -->
     * keep logs across nodes => larger I/O operations <!-- .element: class="fragment" data-fragment-index="2" -->
<br>
<br>
<p style="text-align:left; border:solid 1px red; padding: 10px;">  This in turn cause expensiveness in data-intensive application and 10 to 100x slower than memory write </p> <!-- .element: class="fragment" data-fragment-index="3" -->
 
#HSLIDE
####"How can we have <span style="color:#0b8ff2;text-align:left">Fault-Tolerance</span> in memory storage systems without <span style="color:#0b8ff2;text-align:left">Replication</span> ?" 

#HSLIDE
##<span style="color:#0b8ff2;text-align:left"> Solution</span>
<hr>

#HSLIDE
##<span style="color:#0b8ff2;text-align:left">R</span>esilient <span style="color:#0b8ff2;text-align:left">D</span>istributed <span style="color:#0b8ff2;text-align:left">D</span>atasets
<hr>

#HSLIDE
<p style="text-align:left">/rɪˈzɪlɪənt/</p>
<p style="text-align:left">adjective</p>
* (of a substance or object) able to recoil or spring back into shape after  bending, stretching,or being compressed. 
* (of a person or animal) able to withstand or recover quickly from difficult conditions.
<p style="text-align:left; border:solid 1px red; padding: 5px;">In <span style="color:#0b8ff2"><b>RDD</b></span>, Resilient means that it can be recomputed in case it looses a part of the data.</p> <!-- .element: class="fragment" data-fragment-index="1" -->

   
#HSLIDE
<p style="text-align:left">RDD overcomes above problems by restricting programming interfaces by two ways</p>
* RDDs should be immutable, partitioned data set across the cluster.
* RDDs can be built only through coarse-grained operations.
<br>
<br>
<small>Coarse-grained operations are transformations like <span style="color:#0b8ff2">map,join, filter etc...</span></small>
<br>
Coarse-grained operations are applied to all of the elements in the dataset at once.

#VSLIDE
##<span style="color:#0b8ff2;text-align:left">Granularity</span>

#VSLIDE
* Granularity  is the extent to which a material or system is composed of distinguishable pieces or grains. 
* For example, a kilometer broken into centimeters has finer granularity than a  kilometer broken into meters.
* <span style="color:#0b8ff2">Coarse-grained</span> materials or systems have fewer, larger discrete components than fine-grained materials or systems.
* <span style="color:#0b8ff2">Fine-grained</span> description regards smaller components of which the larger ones are composed.


#HSLIDE
##<span style="color:#0b8ff2;text-align:left">Fault Tolerence</span>
<hr>

#HSLIDE
* Efficient Fault-Tolerance using <span style="color:#0b8ff2"><b>Lineage</b></span>
* In this we remember the graph of transformation that we used to build each RDD
* when we loose data, we just re run the functions (used for transformation) on lost data
* Here we log only the transformation function details not the data
* No cost if nothing fails

#HSLIDE
##<span style="color:#0b8ff2;text-align:left">RDD Recovery</span>

#HSLIDE
![RDD Recovery](/home/557920/Documents/Training Material/Spark/images/lineage.jpg)

#HSLIDE
![RDD Recovery](/home/557920/Documents/Training Material/Spark/images/lineage_failure1.jpg)

#HSLIDE
![RDD Recovery](/home/557920/Documents/Training Material/Spark/images/lineage_failure2.jpg)

#HSLIDE
## <span style="color:#0b8ff2;text-align:left">Generality of RDDs</span>
<hr>

#HSLIDE
<p style="text-align:left">So we got a distributed memory abstraction that is both <span style="color:#0b8ff2">FT</span> and <span style="color:#0b8ff2">Efficient</span>.</p>
<p style="text-align:left">But <span style="color:#0b8ff2;text-align:left">How</span> general/good is  <span style="color:#0b8ff2;text-align:left">RDD</span> ? </p>
* RDD support many existing parallel algorithms.
* RDD can be used for many cluster programming models such as MapReduce, Pregel(programming model for large scale graph problems by google) etc..
* Supports new apps that the existing models don't, such as interactive queries.


#HSLIDE
####"So we understood <span style="color:#0b8ff2;text-align:left"><b>RDD</b></span> is very useful since it is very general. But how do we implement <span style="color:#0b8ff2;text-align:left"><b>RDD</b></span>?"

#HSLIDE
##<span style="color:#0b8ff2;text-align:left">Spark</span>
<hr>

#HSLIDE
* For implementing RDD, Spark is built.
* Spark provides a simple programing interface for RDDs
<br>
<p style="text-align:left"><span style="color:#0b8ff2;text-align:left">Definition</span></p>
Apache Spark is an open source parallel processing framework that enables users to run large-scale data analytics applications across clustered computers.

#HSLIDE
##<span style="color:#0b8ff2;text-align:left">Features</span>
<hr>

#HSLIDE
* Written in Scala,Python and Java
* Support Linux, Mac OS, Windows
* Spark provides an interactive Scala and Python interpreter
* It can process data from a variety of data repositories
    * Hadoop Distributed File System (HDFS)
    * NoSQL databases and relational data stores
* Supports in-memory processing & conventional disk-based processing (data sets are too large to fit into the memory)

#HSLIDE
##<span style="color:#0b8ff2;text-align:left">Spark Programming Interfaces</span> 
<hr>

#HSLIDE
<p style="text-align:left">Spark programming Interfaces provides the following:</p>
* RDD

* Operations on RDD

    * Transformations - build new RDDs

    * Actions - compute and output results


#HSLIDE
##Example - <span style="color:#0b8ff2;text-align:left">Log mining</span>
<hr>

#HSLIDE
####Suppose you have a large log file on cluster and you are going to find some patterns on it

#HSLIDE
<p style="text-align:left"><span style="color:#0b8ff2;text-align:left"><b>Working Model</b></span></p>
<p style="text-align:left">On cluster you have master node and worker nodes. Master node is used for typing our commands, job scheduling, tracking lineage for the data sets etc..</p>
<p style="text-align:left"><span style="color:#0b8ff2;text-align:left"><b>Spark Context</b></span></p>
<p style="text-align:left">The door to Spark represents the connection to a Spark execution environment</p>

#HSLIDE
##<span style="color:#0b8ff2;text-align:left">Code</span>
<hr>

#HSLIDE

```scala
//Here we create a RDD of the log file
//This is the Base RDD
val lines = sc.textFile("hdfs://---")
``` 
```scala
//Transformed RDD from base RDD
val errors = lines.filter(_.startsWith("ERROR"))
``` 
<!-- .element: class="fragment" data-fragment-index="2" -->
```scala
//Suppose the errors are tab seperated and the error 
//message is at the second position
//Then we are going to create another RDD
val message = errors.map(_.split('\t')(2))
``` 
<!-- .element: class="fragment" data-fragment-index="3" -->
```scala
//If you want to persist this message 
//for faster access you can do it by
message.persist()
``` 
<!-- .element: class="fragment" data-fragment-index="4" -->
```scala
//Here we persit only message which is 
//less compared to the actual large log files.
``` 
<!-- .element: class="fragment" data-fragment-index="5" -->

#HSLIDE
* All transformations in Spark are lazy, in that they do not compute their results right away.
* Instead, they just remember the transformations applied to some base data set.
* The transformations are only computed when an action requires a result to be returned to  the driver program.
* So nothing will be happend on cluster unless we call an action.
* Now we call an <span style="color:#0b8ff2"><b>Action</b></span> in above program.

#HSLIDE
```scala
//Call action 'count'
message.filter(_.contains("foo")).count
```
<br>
<p style="text-align:left">when this action (<span style="color:#0b8ff2">count</span>) called</p>

* Master will look where the data is on the cluster and send tasks to worker nodes <!-- .element: class="fragment" data-fragment-index="1" -->
* Node will compute the action and send back the results like mapreduce <!-- .element: class="fragment" data-fragment-index="2" -->

#HSLIDE
* But the nodes will save it in the memory instead of hdfs file system (persist the action rdd, which contains the result) <!-- .element: class="fragment" data-fragment-index="3" -->
* Next time if you do any actions on the same RDD, it will hit on the memory and get back the result much faster <!-- .element: class="fragment" data-fragment-index="4" -->

#HSLIDE
## <span style="color:#0b8ff2;text-align:left">Speed</span>
<hr>

#HSLIDE
<p style="text-align:left">How <span style="color:#0b8ff2;text-align:left">speed</span> is this ?</p>
<p style="text-align:left">Full-text search of wikipedia data of <span style="color:#0b8ff2">50GB on 20 machines</span></p>
    * Spark takes less than  1 second
    * Hadoop takes 20 seconds for the same

<p style="text-align:left">If scaled to <span style="color:#0b8ff2">ITB data on 100 machines</span></p>
	* Spark took 5 to 7 seconds
	* Hadoop took 170 seconds

#HSLIDE
##<span style="color:#0b8ff2;text-align:left">Thank You</span>
<hr>

#HSLIDE
####<p style="text-align:left"><span style="color:#0b8ff2;text-align:left">References</span></p>
<hr>
* https://www.usenix.org/conference/nsdi12/technical-sessions/presentation/zaharia
* http://spark.apache.org/docs/latest/
