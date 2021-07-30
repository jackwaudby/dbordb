# Database of Research Databases

Taking motivation from the excellent [Database of Databases](https://dbdb.io/) the goal here is to catalog **research protoype database systems**, highlighting their "novel" features and design principles.
Initially there will be a focus on distributed databases.

| Name | CC | Rep. | 2PC | Type | Iso. | Con. | TM |
| :---:| :-:| :--: | :-: | :--: | :--: | :--: |:--:|
| [Sinfonia](https://github.com/jackwaudby/dbordb/blob/main/summaries/sinfonia.md)            | OCC             | PB   |:white_check_mark:| L ||||
| [Percolator](https://github.com/jackwaudby/dbordb/blob/main/summaries/percolator.md)        | OCC             | PB   || L  ||||       
| [Spanner](https://github.com/jackwaudby/dbordb/blob/main/summaries/spanner.md)              | 2PL (wound/wait)| MPax || L  ||||       
| [CLOCC](https://github.com/jackwaudby/dbordb/blob/main/summaries/clocc.md)                  | OCC             | VSR  || L  ||||      
| [Granola](https://github.com/jackwaudby/dbordb/blob/main/summaries/granola.md)              |                 | VSR  |:white_check_mark:| L |||| 
| [Calvin](https://github.com/jackwaudby/dbordb/blob/main/summaries/calvin.md)                | 2PL             | Pax  |:x:| L/D ||||     
| [Salt](https://github.com/jackwaudby/dbordb/blob/main/summaries/salt.md)                    | M               |      || L ||||
| [Callas](https://github.com/jackwaudby/dbordb/blob/main/summaries/callas.md)                | M               |      || L ||||       
| [Replicated Commit](https://github.com/jackwaudby/dbordb/blob/main/summaries/rep_commit.md) |                 | Pax  || L ||||
| [Rococo](https://github.com/jackwaudby/dbordb/blob/main/summaries/rococo.md)                | G               | Pax  || L ||||
| [MDCC](https://github.com/jackwaudby/dbordb/blob/main/summaries/mdcc.md)                    |                 ||| U |S/RC|||
| [TAPIR](https://github.com/jackwaudby/dbordb/blob/main/summaries/tapir.md)                  |                 ||| U | SS |||
| [Janus](https://github.com/jackwaudby/dbordb/blob/main/summaries/janus.md)                  | G               | Q |:x:| U | SS |H|OS|
| [PNUTS](https://github.com/jackwaudby/dbordb/blob/main/summaries/pnuts.md)                  |                 ||| E ||||
| [Dynamo](https://github.com/jackwaudby/dbordb/blob/main/summaries/dynamo.md)                |                 ||| E ||||
| [TAO](https://github.com/jackwaudby/dbordb/blob/main/summaries/tao.md)                      |                 ||| E ||||
| [COPS](https://github.com/jackwaudby/dbordb/blob/main/summaries/cops.md)                    |                 ||| C ||||
| [Eiger](https://github.com/jackwaudby/dbordb/blob/main/summaries/eiger.md)                  |                 ||| C ||||
| [Walter](https://github.com/jackwaudby/dbordb/blob/main/summaries/walter.md)                |                 |||||||
| [Lynx](https://github.com/jackwaudby/dbordb/blob/main/summaries/lynx.md)                    |                 |||||||
| [SLOG](https://github.com/jackwaudby/dbordb/blob/main/summaries/slog.md)                    |                 |||||||
| [OceanVista](https://github.com/jackwaudby/dbordb/blob/main/summaries/oceanvista.md)        |                 ||| U ||||
| Tango ||||||||
| Corfu ||||||||                                                                                      
| ANNA  ||||||||

Concurrency Control (CC):
+ **OCC**: *optimistic concurrency control*.
+ **2PL**: *two-phase locking*.
+ **TO**: *timestamp ordering*.
+ **G**: *graph-based*.
+ **M**: *mixed*, combines multiple protocols.

Replication (Rep):
+ **PB**: *primary-backup*.
+ **VSR**: *viewstamped replication*.
+ **Pax**: *paxos*.
+ **MPax**: *multi-paxos*.
+ **Raft**: *raft*.
+ **Q**: *quorum*. 

Type:
+ **L**: *layered*, protocols are combined, e.g., 2PL + 2PC + Paxos.
+ **U**: *unified*, protocols are combined into one.
+ **E**: *eventual*: evenutal consistency.
+ **C**: *causal*: causal consistency.
+ **D**: *deterministic*: deterministic.

Isolation Level (Iso): 
+ **SS**: *strict serializabilty*.
+ **RC**: *read committed*.

Contention (Con): 
+ **H**: designed for high contention.
+ **L**: designed for low contention.

Transaction Model (TM):
+ **OS**: *one-shot*.
