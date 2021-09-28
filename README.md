# Database of Research Databases

Taking motivation from the excellent [Database of Databases](https://dbdb.io/) the goal here is to catalog **research protoype database systems**, highlighting their "novel" features and design principles.
Initially there will be a focus on distributed databases - [here](https://github.com/rystsov/awesome-distributed-transactions) is a collection of distributed transaction materials.

| Name | Concurrency Control | Replication | Commitment | Type | Isolation | Contention | TM | MPT |
| :---:| :-:| :--: | :-: | :--: | :--: | :--: |:--:|:--:|
| [Janus](https://github.com/jackwaudby/dbordb/blob/main/summaries/janus.md)                  | G    |S/WA  |2in1|U|SS |H  | OS  |:white_check_mark:|
| [OceanVista](https://github.com/jackwaudby/dbordb/blob/main/summaries/oceanvista.md)        | ACC  |S/WQ  |2in1|U|SS |L/H| F   |:white_check_mark:|
| [RAMP](https://github.com/jackwaudby/dbordb/blob/main/summaries/ramp.md)                    |RAMP/*|:x:   |2PC |L|RA |L/H|GRW  |:white_check_mark:|
| [RAMP-TAO](https://github.com/jackwaudby/dbordb/blob/main/summaries/ramp_tao.md)            |RAMP/F|A/EC  |2PC |L|RA |H  |RO/WO|:white_check_mark:|
| [CockroachDB](https://github.com/jackwaudby/dbordb/blob/main/summaries/cockroach.md)        |OCC   |S/Raft|PC  |L|S  |-  |I    |:white_check_mark:|
| [GSI](https://github.com/jackwaudby/dbordb/blob/main/summaries/gsi.md)                      |MVSB   |A/EC  |AB  |L|G/PC-SI|-  |I    |:x:|

Concurrency Control:
+ **OCC**: *optimistic concurrency control*.
+ **2PL**: *two-phase locking*.
+ **TO**: *timestamp ordering*.
+ **G**: *graph-based*.
+ **M**: *mixed*, combines multiple protocols.
+ **ACC**: *asynchronous concurrency control*, technique used in OceanVista.
+ **RAMP**: *read atomic multi-partition*, (F: fast, S: small, H: hybird).
+ **MVSB**: *multi-version snapshot based*.


Replication:
+ **S**: *Synchronus replication*:
  + **PB**: *primary-backup*.
  + **VSR**: *viewstamped replication*.
  + **Pax**: *paxos*.
  + **MPax**: *multi-paxos*.
  + **Raft**: *raft*.
  + **WA**: *write-all*.
  + **WQ**: *write-quorum*.
+ **A**: *asynchronus replication*:
  +  **EC**: *eventual consistency*.

Atomic Commitment:
* *2PC*: *two-phase commit*.
* *2in1*: *two-in-one*: commitment combined with concurrency control and replication.
* **PC**: *parallel commits*.
* **AB**: *atomic broadcast*.

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
+ **GSI**: *generalisable snapshot isolation*, also referred to as weak-SI and ANSI-SI.

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

TODO:
* Distinguish between single versioned vs multi-versioned systems. 
