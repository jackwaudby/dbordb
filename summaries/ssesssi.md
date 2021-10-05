# Lazy Database Replication with Snapshot Isolation [Daudjee and Salem, VLDB, 2006] #

## Motivation/Idea ##

This paper considers *Snapshot Isolation (SI)* in the context of a fully replicated database.
The main idea is to use one site (primary) to determine the necessary ordering of transactions and apply this ordering asynchronously at secondaries.

## System Model ##

1 primary site and 1+ secondary site(s), with data fully replicated at each site.
Clients connect to one of the secondary sites.
Read-only transactions are executed only at secondaries.
Update transactions are forwarded to primaries and executed there.
Lazily replication is achieved via refresh transactions which are sent from the primary and executed in the same order on the secondaries.
It is assumed propagated messages between sites are not lost or re-ordered during transmission.

## Transaction Model ##

Assumes transactions are marked as update or read-only transactions in request streams.
For Strong Session SI (see below) it is assumed each transaction has associated with it a session label.

## Workload ##

Simulated the algorithm using CSIM and parameters are extracted from TPC-W:
* Read-only/update mixes of 80/20 and 95/5.
* Fixed the abort probability at 1%.
* Transaction size between 5-15 items.
* Network delay between sites of 10s.

## Isolation and Consistency Guarantees ##

SI is appealing as readers never block writers and vice-a-versa.

This paper discusses several variants of SI:
* **Strong SI**/C-SI: enforces total order across all transactions, reading from the latest committed state.
* **Strong Session SI**/PC-SI: enforces total order with the scope of a session or workflow.
* **Weak SI**/G-SI/ANSI-SI: does not prevent "transaction inversions", i.e., there are no real-time guarantees, which is possible in a lazily replicated database.

## Sketch Algorithm ##

The paper describes 3 algorithms:
1. `Alg-Weak-SI`: forwards all update transactions to the primary and executes read-only transactions at secondaries (never blocking transactions, but providing stale reads).
2. `Alg-Strong-Session-SI`: uses timestamps corresponding the commit order of transactions at the primary to control the order at which read-only transactions are executed at the secondaries. Primary stores `seq(c)` counter for each session `c`. Secondaries store `seq(DB_sec)` counter, i.e., the state of secondary in terms of the primary.
```
at primary, Tu from session c commits, set seq(c) to commit_p(T)

at secondary, refresh of Tu commits, set seq(DB_sec) to commit_p(T)

if T_ro starts, wait if seq(c) > seq(DB_sec)

```
3. `Alg-Strong-SI`: a single session for the system, i.e., a single `seq(c)`.

## Results ##

+ Under low load `Alg-Weak-SI` and `Alg-Strong-Session-SI` perform roughly the same.
+ Under mid/high loads the latency of  `Alg-Strong-Session-SI` increases.
+ `Alg-Strong-SI` did not scale as well as the others.
+ Greater scalability across algorithms is achievable if the workload has a higher percentage of reads.

## Limitations ##

+ No implementation/empirical evaluation.
+ Unclear what happens if the primary site fails, a somewhat "old school" approach to replication and fault-tolerance.
+ Primary site is a clear bottleneck for transaction processing.
+ Simulation parameters outdated.

## Links ##
- [Paper](http://www.vldb.org/conf/2006/p715-daudjee.pdf)
