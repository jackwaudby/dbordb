# Database of Research Databases

Taking motivation from the excellent [Database of Databases](https://dbdb.io/) the goal here is to catalog **research protoype database systems**, highlighting their "novel" features and design principles.
Initially there will be a focus on distributed databases.

| Name | CC | Rep. | 2PC | Type | Iso. | Con. | TM |
| :---:| :-:| :--: | :-: | :--: | :--: | :--: |:--:|
| [Janus](https://github.com/jackwaudby/dbordb/blob/main/summaries/janus.md)                  | G | WA |:x:| U | SS | H | OS |
| [OceanVista](https://github.com/jackwaudby/dbordb/blob/main/summaries/oceanvista.md)        | ACC | WQ |:x:| U | SS | L/H | F |
| [RAMP](https://github.com/jackwaudby/dbordb/blob/main/summaries/ramp.md)            |RAMP|:x:|:white_check_mark:|L|RA|L/H|GRW|
| [RAMP-TAO](https://github.com/jackwaudby/dbordb/blob/main/summaries/ramp_tao.md)            |RAMP|A/EC|:white_check_mark:|L|RA|H|RO/WO|
| [Sinfonia](https://github.com/jackwaudby/dbordb/blob/main/summaries/sinfonia.md)            ||||||||
| [Percolator](https://github.com/jackwaudby/dbordb/blob/main/summaries/percolator.md)        ||||||||       
| [Spanner](https://github.com/jackwaudby/dbordb/blob/main/summaries/spanner.md)              ||||||||       
| [CLOCC](https://github.com/jackwaudby/dbordb/blob/main/summaries/clocc.md)                  ||||||||      
| [Granola](https://github.com/jackwaudby/dbordb/blob/main/summaries/granola.md)              ||||||||
| [Calvin](https://github.com/jackwaudby/dbordb/blob/main/summaries/calvin.md)                ||||||||     
| [Salt](https://github.com/jackwaudby/dbordb/blob/main/summaries/salt.md)                    ||||||||
| [Callas](https://github.com/jackwaudby/dbordb/blob/main/summaries/callas.md)                ||||||||       
| [Replicated Commit](https://github.com/jackwaudby/dbordb/blob/main/summaries/rep_commit.md) ||||||||
| [Rococo](https://github.com/jackwaudby/dbordb/blob/main/summaries/rococo.md)                ||||||||
| [MDCC](https://github.com/jackwaudby/dbordb/blob/main/summaries/mdcc.md)                    ||||||||
| [TAPIR](https://github.com/jackwaudby/dbordb/blob/main/summaries/tapir.md)                  ||||||||
| [PNUTS](https://github.com/jackwaudby/dbordb/blob/main/summaries/pnuts.md)                  ||||||||
| [Dynamo](https://github.com/jackwaudby/dbordb/blob/main/summaries/dynamo.md)                ||||||||
| [TAO](https://github.com/jackwaudby/dbordb/blob/main/summaries/tao.md)                      ||||||||
| [COPS](https://github.com/jackwaudby/dbordb/blob/main/summaries/cops.md)                    ||||||||
| [Eiger](https://github.com/jackwaudby/dbordb/blob/main/summaries/eiger.md)                  ||||||||
| [Walter](https://github.com/jackwaudby/dbordb/blob/main/summaries/walter.md)                ||||||||
| [Lynx](https://github.com/jackwaudby/dbordb/blob/main/summaries/lynx.md)                    ||||||||
| [SLOG](https://github.com/jackwaudby/dbordb/blob/main/summaries/slog.md)                    ||||||||
| Tango ||||||||
| Corfu ||||||||                                                                                      
| ANNA  ||||||||

Concurrency Control (CC):
+ **OCC**: *optimistic concurrency control*.
+ **2PL**: *two-phase locking*.
+ **TO**: *timestamp ordering*.
+ **G**: *graph-based*.
+ **M**: *mixed*, combines multiple protocols.
+ **ACC**: *asynchronous concurrency control*, technique used in OceanVista.
+ **RAMP**: *read atomic multi-partition*.

Replication (Rep):
+ **PB**: *primary-backup*.
+ **VSR**: *viewstamped replication*.
+ **Pax**: *paxos*.
+ **MPax**: *multi-paxos*.
+ **Raft**: *raft*.
+ **WA**: *write-all*.
+ **WQ**: *write-quorum*.
+ **A**: *asynchronus replication*, exact guarantees vary (EC: evenutal consistency).

Type:
+ **L**: *layered*, protocols are combined, e.g., 2PL + 2PC + Paxos.
+ **U**: *unified*, protocols are combined into one.
+ **E**: *eventual*: evenutal consistency.
+ **C**: *causal*: causal consistency.
+ **D**: *deterministic*: deterministic.

Isolation Level (Iso): 
+ **SS**: *strict serializabilty*.
+ **RAMP**: *read atomic*.
+ **RC**: *read committed*.

Contention (Con): 
+ **H**: designed for high contention.
+ **L**: designed for low contention.

Transaction Model (TM):
+ **GRW**: *general read-write*, general read-write transactions.
+ **RO**: *read-only*, read-only transactions.
+ **WO**: *write-only*, write-only transactions.
+ **OS**: *one-shot*, transactions that can be broken into pieces-per-shard in which no piece is dependent on another.
+ **F**: *functors*, transactions capable of executing fully from any server in the database.
