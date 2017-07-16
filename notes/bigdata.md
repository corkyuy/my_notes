# Big Data

## Measuring data

*WHAT THE FUCK!!!! Why are we spending 5 mins with this???*

* Gigabyte
* Terabyte
* Petabyte
* Exabyte
* Zettabyte

## Fundamental Data characteristic

* Unstructured vs Structured data
* Volume (Huge)
* Digital format

* Challenge is to make sense of the data

    is called ** Big Data Analysis **

## 3 V's (Fundamental Data characteristics)

* Velocity - how fast is data generated
* Volume - how large the data is generated
* Varity - how different data is

## Idea

1. Scale out, not UP
    Scale out -> large # of commodity of low end servers over small high-performing servers
1. Assume failures are common

    Make an assumption (e.g. failure after 1000 days)

    MTBF of 1000 days
1. What is the failure rate? 10 failures a day( how...)
    -> Scale out is about scaling, noot about failure
1. Move processing to the data
    Huge data > computation prower

    Since moving data is so huge, moving it is expensive

### MapReduce

 assumes processor & storage are co-located

### Distributed file system (DFS)
  manage date over which MapReduce resides


1. Process data sequentially and avoid random access

    dataset are too large to fit in memory

1. Hide system-level details from AP dev

**short-term memory** (primary/active)

1. Seamless scalability

Writing program for one node -> works for all nodes


## Why DFS

1 T / 400 MB/s * 1024 * 1024 =~ 

I/O is challenge not the storage capacity

* Physically located in different
* logically 1 file system

## Hadoop

* framework
* distributd processing
* large data sets
* cluster

* HDFS storage
* MapReduce processing

**NameNode** - (no data, keeps track where data is)


### HDFS

* fault-tolerant
* high throughput
* suitable for applications with large data set
* Write once -> read many times (getting entire date faster is more important than getting specific record)

**Minimum Block size 64MB**


#### Not suitable

* low latency
* 

### NameNode

* Master
* single point of failure
* maintains and manage blocks on DataNode
* expensive hardware with (RAID)

#### Secondary NameNode

Memory to Hard Disk

**NOT NameNode**

#### Job Tracker


#### RAID

Blahblahblah


### DataNode

* slaves
* serving clients (read/write)

### How data is stored

Block A,B,C

Rack 1 [1..4] - A,  C, C,

Rack 2 [5..8] - A, AB,

Rack 3 [9..12] - ,  B, B, C

| S | Rack 1 | Rack 2 | Rack 3 |
|---|--------|--------|--------|
| 1 | A C    | A      |        |
| 2 | C      | AB     | B      |
| 3 |        |        | B      |
| 4 |        |        | C      |


## MapReduce

### Basics 

**Divide and Conquer**

* Break up large to small (how to break up a large problem)
* Assign task to workers - some workers are better suited than others
* Ensure workers get the data
* Coordinate synchronization
* Share partial results
* How to take care of software errors/hardware faults

*MapReduce - abstracts system-level details from programmers*


fmap :: (a -> b) -> f a -> f b
fold :: (b -> a -> b) -> b -> f a -> b

### Problem

If odd # is equal return 0, if odds > even : positive #, otherwise negative #

**f(x)**

**g(x,i)**

**initial value**

```
{ 8, 7, 5, 12, 13}

f(x)          := (x % 2) ? 1 : -1;
g(x,i)        := x+i
initial value := 0
```

### Mappers and Reducers

**Key-value** pairs form the basic data struct in MapReduce

* map: (k1:v1)      => [(k2:v2)]
* reduce: (k2:[v2]) => [(k3:v3)]

Key-value pairs = split

mapper produces arbitrary number of *intermediate* key-value pairs

reducers applied to *all values* associated with the intermediate key to
generate output key-value pairs

**intermediate key-value** not stored permanently

node A => mapper produces [(a,1),(b,2),(a,3),...]

node B => mapper produces [(b,2),(c,3),...]

reducer 1 => (a,[1,3,...]

reducer 2 => (b,[2,2,...])


#### Word count algo

```
class Mapper
  method Map(docid a; doc d)
    for all term t in doc d do
      Emit(term t; count 1)

class Reducer
  method Reduce(term t; count[c1;c2;...])
    sum = 0
    for all count c in counts[c1;c2;...] do
      sum = sum + c
    Emit(term t; count sum)
```

There is a way to partition data.

#### Hadoop vs Google

| | Google | Hadoop |
|--------|----|----|
|reducers receive keys in | sorted order | arbitrarily ordered|
|reducers can change output key| not allowed | can emit arbitrary number of out key-value pairs|

### Restrictions

* be careful about using external resource ( multiple mappers/reducers may
  be contending for those resources )

### Side effects

* It can have side-effects such as writing files to distributed file system

### Special cases

1. MapReduce can contain no reducers
1. MapReduce is not possible without mapper
1. Identity function
1. Running Id


### Map and fold

if array has 0 AND 9 OR has 5
Value is X
Else Y

f(x) = if x in [5] return 3
      else if x == 0 return 1
      else if x == 9 return 2
      else return 0

g(x,y) =
  return (x|y)
initial value = 0


if ( result == 3 ) // FANCY
else // NOT-FANCY


## Execution framework

MapReduce separate the what of distributed processing from the how

MapReduce program refers to a *job*

JobTracker = submission node

Execution framework = runtime

Responsibilities:
    * divides into smaller unit called tasks (task queue)
    * data/code co-location
    * synchronization
        copying intermediate data over the network (with the process) is called
        "Shuffle and sort"
    * Error and fault handling


## Partitioners and combiners

* partitioners
    * **resp** for dividing up the intermediate key space and assign
  intermediate key-value pairs to reducers.
    * specifies the task to w/c an intermediate key-value pair must be copied
    * simplest partitioner involves computing the hash value of the key and
      then taking the mod of that value with the number of reducers.


* combiners
    * optimization in MapReduce that allow local aggregation before the shuffle
      and sort phase
    * "mini-reducers" that take place out of the mapper, before shuffle and
      sort
    * "nothing but a reducer"
    * framework does not guarantee running the combiner


* DFS

Stores 3 separate copies of data (by default) to ensure :
    * reliability
    * availability
    * performance

- **NameNode**
    * clients contact namenode to find where data is stored
    * returns relevant block id
- **DataNode**
    * client contacts the datanode to retrieve data
    * returns data


HDFS namenode responsibilities

* namespace management
* coordinating file operations
* maintaining overall health of the file system
    * rebalancing


HDFS (or GFS) environment

* Stores a relatively modest number of *large* files
* workload are batch-oriented
    * sustained bandwidth is more important than low latency
* File system in deployed with env. of cooperative users
    * no security discussion
* System is built on unreliable and (maybe) inexpensive components
* weakness: master goes offline, entire filesystem and mapreduce halts


## Hadoop Cluster Architecture

### namenode

* job submission node runs **jobtracker**
    * jobtracker
        * single point of contact for client wishing to execute a MapReduce
        * monitors the progress of running MapReduce jobs
        * responsible for coordinating execution of mappers / reducers
* bulk of hadoop cluster consists of slave node that runs **tasktracker**
    * tasktracker
        * actually running the user code
        * datanode daemon, serving HDFS data



## Multiple files



* Process

1. Load file
1. Map :: k:v -> [k2:v2]
1. Shuffle - sort
1. Reduce :: k2:[v2] -> [k3:v3]








