# RAMP-TAO: Layering Atomic Transactions on Facebook's Online TAO Data Store [Cheng et al., VLDB 2021] #

## Motivation ##

Moved from simple API to higher-level framework and developers began desiring transactional semantics.
Initially, *failure-atomic* write transactions were added.
Then, desired to add "opt-in" *atomically visible* read transactions that are cache-friendly and hot spot friendly to TAO.

Selected RAMP-Fast which provides 1 RT reads but the algorithm could not be directly applied to TAO, as:
1. Provides no distinction between single- and multi-item reads; desire to avoid costs for reads of *non-transactional* writes (update 1 item).
2. There is no consideration for replication of metadata; desire to avoid unneeded replication and storing metatdata for non-transactional writes.
3. Requires full multi-versioning; TAO is single-versioned.

## System Model ##

TAO is a geo-distributed data store that prioritises availability and low-latency read operations.
It is a statically-sharded MySQL database with 2 layers of caches.
Updates are replicated asynchronously via Wormhole providing eventual consistency.
It has two APIs:
+ Simple: point, get, range, count operations and create/delete/update of nodes and edges.
+ Ent: higher level query framework

## Transaction Model ##
Single-item operations are referred to as non-transactional.
Any request involving 2+ items is referred to as transactional.
**Does not offer read-write transactions**, transactions are either read- or write-only:
+ Failure-atomic write transactions/write atomicity ensuring no partial writes. They are 2 options:
  + Single-shard multi-writes; multi-put on a single shard.
  + Cross-shard transactions; each transaction gets unique ID, uses 2PL and 2PC. This leads to concurrent reads observing stale data especially during recovery.
+ Atomically visible read transactions ensuring no fractured reads.

## Workload ##

Facebook's workload is read-intensive, there are 10 billion reads/s to 10 million writes/s.
In a measurement study they find fractured reads occur 0.06% of the time.
Additionally, they find 99.99% of TAO replicas are less than 60 seconds stale.
They investigate query and changesets composition along 4 dimensions: number of operations, number of data items, number of shards, and success rate:
+ Queries: consist of 2.4 operations and involve 2.2 items on average, but the distribution is long tailed (max of 587 operations and 162 items).
+ Changesets: consist of 2.4 operations and involve 2.1 items, they have a failure rate 2.2% and involve 1 to 10 shards.

High frequency of *bidirectional associations*, 52% of association types are bidirectional, and these comprises of 56% of the write volume.
Note, in TAO these edges are sharded by source node.
If there are no edge constraints (uniqueness/existence) reads complete in 1 round trip and a background 2PC fixer process is used to clean up inconsistencies.
Else, if there are constraints the 2PC protocol is used.
Important regards read atomicity a applications read a pair of edges from one side but sometimes complex queries use both (indirectly).

## Sketch Algorithm ##

RAMP-TAO:
1. Does not fetch metadata and check for fractured reads for items updated by non-transactional writes.
2. Does not store metadata in the cache, only in the database and access information is stored at shard-level granularity.
3. partial multi-versioning using the Refill Library (a recent write buffer) which stores 3 minutes of writes from all regions.

```
// read path

if all items updated by non-transactional writes then read from cache and return
else check results from metadata in the cache
    if atomically visible then return
    else
        if item timestamp < oldest item in cache // evicted, safe to return
        else // not yet replicated to cache
            fetch needed versions from database

// write path

assign all items the same timestamp
if transaction write then set transaction bit
```

### Results ###

In a prototype implementation 99.3% of reads complete in 1 round trip and there is only a 0.42% increase in memory usage.

### Optimisations ###
+ Can fall back to stable versions that are still in the cache, but at a cost of some stale reads.
+ Bidirectional edges read from the committed side and do an asychronous read repair.

### Limitations ###
+ Read atomic is weaker than snapshot isolation and serializability, offers no point-in time snapshots.
+ Limited recency guarantees.

## Links
+ [Paper](https://www.vldb.org/pvldb/vol14/p3014-cheng.pdf)
+ [Blog post](https://engineering.fb.com/2021/08/18/core-data/ramp-tao/)
