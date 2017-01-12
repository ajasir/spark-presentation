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

Why Not <span style="color:#0b8ff2;text-align:left">Map Reduce</span> ?

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

<span style="color:#0b8ff2;text-align:left">Goal: In-Memory Data Sharing </span>

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

style="color:#0b8ff2;text-align:left">Challenge </span>?

#HSLIDE

"How to design a distributed memory abstraction that is both <span style="color:#0b8ff2;text-align:left">Fault-Tolerant</span> and <span style="color:#0b8ff2;text-align:left">Efficient</span> ?" 

#HSLIDE



