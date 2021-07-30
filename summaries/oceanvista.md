# Ocean Vista: Gossip-Based Visibility Control for Speedy Geo-Distributed Transactions [Fan et al., VLDB, 2019]

## Motivation
Distributed transactions have a high overhead for: 
(i) high contention workloads; low contention workloads within a single data center become high contention workloads due to increased network latency, and 
(ii) globally distributed data.
This overhead arises partly due to synchronous transaction processing, i.e, transaction order is resolved during execution, which requires at least 1 WAN RT and can result in a high degree of aborts. 
Additionally, replication imposes challenges as: (i) a write-all approach; suffers from straglers, (ii) a write-quorum approach; reading from a leader is a bottleneck, or read-quorum is costly for read-dominanted workloads, again at least 1 WAN RT is needed. 


## Idea
This paper combines concurrency control, replication, and commitment into one protocol using the idea of *visibility*. 
A transaction model called *functors* is used which allows a transaction to be fully executed from any server. 
Then by assigning a transaction a *version* (a globally unique timestamp) there exists a total order over transactions. 
Transaction can then be replicated across the system and asynchronously processed when all preceeding transactions are also sufficiently replicated. 
This is acheived through gossiping *version* watermarks and enables a form of batch processing. 
In the best case this results in 1 WAN RT to commit a transaction.

## Algorithm
+ `Vwatermark` indicates transactions are replicated to a majority of replicas within each shard (completed their S-phase). 
+ `Rwatermark` indicates the minimum version (of transactions that operate on that shard) within each shard that are fully replicated.
+ Gossipers aggegrate watermarks and spread across the database. 

### (1) Write-only/S-Phase 
*Coordinator:*
* Transaction `T` is assigned a timestamp/version `ts` and enters in `TSset`; imposes a total order over transactions. 
* Send write placeholders (functors) to all shards.
* When write-quorum for each shard responds remove `ts` from `TSset`; asychronously writes to all shards involved in `T`.

*Server:*
* Receives a functors and writes placeholders to multiversion storage

### (2) Read-only phase/E-Phase
*Coordinator/Server:*
* When `ts < Vwatermark` begin execution-phase
* Read version immediately before `T`
* If key is in another shard then 
  *  If `ts < Rwatermark` read from 1 shard (nearest)
  *  Else, read from quorum

*Coordinator-only:*
* Receives first response pass back to client

(3) Asynchronous writes
*Coordinator/Server:*
* Values asychronously written to all replicas in the write set.

## Isolation Level
Strict Serializability

## Transaction Model 
Functors - transactions are deterministic stored procedures with a known write-set

## Links
- [Paper](http://www.vldb.org/pvldb/vol12/p1471-fan.pdf)
