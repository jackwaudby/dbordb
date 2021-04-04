# Database of Research Databases

Taking motivation from the excellent [Database of Databases](https://dbdb.io/) the goal here is to catalog **research protoype database systems**, highlighting their "novel" features and design principles.
Initially there will be a focus on distributed databases.

| Name                                                               | Concurrency Control  | Replication      | 2PC   | Type                  |
| -----------                                                        | :------------------: | :--------------: | ----- | ----------            |
| [Sinfonia](https://github.com/jackwaudby/dbordb/blob/main/sinfonia.md)                                                          | OCC                  | Primary-Backup   | ✅    | Layered               |]
| [Percolator](https://github.com/jackwaudby/dbordb/blob/main/percolator.md)                                                         | OCC                  | Primary-Backup   |       | Layered               |
| [Spanner](https://github.com/jackwaudby/dbordb/blob/main/spanner.md)                                                           | 2PL + wound/wait     | Multi-Paxos      |       | Layered               |
| [CLOCC](https://github.com/jackwaudby/dbordb/blob/main/clocc.md)   | OCC                  | VSR              |       | Layered               |
| [Granola](https://github.com/jackwaudby/dbordb/blob/main/granola.md)                                                       |                      | VSR              | ✅    | Layered               |
| [Calvin](https://github.com/jackwaudby/dbordb/blob/main/calvin.md) | 2PL                  | Paxos            | ❌    | Layered/Deterministic |
| [Salt](https://github.com/jackwaudby/dbordb/blob/main/salt.md)                                                             | Mixed                |                  |       | Layered               |
| [Callas](https://github.com/jackwaudby/dbordb/blob/main/callas.md)                                                               | Mixed                |                  |       | Layered               |
| [Replicated Commit](https://github.com/jackwaudby/dbordb/blob/main/rep_commit.md)                                                    |                      | Paxos            |       | Layered               |
| [Rococo](https://github.com/jackwaudby/dbordb/blob/main/rococo.md)                                                               | Graph                | Paxos            |       | Layered               |
| [MDCC](https://github.com/jackwaudby/dbordb/blob/main/mdcc.md)                                                                 |                      |                  |       | Unified               |
| [TAPIR](https://github.com/jackwaudby/dbordb/blob/main/tapir.md)                                                                |                      |                  |       | Unified               |
| [Janus](https://github.com/jackwaudby/dbordb/blob/main/janus.md)   |                      |                  |       | Unified               |
| [PNUTS](https://github.com/jackwaudby/dbordb/blob/main/pnuts.md)                                                              |                      |                  |       | Eventual              |
| [Dynamo](https://github.com/jackwaudby/dbordb/blob/main/dynamo.md)                                                              |                      |                  |       | Eventual              |
| [TAO](https://github.com/jackwaudby/dbordb/blob/main/tao.md)                                                                 |                      |                  |       | Eventual              |
| [COPS](https://github.com/jackwaudby/dbordb/blob/main/cops.md)                                                               |                      |                  |       | Causal                |
| [Eiger](https://github.com/jackwaudby/dbordb/blob/main/eiger.md)                                                               |                      |                  |       | Causal                |
| [Walter](https://github.com/jackwaudby/dbordb/blob/main/walter.md)                                                              |                      |                  |       |                       |
| [Lynx](https://github.com/jackwaudby/dbordb/blob/main/lynx.md)                                                                |                      |                  |       |                       |
| [SLOG](https://github.com/jackwaudby/dbordb/blob/main/slog.md)                                                                |                      |                  |       |                       |
| [OceanVista](https://github.com/jackwaudby/dbordb/blob/main/oceanvista.md)                                                          |                      |                  |       | Unified               |
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
