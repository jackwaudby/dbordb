# Database of Research Databases

Taking motivation from the excellent [Database of Databases](https://dbdb.io/) the goal here is to catalog **research protoype database systems**, highlighting their "novel" features and design principles.
Initially there will be a focus on distributed databases.

| Name                                                                                        | CC                   | Replication      | 2PC   | Type       |   |
| -----------                                                                                 | :------------------: | :--------------: | ----- | ---------- |   |
| [Sinfonia](https://github.com/jackwaudby/dbordb/blob/main/summaries/sinfonia.md)            | OCC                  | Primary-Backup   | ✅    | L          | ] |
| [Percolator](https://github.com/jackwaudby/dbordb/blob/main/summaries/percolator.md)        | OCC                  | Primary-Backup   |       | L          |   |
| [Spanner](https://github.com/jackwaudby/dbordb/blob/main/summaries/spanner.md)              | 2PL (wound/wait)     | Multi-Paxos      |       | L          |   |
| [CLOCC](https://github.com/jackwaudby/dbordb/blob/main/summaries/clocc.md)                  | OCC                  | VSR              |       | L          |   |
| [Granola](https://github.com/jackwaudby/dbordb/blob/main/summaries/granola.md)              |                      | VSR              | ✅    | L          |   |
| [Calvin](https://github.com/jackwaudby/dbordb/blob/main/summaries/calvin.md)                | 2PL                  | Paxos            | ❌    | L/D        |   |
| [Salt](https://github.com/jackwaudby/dbordb/blob/main/summaries/salt.md)                    | Mixed                |                  |       | L          |   |
| [Callas](https://github.com/jackwaudby/dbordb/blob/main/summaries/callas.md)                | Mixed                |                  |       | L          |   |
| [Replicated Commit](https://github.com/jackwaudby/dbordb/blob/main/summaries/rep_commit.md) |                      | Paxos            |       | L          |   |
| [Rococo](https://github.com/jackwaudby/dbordb/blob/main/summaries/rococo.md)                | Graph                | Paxos            |       | L          |   |
| [MDCC](https://github.com/jackwaudby/dbordb/blob/main/summaries/mdcc.md)                    |                      |                  |       | U          |   |
| [TAPIR](https://github.com/jackwaudby/dbordb/blob/main/summaries/tapir.md)                  |                      |                  |       | U          |   |
| [Janus](https://github.com/jackwaudby/dbordb/blob/main/summaries/janus.md)                  |                      |                  |       | U          |   |
| [PNUTS](https://github.com/jackwaudby/dbordb/blob/main/summaries/pnuts.md)                  |                      |                  |       | E          |   |
| [Dynamo](https://github.com/jackwaudby/dbordb/blob/main/summaries/dynamo.md)                |                      |                  |       | E          |   |
| [TAO](https://github.com/jackwaudby/dbordb/blob/main/summaries/tao.md)                      |                      |                  |       | E          |   |
| [COPS](https://github.com/jackwaudby/dbordb/blob/main/summaries/cops.md)                    |                      |                  |       | C          |   |
| [Eiger](https://github.com/jackwaudby/dbordb/blob/main/summaries/eiger.md)                  |                      |                  |       | C          |   |
| [Walter](https://github.com/jackwaudby/dbordb/blob/main/summaries/walter.md)                |                      |                  |       |            |   |
| [Lynx](https://github.com/jackwaudby/dbordb/blob/main/summaries/lynx.md)                    |                      |                  |       |            |   |
| [SLOG](https://github.com/jackwaudby/dbordb/blob/main/summaries/slog.md)                    |                      |                  |       |            |   |
| [OceanVista](https://github.com/jackwaudby/dbordb/blob/main/summaries/oceanvista.md)        |                      |                  |       | U          |   |
| Tango                                                                                       |                      |                  |       |            |   |
| Corfu                                                                                       |                      |                  |       |            |   |

Concurrency Control (CC):
+ **OCC**: *optimistic concurrency control*.
+ **2PL**: *two-phase locking*.
+ **TO**: *timestamp ordering*.
+ **G**: *graph-based*.
+ **M**: *mixed*, combines multiple protocols.

Replication:
+ Viewstamped Replication (VSR)
+ Paxos
+ Raft

Type:
+ **L**: *layered*, protocols are combined, e.g., 2PL + 2PC + Paxos.
+ **U**: *unified*, protocols are combined into one.
+ **E**: *eventual*: evenutal consistency.
+ **C**: *causal*: causal consistency.
+ **D**: *deterministic*: deterministic.


Other dimensions:
+ workload contention
+ data model
+ transaction types, e.g., one-shot
+ partial vs full replication
+ transactional isolation level/replicated data consistency
