# Consolidating Concurrency Control and Consensus for Commits under Conflicts [Mu et al., OSDI, 2016]

## Idea
Reduce cross-data-center coordination by combining concurrency control and replication protocols to provide geo-distributed transactions.
They observe achieving strict serializability for transaction consistency and linearizability for replication consistency can be mapped to the same abstraction - constructing and maintaining an acyclic serialization graph.
Designed for high contention workloads, deterministically reordering transactions to avoid aborts.
Best-case: commit in 1 cross-data-center RT, worst-case: 2 RTs.

## Algorithm ##
### (1) Pre-accept

*Coordinator:*
- Send pieces of `T` to all replicas within their shard
- If all replicas within each shard reply with the same dependencies skip **Accept**

*Server:*
- Insert `T` into local dependency graph `(T.state = pre-accepted)`
- Calculate direct dependencies (do not execute piece) and send to coordinator

### (2) Accept (optional)

Reaches consensus on within-shard dependencies using a ballot accept/reject mechanism

### (3) Commit

*Coordinator:*
- Aggregate dependencies of pieces and send to servers
- When receives 1 response from each shard responsed to client

*Server:*
- Update local dependency graph with global dependencies `(T.state = committing)`
- Execute when all ancestor state is `committing`
  - If server does not particpate in a dependency `T' -> T` send inquire message to nearest server that does
  - Receive direct dependencies of `T'` when it has become committing on `S'`
- Compute strongly connected components and execute in order sorted by transaction ids
- Process `T`. `(T.state = processed)`
- Return result to coordinator

## Isolation Level
Strict serializabilty.

## Transaction Model
One-shot transactions written as stored procedures.
One-shot transactions preclude the execution output of one piece being used as input for a piece that executes on a different server.

## Links
- [Paper](https://www.usenix.org/system/files/conference/osdi16/osdi16-mu.pdf)
- [Blog post](http://muratbuffalo.blogspot.com/2020/11/consolidating-concurrency-control-and.html)
