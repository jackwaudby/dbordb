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
* Primary-backup replication

## Workload ##

Designed for workloads consists of mainly short-lived transactions.

## Isolation and Consistency Guarantees ##

Serializability and snapshot isolation.

## Sketch Algorithm ##

### Epoch-based atomic commit protocol ###
* A batch of transactions run and commit in an *epoch*.
* Result of each transaction is not released until the end of the epoch.
* All participant nodes agree to commit all transactions in the current epoch.

Two components:
* Centralized epoch coordinator node (ECN): replicated with Paxos/Raft.
* Participant node (PN)

```
------ECN------
send PREPARE to all PN
--------------

------PN------
receive PREPARE
force log PREPARED-WRITE record
- all txnIds in an epoch
- epoch no
send PREPARE-ACK to ECN
--------------

------ECN------
if receive PREPARE-ACK from all PN
force log COMMIT record
- current epoch no **point of no return**
- inc global epoch
send COMMIT to all PN

else at least 1 PN has failed
send ABORT to all PN
--------------

------PN------
if receive COMMIT
- release results to clients
- send COMMIT-ACK to ECN
- begin excecuting next epoch

else receive ABORT
- rollback writes to last committed epoch
- send ABORT-ACK to ECN
--------------

------ECN------
receive COMMIT/ABORT-ACK from all PN
--------------
```

### Replication Protocol ###

Asynchronous primary-backup replication with synchrony enforced at epoch boundaries:

### Concurrency Control Protocol ###

Transaction runs in 2 phases: execution and commit.
The node initiating a transaction is the *coordinator node* (CN), and other nodes are *participant nodes* (PN).

Execution:
* Read records `(value,TID)` into local read set (RS) on CN
* Locate nodes where record exists, if not local then read from nearest remote
* Write records  `(value,TID)` into local per-transaction write set (WS) on CN

Commit:
* Lock all records in WS: CN sends lock request to PNs who apply NO-WAIT deadlock detection and check  TID is the expected value.
* Validate all records in RS and generate TID: send validate request to PNs who abort transaction if records are locked or changed. If succeeds then assign a TID.
* Commit changes to database: values of WS sent to all backups who apply write using thomas write rule based on TID.

OCC:
* Physical time: extension of Silo.
* Logical time: extension of TicToc.

## Evaluation/Results ##

Setup:
* 8 server cluster: 16 CPUs, 64GB RAM
* YSCB: transaction has 8 reads and 2 read/writes, 20% are multi-partition transactions.
* TPC-C: new-order and payment transactions, 10% and 15% are multi-partition transactions respectively. Replication factor of 3. 400K records per partition with 96 partitions.
* Concurrency control: S2PL with NO-WAIT, PT-OCC, LT-OCC.
* Commit/replication: 2PC w/o replication, 2PC (sync), Epoch (async).

Findings:
* Concurrency control protocols achieve 2-4x increase in throughput through epoch-based commit and replication compared to 2PC (Sync).
* Epoch-based commit/replication trades latency for higher throughput, the performance advantage is more significant when durable write latency is larger.
* Epoch-based commit has significant advantages in the setting.
* 20% higher throughput when running under SI.
* An epoch size of 10ms is a good balance between throughput and latency.

## Limitations ##

* Latency is increased by a few milliseconds per transaction - the authors argue this is permissible for common applications.
* A single-failure causes all transactions in an epoch to be rolled back and retried, even those on non-failing nodes.
* A single long-running transaction can prevent a whole epoch of transactions from committing.

## Links ##

+ [Paper](http://pages.cs.wisc.edu/~yxy/pubs/coco.pdf)
+ [Y Lu PhD thesis](https://dspace.mit.edu/bitstream/handle/1721.1/129261/1227521054-MIT.pdf?sequence=1&isAllowed=y)
