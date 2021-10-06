# Clock-SI: Snapshot Isolation for Partitioned Data Stores Using Loosely Synchronized Clocks #

## Motivation/Idea ##

Partitioned databases improve latency and throughput by allowing concurrent access in different partitions and also allows all data to reside in-memory.
To support snapshot isolation across partitions in such systems a centralized timestamp authority (CTA) is used.
This imposes 1 RTT to the CTA for read-only transactions and 2 RTTs for update transactions and is scalability bottleneck.
Clock-SI instead uses loosely synchronized clocks to allow partitions to independently certify transactions and uses 2PC for atomic commitment.
To account for clock skew and interactions with concurrent committing transactions, a transaction is forced to delay.
They can also select a slightly stale timestamp to avoid delaying but at a higher chance of aborting due to a WW conflict with a concurrent transaction.

## System Model ##

Multi-versioned partitioned key-value store with each server having 1 partition.
Each server has a physical clock, clocks are synchronized using NTP.
It is assumed that clocks always move forward and the maximum clock skew is bounded.
Replication of partitions is beyond the scope of this paper.

## Transaction Model ##

Distinguishes between read-only and update transactions.

## Workload ##

* YCSB, transactions accessing 8 items.
* Twitter-feed benchmark, 90/10 read/write split.

## Isolation and Consistency Guarantees ##

Provides *Snapshot Isolation* with the following properties:
* Transaction reads from a consistent snapshot at the transactions start time; reading the greatest committed version for each item accessed that is smaller than its start time timestamp.
* Update transactions commit in some total order.
* A transaction aborts if it has a WW conflict with a concurrently transaction that has already committed.

In the snapshot isolation family Clock-SI appears to support Strong SI/Conventional SI.
However, it does present a method to achieve Strong Session SI/Prefix Consistent SI.
Additionally, if stale snapshots are used it provides Weak SI/ANSI-SI/Generalizable SI.

Note, due to clock skew two independent transactions (accessing different partitions) may be observed out of order by a side channel with respect to global time.
Put differently the transaction commit order may differ from the real time commit order.

## Sketch Algorithm ##

* Each partition `P_a` has a local physical clock `PC_a`.
* `delta` used to control snapshot age.

```
client connects to P_a (originating partition) to execute T_i

# read protocol

T_i assigned snapshotTS_i = PC_a - delta

for each item:
    if item updated by T_j with commitTS_j < snapshotTS_i then:
        delay read until T_j state moved to committed
    if item is remote at P_b then:
        if snapshotTS_i > PC_b then:
        delay read until PC_b advances

# commit protocol

if T_i is single partition then:
    verify writes (active -> committing)
    assign commitTS_i from PC_a
    write to stable storage (committing -> committed)

if T_i is multi partition then:
    for each partition P_x:
        certify locally
        write prepared to local storage (active -> prepared)
        get preparedTS_i from PC_x
        send to coordinator (originating partition)

    coordinator sets commitTS_i = max(preparedTS)
    send to partitions (prepared -> committed)
```

## Results ##

* LAN: 50% improvement over CTA for single-partition transactions.
* WAN (3 zones): reduces latency by 160ms compared to CTA when partition is not co-lated in the same data center, drops to sub-millis when co-located.
* Choosing a "good" snapshot age reduces delays at a small cost of additional aborts.

## Limitations ##

* Same result can apparently be achieved with [HLC](http://muratbuffalo.blogspot.com/2014/10/clock-si-snapshot-isolation-for.html).
* No comparison with other systems.

## Links ##

* [Paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/samehe-clocksi.srds2013.pdf)
