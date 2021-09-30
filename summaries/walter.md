# Transactional storage for geo-replicated systems [Sovran et al., SOSP, 2011] #

## Motivation ##

In 2011, state-of-art systems offered an uncomfortable choice for applications requiring geo-replication: providing no or little transactional semantics and requiring application-level conflict resolution logic in the event of conflicting concurrent writes.
Walter aims to provide strong semantics within a site, but weaker semantics across sites allowing asynchronous (lazy) replication - specifically, it provides *parallel snapshot isolation (PSI)*.

## System Architecture ##

Walter is a multiversioned transactional key-store replicated across several geographic sites.
Replication between sites is asynchronous owing to: (i) *preferred sites* and (ii) *c-sets*, techniques that prevent write conflicts between concurrent transactions.
Key-value pairs (objects) are grouped into containers (unit of replication) which share the same preferred site.

## Transaction Model ##

Supports read/write transactions but depending on the composition of the write set either the fast or slow commit paths will be taken.
A transaction that executes on items that have the local site as their preferred site or are c-sets can commit without cross-site communication.

## Workload ##

Implemented 3 workloads with a replication factor of 4 (max RTT was 270ms):
1. Microbenchmark testing the fast and slow commit paths.
2. WaltSocial.
3. ReTwis.

Note, transactions were small, touching between 1-5 objects.

## Isolation and Consistency Guarantees ##

Snapshot isolation (SI) requires:
* (i) transactions read from a snapshot of most-recently committed transactions.
* (ii) if concurrent transactions write the same item, the first-committer wins.

Problems:
* (i) forces transactions that don't conflict into an (unneeded) total order, which is inefficient in a replicated context.
* (ii) ensuring a snapshot of most-recently committed transactions requires synchronous replication/communication with the scope of transaction, which again is inefficient in a replicated context.

PSI requires:
* (i) transactions read from a snapshot of most-recently committed transactions **within a site**.
* (ii) if concurrent transactions (**potentially executing on different sites**) write the same item, the first-committer wins.
* (iii) if `Ti` commits before `Tj` at site `A` then this should hold across all sites.

PSI allows a *long fork* anomaly in which views at sites can diverge whilst committed transactions are asynchronously propagating.

## Sketch Algorithm ##

Site-level data structures:
* `CurrSeqNum_i`: last sequence number assigned by site `i`.
* `CommittedVTS_i[j]` a vector containing the sequence number from the last transaction from site `j` committed at site `i`.
* Each object `oid` has a sequence of writes tagged with the writing transaction's `version`.

Transaction-level data structures:
* `StartVTS` or vector timestamp: set equal to  `CommittedVTS_i[j]` when `T` starts executing, indicates the snapshot `T` will read.
* `CommitTS` or `version`: a tuple `<site_id,seq_num>` indicating the site `T` executed at and the sequence number it was assigned (this totally orders transactions within a site).

Visibility rule:
```
Given version (v) and StartVTS; v is visible if v.seq_num <= StartVTS[v.site_id]
```

Algorithm:
```
for T executing at site i:
(i) get StartVTS based on CommittedVTS_i[j]
(ii) write to temp buffer, T.updates
(iii) read most recent version based on visibility rule

fast commit:
(iv) if WriteSet(T) contains objects that have site i as preferred site then for each object written
    - check if all versions available are visible (no updates)
    - check if any are locked (slow commit)

slow commit:
(iv) if WriteSet(T) contains objects that are not preferred at site i
    - ask all sites to vote: vote yes if object is unmodified and unlocked
    - if all vote yes proceed else abort and release locks

(v) get new seq_num
(vi) mark as committed: async propagation

async propagation:
site i copies objects to all replicas
    replica j responds when:
    1. received T
    2. received all transactions that causally preceed T based on T.StartVTS
    3. received all transactions from site i with seq_num < T.seq_num
when f+1 replicas respond T is marked as disaster safe
    replica j commits T when:
    1. learns T is marked as disaster safe
    2. all transactions that causally preceed T are marked as committed
    3. all transactions that preceed T on site i are committed
```

Comments:
* **Clocks:** timestamps are logical, no real-time guarantees.
* **Concurrency Control:** multiversion snapshot based concurrency control.
* **Replication:** lazy replication with restricted update anywhere.
* **Fault-tolerance:** supports (i) normal, writes are replicated across local cluster storage, and (ii) disaster-safe, transaction is logged at `f+1` sites.
* **Atomic commitment:** uses 2PC in slow commit path.

## Limitations ##
+ Not evaluated on a conventional transaction processing workload, e.g., TPC-C.
+ Transactions cannot span multiple partitions.
+ Evaluation did not consider mixed workload that containing transactions using the slow and fast commit paths.
+ Designing applications to use preferred sites and c-sets could be non-trivial.

## Links ##
- (Paper)[http://www.news.cs.nyu.edu/~jinyang/pub/walter-sosp11.pdf]
