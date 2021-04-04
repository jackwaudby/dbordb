# Database of Research Databases

Taking motivation from the excellent [Database of Databases](https://dbdb.io/) the goal here is to catalog **research protoype database systems**, highlighting their "novel" features and design principles.
Initially there will be a focus on distributed databases. 

| Name        | Concurrency Control | Replication    | 2PC | Type     | Clocks     |
| ----------- | :------------------:|:--------------:|-----|----------|----------- |
| Sinfonia    | OCC                 | Primary-Backup |  ✅  | Layered  | |
| Percolator  | OCC                 | Primary-Backup |    | Layered  |  |
| Spanner     | 2PL + wound/wait    | Multi-Paxos    |    | Layered  | Atomic |
| [CLOCC](https://github.com/jackwaudby/dbordb/blob/main/clocc.md) | OCC|VSR|  |Layered  |  |
| Granola |  | VSR |✅ |Layered  |  |
| [Calvin](https://github.com/jackwaudby/dbordb/blob/main/calvin.md) | 2PL | Paxos | ❌ |Layered/Deterministic|  |
| Salt |Mixed|||Layered||
| Callas | Mixed|||Layered||
| Replicated Commit ||Paxos||Layered||
| Rococo | Graph|Paxos||Layered||
| MDCC ||||Unified||
| TAPIR ||||Unified||
| [Janus](https://github.com/jackwaudby/dbordb/blob/main/janus.md) ||||Unified||
| PNUTS ||||Eventual||
| Dynamo ||||Eventual||
| TAO |||| Eventual||
| COPS ||||Causal||
| Eiger ||||Causal||
| Walter  ||||||
| Lynx ||||||
| SLOG ||||||
| OceanVista ||||Unified||

Concurreny Control: 
+ OCC
+ 2PL
+ TO
+ Graph
+ Mixed

Replication:
+ VSR: Viewstamped Replication
+ Paxos
+ Raft

Type:
+ Layered: roll together protocols, e.g., 2PL + 2PL + Paxos. 
+ Unified: combine concurrency control, replication, and commitment protocols.
+ Eventual: catch-all for systems that do not offer convential transactions.
+ Causal

Other dimensions:
+ workload contention
+ data model
+ transaction types, e.g., one-shot
+ partial vs full replication
+ transactional isolation level/replicated data consistency 

 


