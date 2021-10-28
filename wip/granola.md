# Granola: Low-Overhead Distributed Transaction Coordination [Cowling & Liskov, USENIX ATC, 2012] #

## Motivation/Idea ##

Coordinating distributed transactions in a partitioned database incurs performance costs for locking and undo logging (30-40% of CPU overhead) and additional overhead for retries arising from aborts and blocking due to contention.
Granola, taking motivation from H-Store this paper, attempts to provide powerful primitives whilst avoiding these costs.
Specifically, this is achieved through a restricted transaction model, **independent transactions**; which bare a resemblance to H-Store's one-shot and strongly two-phase transactions.
These transactions don't require locking, undo logging, nor do they contend with other transactions.
Independent transactions have lower-latency consisting of 3 one-way messages and 1 disk-write; 2PC has 4 messages and 2 disk writes.
Suitable use cases are:
* Distributed read-only transactions.
* Updating replicated data.
* Executing transaction that are predicated on replicated data.
* Transactions that have deterministic commit decisions.

## System Model ##

* *Clients*: issue transaction requests and determine which repositories are needed and which transaction class to use.
* *Repositories*: store data and execute transactions. Each has a number of replicas.



## Transaction Model ##

Transactions execute in one-round of communication between clients and the transaction's set of repositories.
Interactive transactions are prohibited, i.e., no multi-statements followed by a commit decision - transactions are a single-operation.
Supports 3 transaction types:
1. Single repository transactions (single-partition transactions, 1PT).
2. Coordinated distributed transactions (multi-partition transactions, MPT).
3. Independent transactions (IT): MPT but each partition arrives at the same transaction decision independently, avoiding the need for 2PC.
Note, communication between repositories occurs only for MPTs.

## Workload ##

## Isolation and Consistency Guarantees ##

## Sketch Algorithm ##

### Clocks/Transaction IDs ###

* TID: client ID and sequence number per client.
* Loosely synchronized clock rates.

### Concurrency Control ###

To execute 1PT and MPT a timestamp-based coordinated mechanism (without locking) is used.
This reduces overheads from aborts, log management, and locking.

TODO: are transactions always executed sequentially within a repo?

### Replication ###

### Atomic Commitment ###

## Results ##

* 3x thpt increase over locking on TPC-C

## Limitations ##

## Links ##
