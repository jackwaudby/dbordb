# Calvin: Fast Distributed Transactions for Partitioned Database Systems  [Thomson et al., SIGMOD, 2012] #

## Motivation/Idea ##

Circa 2012 the database trend was to move away from ACID transactions as a means to provide scalability.
This is fine for applications that are "embarrassingly partitionable", but results in increased app development complexity for those that aren't.
Another database trend is towards reduced consistency guarantees of replicated data in response to the limitations summarized in the CAP theorem, though this is reversing as network partitions are rare, synchronous replication is plausible.

Calvin provides a transactional layer that handles: (i) scheduling, (ii) replication, and (iii) network communication to provide ACID guarantees.
The main idea is to move the agreement of how to handle transactions outside of transaction boundaries, before deterministically acquiring locks and executing.
This approach avoids the classical method of holding locks and running a distributed commit protocol (2PC) which often exceeds the actual local execution time of a transaction; 2PC optimizations can reduced latency, but don't reduce transaction's contention footprint, which is what harms throughput.

Determinism plays a key role in Calvin, facilitating active replication and simplifying recovery; if a node crashes history can be replayed from the transaction input log or a node can recover from a replica.

## System Model ##

Calvin is a transaction layer above a storage system with a CRUD interface.
Data is partitioned and replicated across nodes.
A node has 3 components:
* **Sequencing layer:** receives transaction inputs, puts them in a global input sequence which it replicates and durably logs via the [replication strategy](#markdown-header-replication).
* **Scheduling layer:** deterministic locking to ensure execution order equals global input sequence.
* **Storage layer**



## Transaction Model ##

## Workload ##

## Isolation and Consistency Guarantees ##

## Sketch Algorithm ##

### Clocks ###

Time is broken in epochs of 10ms in which transaction requests get gathered.

### Concurrency Control ###

Strict 2PL

### Replication ###

Asynchronous

Synchronous (Paxos-based)

### Atomic Commitment ###

## Results ##

## Limitations ##

## Links ##
+ [Paper](http://cs.yale.edu/homes/thomson/publications/calvin-sigmod12.pdf)
+ [TODS Extended Paper](http://www.cs.umd.edu/~abadi/papers/calvin-tods14.pdf)
+ [Morning Paper](https://blog.acolyer.org/2019/03/29/calvin-fast-distributed-transactions-for-partitioned-database-systems/)
+ [The Case for Determinism in Database Systems (2010)](https://www.cs.umd.edu/~abadi/papers/determinism-vldb10.pdf)
+ [An Evaluation of the Advantages and Disadvantages of Deterministic Database Systems (2014)](http://www.vldb.org/pvldb/vol7/p821-ren.pdf)
