# Scalable Atomic Visibility with RAMP Transactions #

## Motivation ##
Distributed databases split their data across multiple servers, a.k.a, *partitions*.
This provides great scalability for operations that access a single partition, but for multi-partition operations some communication/coordination between partitions is required.
There exists a class of applications that require multi-partition atomically visible transactional access: all or none of a transaction’s effect should be visible.
These are:
+ Foreign key constraints; a user performing a “like” action on a Facebook page produces updates to both the LIKES and LIKED_BY associations.
+ Secondary indexing; lookups by some secondary attribute.
+ Materialized view maintenance; some precomputed query over the data.
Prior to RAMP approaches were either: (i) fast, but inconsistent, offering no transactional semantics, or (ii) slow, unavailable in the presence of network partitions, and coordination intensive.
In short, there was a perceived dichotomy between scalability and atomic visibility and this was “a fact of life in the big cruel world of huge systems” [Helland 2007].

## Contribution ##
This work refutes the notion that atomically visible transactions are  adds with scalability.
They introduce a new isolation level, *Read Atomic*, and describe 3 read atomic multi-partition (RAMP) algorithms.
In doing so they identify 2 key scalability constraints;
1. *Coordination-free execution*; one client’s transactions cannot cause another client’s transactions to stall or fail.
2. *Partition independence*; clients only contact partitions that their transactions directly reference.

## Algorithms ##
In short they allow reads to "race" writes, autonomously detecting the presence of non-atomic (partial) reads and, if necessary, repairing them via a second round of communication with servers.
There are 3 algorithms;
1. RAMP-Small: require constant space (a timestamp per write) and require 2 RTTs for reads.
2. RAMP-Fast: require metadata linear with no. writes but in the best case only require 1 RTT for reads.
3. RAMP-Hybird: intermediate solution that uses a Bloom filter.
Each algorithm allows 2PC to run in parallel over the data - a big win.

### RAMP-Fast ###

Write transactions;
+ T assigned a unique timestamp TS
+ Assign TS to each write in T
+ PREPARE; add writes to local database
+ COMMIT;
  + Mark versions as committed
  + Update *lastCommit*; highest timestamp committed version

Read transactions;
+ GET; highest committed version for each item
+ Calculate missing versions
+ GET_MISSING; grab missing versions


## Evaluation ##
They compared 5 algorithms with RAMP; no write or read locks, repeatable read, read committed, read uncommitted, Eiger’s 2PC-PCI.
They ran experiments measuring throughput, latency, and scalabililty using YCSB with 95/5 read/write split.
For throughput and latency performed close to no write or read locks.
Scaled linearly to over 7 millions operations per second.

## Limitations ##
Read atomic isolation does not prevent concurrent updates, offer recency guarantees, or enforce transitive read dependencies.

## Links ##
* [Paper](https://dl.acm.org/doi/10.1145/2909870)
* [Morning Paper](https://blog.acolyer.org/2015/03/27/scalable-atomic-visibility-with-ramp-transactions/)
* [Slides](https://docs.google.com/presentation/d/1NjN-bzDFmVpLzOFYht-Rg69pCbJG7Emmc8Jgb_3HcCU/edit#slide=id.ge6489d0440_0_14)
* [Blog post](http://www.bailis.org/blog/scalable-atomic-visibility-with-ramp-transactions/)
