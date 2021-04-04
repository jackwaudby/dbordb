# Database of Research Databases

Taking motivation from the excellent [Database of Databases](https://dbdb.io/) the goal here is to catalog **research protoype database systems**, highlighting their "novel" features and design principles.
Initially there will be a focus on distributed databases. 

| Name        | Concurrency Control | Replication | 2PC        | Type       | Clocks     |
| ----------- | :-----------:       | ----------- |----------- |----------- |----------- |
| Sinfonia    | OCC                 |    Primary-Backup         | ✅ | Layered| ?|
| Percolator  |
| Spanner ||||||
| [CLOCC](https://github.com/jackwaudby/dbordb/blob/main/clocc.md) ||||||
| Granola ||||||
| [Calvin](https://github.com/jackwaudby/dbordb/blob/main/calvin.md) | 2PL | Paxos | ❌ |Layered/Deterministic| ? |
| Salt ||||||
| Callas ||||||
| Replicated Commit ||||||
| Rococo ||||||
| MDCC ||||||
| TAPIR ||||||
| [Janus](https://github.com/jackwaudby/dbordb/blob/main/janus.md) ||||||
| PNUTS ||||||
| Dynamo ||||||
| TAO ||||||
| COPS ||||||
| Eiger ||||||
| Walter  ||||||
| Lynx ||||||
| SLOG ||||||
| OceanVista ||||||

Type:
+ Layered: roll together protocols, e.g., 2PL + 2PL + Paxos. 
+ Unified: combine concurrency control, replication, and commitment protocols.
+ Eventual: catch-all for systems that do not offer convential transactions.

Other dimensions:
+ specific workload 
+ data model
+ transaction types, e.g., one-shot
 

