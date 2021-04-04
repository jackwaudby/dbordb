## Consolidating Concurrency Control and Consensus for Commits under Conflicts 

**Authors:** Mu et al

**Year:** 2016

**Conference:** OSDI

**Paper:** https://www.usenix.org/system/files/conference/osdi16/osdi16-mu.pdf

### Idea
2-in-1 concurrency control and replication protocol. 
Transactions are ordered based on arrival order at shards. 
Designed for high contention workloads - reorders transactions to avoid aborts. 
Assumes one-shot transactions. 

### Concurrency Control
❌

### Replication 
❌

### Links
+ [Morning Paper](https://blog.acolyer.org/2019/03/29/calvin-fast-distributed-transactions-for-partitioned-database-systems/)
