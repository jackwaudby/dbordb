# Database of Research Databases

Taking motivation from the excellent [Database of Databases](https://dbdb.io/) the goal here is to catalog **research protoype database systems**, highlighting their "novel" features and design principles.
Initially there will be a focus on distributed databases.

| Name                                                               | Concurrency Control  | Replication      | 2PC   | Type                  |
| -----------                                                        | :------------------: | :--------------: | ----- | ----------            |
| [Sinfonia](https://github.com/jackwaudby/dbordb/blob/main/summaries/sinfonia.md)                                                          | OCC                  | Primary-Backup   | ✅    | Layered               |]
| [Percolator](https://github.com/jackwaudby/dbordb/blob/main/summaries/percolator.md)                                                         | OCC                  | Primary-Backup   |       | Layered               |
| [Spanner](https://github.com/jackwaudby/dbordb/blob/main/summaries/spanner.md)                                                           | 2PL + wound/wait     | Multi-Paxos      |       | Layered               |
| [CLOCC](https://github.com/jackwaudby/dbordb/blob/main/summaries/clocc.md)   | OCC                  | VSR              |       | Layered               |
| [Granola](https://github.com/jackwaudby/dbordb/blob/main/summaries/granola.md)                                                       |                      | VSR              | ✅    | Layered               |
| [Calvin](https://github.com/jackwaudby/dbordb/blob/main/summaries/calvin.md) | 2PL                  | Paxos            | ❌    | Layered/Deterministic |
| [Salt](https://github.com/jackwaudby/dbordb/blob/main/summaries/salt.md)                                                             | Mixed                |                  |       | Layered               |
| [Callas](https://github.com/jackwaudby/dbordb/blob/main/summaries/callas.md)                                                               | Mixed                |                  |       | Layered               |
| [Replicated Commit](https://github.com/jackwaudby/dbordb/blob/main/summaries/rep_commit.md)                                                    |                      | Paxos            |       | Layered               |
| [Rococo](https://github.com/jackwaudby/dbordb/blob/main/summaries/rococo.md)                                                               | Graph                | Paxos            |       | Layered               |
| [MDCC](https://github.com/jackwaudby/dbordb/blob/main/summaries/mdcc.md)                                                                 |                      |                  |       | Unified               |
| [TAPIR](https://github.com/jackwaudby/dbordb/blob/main/summaries/tapir.md)                                                                |                      |                  |       | Unified               |
| [Janus](https://github.com/jackwaudby/dbordb/blob/main/summaries/janus.md)   |                      |                  |       | Unified               |
| [PNUTS](https://github.com/jackwaudby/dbordb/blob/main/summaries/pnuts.md)                                                              |                      |                  |       | Eventual              |
| [Dynamo](https://github.com/jackwaudby/dbordb/blob/main/summaries/dynamo.md)                                                              |                      |                  |       | Eventual              |
| [TAO](https://github.com/jackwaudby/dbordb/blob/main/summaries/tao.md)                                                                 |                      |                  |       | Eventual              |
| [COPS](https://github.com/jackwaudby/dbordb/blob/main/summaries/cops.md)                                                               |                      |                  |       | Causal                |
| [Eiger](https://github.com/jackwaudby/dbordb/blob/main/summaries/eiger.md)                                                               |                      |                  |       | Causal                |
| [Walter](https://github.com/jackwaudby/dbordb/blob/main/summaries/walter.md)                                                              |                      |                  |       |                       |
| [Lynx](https://github.com/jackwaudby/dbordb/blob/main/summaries/lynx.md)                                                                |                      |                  |       |                       |
| [SLOG](https://github.com/jackwaudby/dbordb/blob/main/summaries/slog.md)                                                                |                      |                  |       |                       |
| [OceanVista](https://github.com/jackwaudby/dbordb/blob/main/summaries/oceanvista.md)                                                          |                      |                  |       | Unified               |
| Tango |||||
| Corfu |||||

Concurrency Control:
+ OCC
+ 2PL
+ TO
+ Graph
+ Mixed

Replication:
+ Viewstamped Replication (VSR)
+ Paxos
+ Raft

Type:
+ Layered: roll together protocols, e.g., 2PL + 2PL + Paxos.
+ Unified: combine concurrency control, replication, and commitment protocols into a single protocol.
+ Eventual: evenutal consistency.
+ Causal: causal consistency.

Other dimensions:
+ workload contention
+ data model
+ transaction types, e.g., one-shot
+ partial vs full replication
+ transactional isolation level/replicated data consistency
