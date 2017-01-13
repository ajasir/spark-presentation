#HSLIDE

# <span style="color:#0b8ff2;text-align:left">R</span>esilient 
# <span style="color:#0b8ff2;text-align:left">D</span>istributed
# <span style="color:#0b8ff2;text-align:left">D</span>atasets
<hr>
A Fault-Tolerant Abstraction for In-Memory Cluster Computing

#HSLIDE

# <span style="color:#0b8ff2;text-align:left">Map Reduce</span>


#HSLIDE

MapReduce greatly simplified “big data” analysis on large, unreliable clusters 

But as soon as it got popular, users wanted more:

* Complex application that often has multiple Map Reduce Stages

    (eg :- Iterative machine learning algorithms  & Iterative graph alogrithms)
    
* Interactive data mining. 


#HSLIDE

# Why Not <span style="color:#0b8ff2;text-align:left">Map Reduce</span> ?

#HSLIDE

Complex apps and interactive queries both need one thing that MapReduce lacks:

Efficient primitives for <span style="color:#0b8ff2;text-align:left">Data Sharing</span>


<table>
  <tr>
    <td>Unfortunately, in MapReduce, the only way to share data across jobs is stable storage such as dfs, which is slow !</td>
    </tr>
   </table>
    
#HSLIDE

to be filled

<table>
  <tr>
    <td>Slow due to replication and disk I/O, but necessary for fault tolerance </td>
    </tr>
   </table>

#HSLIDE

# <span style="color:#0b8ff2;text-align:left">Goal: In-Memory Data Sharing </span>

#HSLIDE

to be filled

#HSLIDE

to be filled
<table>
  <tr>
    <td>10-100x faster than network/disk, but how to achieve <span style="color:#0b8ff2;text-align:left">F</span>ault <span style="color:#0b8ff2;text-align:left">T</span>olerence</td>
    </tr>
   </table>

#HSLIDE

<span style="color:#0b8ff2;text-align:left">Challenge </span>?

#HSLIDE

"How to design a distributed memory abstraction that is both <span style="color:#0b8ff2;text-align:left">Fault-Tolerant</span> and <span style="color:#0b8ff2;text-align:left">Efficient</span> ?" 

#HSLIDE

There are many in-memory storage abstractions for clusters such as  RAMCloud, Piccolo etc..

Still we are not using these. <span style="color:#0b8ff2;text-align:left">Why</span> ?

* It has interfaces(programming) based on fine-grained updates to mutable states

* In all these systems, we got a table of cells, and  we can go read and write any cells in that table
 
* So in order to provide fault tolerance, we require to, either
     * replicate data in cell level
     * keep logs across nodes => larger I/O operations
        
<table>
  <tr>
    <td>This in turn cause expensiveness in data-intensive application and 10 to 100x slower than memory write</td>
    </tr>
   </table>
 
#HSLIDE  

"How can we have <span style="color:#0b8ff2;text-align:left">Fault-Tolerance</span> in memory storage systems without <span style="color:#0b8ff2;text-align:left">Replication</span> ?" 

#HSLIDE 
 
<span style="color:#0b8ff2;text-align:left"> Solution</span>
 
#HSLIDE 
<span style="color:#0b8ff2;text-align:left">R</span>esilient <span style="color:#0b8ff2;text-align:left">D</span>istributed <span style="color:#0b8ff2;text-align:left">D</span>atasets
   
#HSLIDE   

/rɪˈzɪlɪənt/

adjective

* (of a substance or object) able to recoil or spring back into shape after  bending, stretching,or being compressed. 

* (of a person or animal) able to withstand or recover quickly from difficult conditions.

<table>
  <tr>
    <td>In RDD, Resilient means that it can be recomputed in case it looses a part of the data.</td>
    </tr>
   </table>
   
#HSLIDE

RDD overcomes above problems by restricting programming interfaces by two ways. 
* RDDs should be immutable, partitioned data set across the cluster.
* RDDs can be built only through coarse-grained operations.
Coarse-grained operations are transformations like map,join, filter etc...

Coarse-grained operations are applied to all of the elements in the dataset at once.

#HSLIDE

# <span style="color:#0b8ff2;text-align:left">Fault Tolerence</span>

<hr>

* Efficient Fault-Tolerance using Lineage 
* In this we remember the graph of transformation that we used to build each RDD
* when we loose data, we just re run the functions (used for transformation) on lost data
* Here we log only the transformation function details not the data
* No cost if nothing fails

#HSLIDE

# <span style="color:#0b8ff2;text-align:left">Granularity</span>

#HSLIDE

* Granularity  is the extent to which a material or system is composed of distinguishable pieces or grains. 
* For example, a kilometer broken into centimeters has finer granularity than a  kilometer broken into meters.
* Coarse-grained materials or systems have fewer, larger discrete components than fine-grained materials or systems.
* Fine-grained description regards smaller components of which the larger ones are composed.

#HSLIDE
# <span style="color:#0b8ff2;text-align:left">RDD Recovery</span>

#HSLIDE
RDD Recovery
<hr>
to be filled

#HSLIDE

# <span style="color:#0b8ff2;text-align:left">Generality of RDDs</span>

#HSLIDE

So we got a distributed memory abstraction that is both FT and efficient.

But <span style="color:#0b8ff2;text-align:left">How</span> general/good is  <span style="color:#0b8ff2;text-align:left">RDD</span> ?

* RDD support many existing parallel algorithms.
* RDD can be used for many cluster programming models such as MapReduce, Pregel(programming model for 
* Large scale graph problems by google) etc..
* Supports new apps that the existing models don't, such as interactive queries. 

#HSLIDE

"So we understood <span style="color:#0b8ff2;text-align:left">RDD</span> is very useful since it is very general. But how do we implement <span style="color:#0b8ff2;text-align:left">RDD</span> ?" 

#HSLIDE

# <span style="color:#0b8ff2;text-align:left">Spark</span>

#HSLIDE

For implementing RDD, Spark is built.
Spark provides a simple programing interface for RDDs
Definition
Apache Spark is an open source parallel processing framework that enables users to run large-scale data analytics applications across clustered computers.


