# Database of Research Databases

Taking motivation from the excellent [Database of Databases](https://dbdb.io/) the goal here is to catalog **research protoype database systems**, highlighting their "novel" features and design principles.
Initially there will be a focus on distributed databases - [here](https://github.com/rystsov/awesome-distributed-transactions) is a collection of distributed transaction materials.

| Name | CC | Rep. | 2PC | Type | Iso. | Con. | TM |
| :---:| :-:| :--: | :-: | :--: | :--: | :--: |:--:|
| [Janus](https://github.com/jackwaudby/dbordb/blob/main/summaries/janus.md)                  | G | WA |:x:| U | SS | H | OS |
| [OceanVista](https://github.com/jackwaudby/dbordb/blob/main/summaries/oceanvista.md)        | ACC | WQ |:x:| U | SS | L/H | F |
| [RAMP](https://github.com/jackwaudby/dbordb/blob/main/summaries/ramp.md)            |RAMP/*|:x:|:white_check_mark:|L|RA|L/H|GRW|
| [RAMP-TAO](https://github.com/jackwaudby/dbordb/blob/main/summaries/ramp_tao.md)            |RAMP/F|A/EC|:white_check_mark:|L|RA|H|RO/WO|
| [CockroachDB](https://github.com/jackwaudby/dbordb/blob/main/summaries/cockroach.md)            ||||||||

Concurrency Control (CC):
+ **OCC**: *optimistic concurrency control*.
+ **2PL**: *two-phase locking*.
+ **TO**: *timestamp ordering*.
+ **G**: *graph-based*.
+ **M**: *mixed*, combines multiple protocols.
+ **ACC**: *asynchronous concurrency control*, technique used in OceanVista.
+ **RAMP**: *read atomic multi-partition*, (F: fast, S: small, H: hybird).

Replication (Rep):
+ **PB**: *primary-backup*.
+ **VSR**: *viewstamped replication*.
+ **Pax**: *paxos*.
+ **MPax**: *multi-paxos*.
+ **Raft**: *raft*.
+ **WA**: *write-all*.
+ **WQ**: *write-quorum*.
+ **A**: *asynchronus replication*, exact guarantees vary (EC: evenutal consistency).

Two-phase Commit (2PC):
* :white_check_mark:: yes.
* :x:: now.
* **PC**: *parallel commits*.

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
+ **I**: *interactive*, interactive transactions.
+ **RW**: *read-write*, read-write transactions.
+ **RO**: *read-only*, read-only transactions.
+ **WO**: *write-only*, write-only transactions.
+ **OS**: *one-shot*, transactions that can be broken into pieces-per-shard in which no piece is dependent on another.
+ **F**: *functors*, transactions capable of executing fully from any server in the database.
