# Epoch-based Commit and Replication in Distributed OLTP Databases [Lu et al., VLDB, 2021] #

## Motivation/Idea ##

Distributed, partitioned, shared-nothing OLTP databases scale well if all transactions access 1 partition as no coordination is required between nodes.
Transactions that access 1+ partitions typically use a 2PC to ensure atomicity and durability.
2PC hurts performance because:
* Transactions hold locks until the second phase, which blocks other transactions and reduces concurrency.
* It requires 2 network round trips (RTs) and 2 sequential durable writes **for each** distributed transaction.

Moreover, these systems desired high availability which is achieved via data replication.
To ensure strong replica consistency they use synchronous replication, where transactions commit only when their writes have arrived at all replicas.
Thus, even single partition transactions must wait for at least 1 network RT.

**The problem with 2PC and synchronous replication is they operate at the granularity of 1 transaction.**
COCO groups transactions that arrive within a window into epochs (similar to group-commit to disk in single site databases).
This allows:
* Locks to be released  immediately after executing and not be held for replication.
* Logging to disk occurs asynchronously in the background at epoch-level granularity.

## System Model ##

* Shared-nothing architecture
* Partitioned
* Main-memory

## Transaction Model ##

## Workload Assumptions ##

Designed for workloads consists of mainly short-lived transactions.

## Isolation and Consistency Guarantees ##

Serializability and snapshot isolation.

## Sketch Algorithm ##

### Clocks ###

### Concurrency Control ###

OCC:
* Physical time
* Logical time

### Replication ###

### Atomic Commitment ###

## Evaluation/Results ##

8 server cluster
YSCB
TPCC

## Limitations ##

* Latency is increased by a few milliseconds per transaction - the authors argue this is permissible for common applications.
* A single-failure causes all transactions in an epoch to be rolled back and retried, even those on non-failing nodes.
* A single long-running transaction can prevent a whole epoch of transactions from committing.

## Links ##

+ [Paper](http://pages.cs.wisc.edu/~yxy/pubs/coco.pdf)
