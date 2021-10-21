⚠ **WORK IN PROGRESS** ⚠️

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
* **Sequencing layer:** receives transaction inputs, puts them in a global input sequence in every [epoch](#clocks) which it replicates and durably logs via the [replication strategy](#replication).
* **Scheduling layer:** [deterministic locking](#concurrency-control) to ensure execution order equals global input sequence.
* **Storage layer**

The unbundled design means no assumptions about physical data layout can be made.
This means logging and concurrency control must be logical.
Logging is straightforward as any replica can rebuild its state from the input logged by the sequencer; checkpointing can be done to improve performance.
Concurrency control is trickier and requires a deterministic locking protocol.

## Transaction Model ##

* **Pre-declared Transactions:** must declare their complete item access set up front, read set and write set.
* **Dependent Transactions:** must perform some reads to determine their full read/write set. Uses Optimistic Lock Location Prediction (OLLP) to support these transactions. Run a recon query to work out the full read and write set, then add to global input sequence, check at runtime is the recon query result is still valid.

## Workload ##

## Isolation and Consistency Guarantees ##

## Sketch Algorithm ##

For an epoch, the sequencer at a node:
* Gathers transaction input requests into a global input sequence (batch).
* Replicates the global input sequence.
* After replication sends: (i) sequencer's id, (ii) epoch no., and (iii) all transaction requests to schedulers of each partition within its replica.
* Schedulers then construct their view of the global input sequence.

### Clocks ###

Time is broken in epochs in which transaction requests get gathered.
The epoch at each node is synchronously incremented every 10ms.

### Concurrency Control ###

Deterministic Strict 2PL:
* Acquire all locks for a request
* Hand to worker thread:
    * Perform read/write set analysis. Identify local elements and the set of participant nodes in the transaction (read set = passive participant, write set = active participant)
    * Perform local reads
    * Forward local reads to all workers on active participant nodes
    * Collect remote read results
    * Apply local writes

Ideally, several of these steps happen in parallel.

### Replication ###

Nodes are organized into replication groups, i.e., all replicas of a partition.
* Asynchronous: 1 replica is the master and all requests are forwarded here; complex recovery if master fails.
* Synchronous (Paxos-based).

### Atomic Commitment ###

## Results ##

## Limitations ##

## Links ##
+ [Paper](http://cs.yale.edu/homes/thomson/publications/calvin-sigmod12.pdf)
+ [TODS Extended Paper](http://www.cs.umd.edu/~abadi/papers/calvin-tods14.pdf)
+ [Morning Paper](https://blog.acolyer.org/2019/03/29/calvin-fast-distributed-transactions-for-partitioned-database-systems/)
+ [The Case for Determinism in Database Systems (2010)](https://www.cs.umd.edu/~abadi/papers/determinism-vldb10.pdf)
+ [An Evaluation of the Advantages and Disadvantages of Deterministic Database Systems (2014)](http://www.vldb.org/pvldb/vol7/p821-ren.pdf)
