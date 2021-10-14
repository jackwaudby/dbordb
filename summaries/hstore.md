# The End of an Architectural Era (It's Time for a Complete Rewrite) [Stonebraker et al., VLDB, 2007] #

## Motivation/Idea ##

Popular relational DBMSs, Oracle, DB2, SQL Server, share a common architectural root: System R (1970s), disk-oriented storage/indexing, multithreading, lock-based concurrency control, and log-based recovery.
Each system was designed 25 years ago (nearly 40 years ago at the time of writing this summary) when hardware characteristics were significantly different - nowadays CPU are 1000x faster, RAM is 1000x larger, and DISK .
Moreover, interactions patterns with DBMS have changed - interactive transactions are rare and users seldom presented with a SQL interface.
Given the above (and DBMSs poor performance on business data processing workloads) this paper argues OLTP systems are in need of a redesign (H-Store).

## System Model ##

* **Main memory:** any database less than 1TB is deployable completely in main memory on a shared nothing cluster.
* **Single-threaded execution:** no need for complex multi-threaded data structures and resource governors.
* **Grid-computing:** horizontally partition over the nodes of a grid/cluster.
* **High-availability:** tables are replicated at least 2x. Transactions can read from any replica, but must update/execute everywhere. (Read-only tables replicated at every site.

In H-Store a physical site is divided into a logical site per-core with a dedicated resources.

## Transaction Model ##

Observe that OLTP transactions are lightweight:
* Heaviest TPC-C transactions touches 400 records.
* Useful work takes less than 1ms.
* No user stalls.
* If main memory then no disk stalls.
* Long running transactions are split into multiple shorter transactions.
* Ad-hoc queries are executed on a different system.

Therefore, transactions execute as pre-declared stored procedures, no ad-hoc transactions nor are user-stalls allowed.

* **One-shot transactions:** decompose into a collection of single-site plans. **TODO:** *are these equal to independent transactions?*
* **Two-phase transactions:** sequence of read operations (phase 1) followed by a consistency check to determine if the transaction needs to abort, before write operations are executed (phase 2).
* **Strongly two-phase transactions:** two-phase and phase 1 operations produce the same result at all sites.
* **Sterile:** two transactions commute if any interleaving of their single-site sub-plans produces the same final database site as any other interleaving. A transaction class (group of transactions) is sterile if it commutes with all transaction classes (including itself).

## Workload ##

Requires the application to be written as a tree schema.
Data partitioning is determined based on the root table.
All equi-joins are co-located within the same site.
Such an application is called a *constrained tree application (CTA)*.


## Isolation and Consistency Guarantees ##

Serializability.

## Sketch Algorithm ##

Changes depending on the transaction model:
* Read operations are sent to any replica.
* Write operations are sent to all replicas.

Transactions are assigned timestamps on entry, `(site_id,local_ts)`; sufficient to totally order transactions.
It is assumed clocks are synchronized across sites using NTP.
Note, the 'delay' below is used to ensure transactions execute in the correct order on each replica.

```
(i) if all transaction class are single-site/one-shot transactions

coordinator dispatches sub-plans to sites and associated replicas
workers delay then execute

(ii) single-site/one-shot/two-phase

same as (i)

(iii) sterile

no need to assign timestamps
if two-phase
    collect responses (ok/abort) at end of phase 1 and do 2PC
if strongly two-phase
    no 2PC needed

(iv) other (basic)

determine potentially conflicting transaction classes
coordinator dispatches sub-plans to sites and associated replicas
workers delay then:
    if no uncommitted potentially conflicting transaction at this site with lower timetstamp then execute
    else abort

if coordinator receives all 'ok' then commit
else abort

(v) other (intermediate)

before executing/aborting a sub-plan:
    stalls numberOfMessagesInTxnClass * averageRTTDelay to see if earlier plan appears then:
        sequence sub-plans to avoid aborting

(vi) other (advanced)

track read/write set of transactions at each site:
    abort due to OCC rules
```

## Results ##

* Transform TPC-C into a one-shot, sterile, and strongly two-phase workload.
* 70K TPC-C transactions/sec on a 2 CPU/4GB RAM/250GB disk machine.

## Limitations ##

- Complete workload (statements, program logic, and schema) must be specified in advance.
- The delay is used to ensure transactions execute in the correct order on each replica feels like it could be problematic in practice.
- Unclear how to automate the process of turning a workload into a one-shot or sterile workload.

## Links ##
- [H-Store Publication List](https://hstore.cs.brown.edu/publications/)
