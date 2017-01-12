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
 

