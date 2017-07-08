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

** NOT NameNode **

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







