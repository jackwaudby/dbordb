# Database of Research Databases

Taking motivation from the excellent [Database of Databases](https://dbdb.io/) the goal here is to catalog **research protoype database systems**, highlighting their "novel" features and design principles.
Initially there will be a focus on distributed databases - [here](https://github.com/rystsov/awesome-distributed-transactions) is a collection of distributed transaction materials.

| Name | Concurrency Control | Replication | Commitment | Type | Isolation | Contention | TM | MPT |
| :---:| :-:| :--: | :-: | :--: | :--: | :--: |:--:|:--:|
| [Janus](https://github.com/jackwaudby/dbordb/blob/main/summaries/janus.md)                  | G    |S/WA  |2in1|U|SS     |H  |OS   |:white_check_mark:|
| [OceanVista](https://github.com/jackwaudby/dbordb/blob/main/summaries/oceanvista.md)        | ACC  |S/WQ  |2in1|U|SS     |L/H|F    |:white_check_mark:|
| [RAMP](https://github.com/jackwaudby/dbordb/blob/main/summaries/ramp.md)                    |RAMP/*|:x:   |2PC |L|RA     |L/H|RW   |:white_check_mark:|
| [RAMP-TAO](https://github.com/jackwaudby/dbordb/blob/main/summaries/ramp_tao.md)            |RAMP/F|A/EC  |2PC |L|RA     |H  |RO/WO|:white_check_mark:|
| [CockroachDB](https://github.com/jackwaudby/dbordb/blob/main/summaries/cockroach.md)        |OCC   |S/Raft|PC  |L|S      |-  |I    |:white_check_mark:|
| [Generalised SI](https://github.com/jackwaudby/dbordb/blob/main/summaries/gsi.md)           |MVSB  |A/EC  |AB  |L|G/PC-SI|-  |I    |:x:|
| [Walter](https://github.com/jackwaudby/dbordb/blob/main/summaries/walter.md)                |MVSB  |A/EC  |2PC |L|P-SI   |-  |I/PS |:x:|
| [Strong Session SI](https://github.com/jackwaudby/dbordb/blob/main/summaries/ssesssi.md)    |MVSB  |A/EC  |:x: |E|G/PC-SI|-  |RO/RW|:x:|
| [Clock SI](https://github.com/jackwaudby/dbordb/blob/main/summaries/clocksi.md)             |MVSB  |:x:   |2PC |-|G/C-SI |-  |RO/RW|:white_check_mark:|
| [H-Store](https://github.com/jackwaudby/dbordb/blob/main/summaries/hstore.md)             |OCC  |WA   |2PC* |U|S |-  |OS/TP/ST|:white_check_mark:|


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
  + **WA**: *write-all*, or update everywhere.
  + **WQ**: *write-quorum*.
+ **A**: *asynchronus replication*:
  +  **EC**: *eventual consistency*.

Atomic Commitment:
* **2PC**: *two-phase commit*, * means sometimes used.
* **2in1**: *two-in-one*: commitment combined with concurrency control and replication.
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
+ **S**: *serializabilty*.
+ **RAMP**: *read atomic*.
+ **RC**: *read committed*.
+ **C-SI**: *conventional snapshot isolation*, also referred to as Strong-SI.
+ **G-SI**: *generalisable snapshot isolation*, also referred to as Weak-SI and ANSI-SI.
+ **PC-SI**: *prefix consistent snapshot isolation*, also referred to as Strong Session SI.
+ **P-SI**: *parallel snapshot isolation*.

Contention (Con): 
+ **H**: designed for high contention.
+ **L**: designed for low contention.

Transaction Model (TM):
+ **I**: *interactive*, interactive transactions.
+ **RW**: *read-write*, read-write transactions.
+ **RO**: *read-only*, read-only transactions.
+ **WO**: *write-only*, write-only transactions.
+ **OS**: *one-shot*, transactions that can be broken into pieces-per-shard in which no piece is dependent on another.
+ **TP**: *two-phase*, sequence of read operations (phase 1) followed by a consistency check to determine if the transaction needs to abort, before write operations are executed (phase 2). Strongly two-phase if phase 1 operations produce the same result at all sites.
+ **ST**: *sterile*, two transactions commute if any interleaving of their single-site sub-plans produces the same final database site as any other interleaving. A transaction class (group of transactions) is sterile if it commutes with all transaction classes (including itself).
+ **F**: *functors*, transactions capable of executing fully from any server in the database.
+ **PS**: *preferred sites*, transactions operating on only locally preferred site data can commit without remote commuication.


TODO:
* Distinguish between single versioned vs multi-versioned systems. 
