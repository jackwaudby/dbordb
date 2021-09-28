# Database Replication Using Generalised Snapshot Isolation [Elnikety et al., SRDS 2005]  #

## Motivation/Idea ##
Extends *Snapshot Isolation (SI)* for use in replicated databases.
Permits snapshots to be stale, but keeps the desirable properties of SI: readers never block/abort writers and writers never block/abort readers.
In a replicated setting they argue requiring the **latest** snapshot negates the benefits of SI as transactions may need to block, hence reading from a slightly stale snapshot is ok.
However, reading from stale snapshots increases the chance of write-write conflicts and thus an increase in aborts.

## System Model/Architecture ##
Assumes a multi-versioned system where a snapshot refers to a committed state of the database.
Database goes through a monotonically increasing version history, versions are used to approximate global time. **[Clocks]**
System consists of a set of *sites* each containing a full copy of the data (fully replicated).  **[Replication]**
A transaction executes at a single site, but commitment requires remote communication and upon commit transaction's write sets are applied at other replicas.

They consider 2 architectures:
* Centralized certifier: 1 site is designated as master, transactions may execute remotely but are forwarded to the master for validation.
* Distributed certifier: replicas validate locally and gather agreement from other replicas using an atomic broadcast protocol. **[Atomic Commitment]**
Both certify a transaction using the algorithm below.

## Transaction Model ##

Assumes general read-write transactions though the paper distinguishes between update and read-only transactions.

## Workload ##

Extracted parameters from TPC-W:
* 10K TPS with 15% update
* Replication factor of 8
* RTT between replicas of 200ms
* 400ms old snapshots
* Update 4 items per transaction

## Isolation and Consistency Guarantees ##
Defines 3 snapshot-based isolation levels:
* Conventional .SI = Strong SI: transaction reads the latest snapshot.
* Prefix-Consistent SI = Strong Session SI: transactions reads the latest snapshot on a replica, observing at least all writes committed on that replica. Desirable for consistent "transaction workflows", equivalent to providing a consistent view in the context of a session.
* Generalisable SI = ANSI SI = Weak SI

## Sketch Algorithm ##

```
# begin rule
Ti is assigned SNAPSHOT(Ti) and START(Ti)

# read rule
Ti reads X such that it reads a consistent snapshot at SNAPSHOT(Ti)

# commit/certification rule

if WS(Ti) and WS(Tj) overlap and SNAPSHOT(Ti) < COMMIT(Tj) < COMMIT(Ti) then abort Ti

# CSI vs PC-SI
- CSI; SNAPSHOT(Ti) = START(Ti)
- PC-SI; if Ti and Tj in the same workflow and COMMIT(Ti) < START(Tj) then COMMIT(Ti) < SNAPSHOT(Tj)

```

## Results ##
* Abort rate is a linear function of transaction length and snapshot freshness.
* In a single site deployment, GSI has a higher abort rate than CSI.
* In a replicated deployment, PC-SI has a higher abort rate than CSI, but lower latency of update (50%) and read-only (80%) transactions. However, as RTT increases CSI aborts more than PC-SI and much higher latency. The caveat being observing stale snapshots. Overall, if the workload is read-dominant PC-SI is a better choice.


## Limitations ##
* No implementation, only analytic model.
* Don't consider hotspots, or meaningfully vary the workload.
* Doesn't permit multi-partition transactions.

## Links ##
- [Paper](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.84.4364&rep=rep1&type=pdf)
