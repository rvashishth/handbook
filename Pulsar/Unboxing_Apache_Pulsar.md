- pulsar uses apache flink

https://hub.streamnative.io/
https://streamnative.io/blog
https://medium.com/streamnative

## Unboxing apache pulsar

### Major

#### Apache Zookkeeper

    zookeeper allow distributed process to co-ordinate with each other, consider it as an in-memory distributed
    file system where data is copied among the nodes of one cluster.

    Zookeeper as a Service Discovery -
    It can act as a registry of active server apps which can be discovered by other client app, like eureka style service registry.

    Zookeeper as config/metadata server - where client app register as watcher for a zNode, zk can notify client on any
    config change.

    zookeeper data is kept in memory
    Running in replicated modes, all zookeeper server has copies of same configuration file
    zookeeper keeps a snapshot of in-memory data

#### Apache Bookkeeper

    Bookie - are underlying storage servers used by Bookkeeper.

    Individual bookies store fragments of ledgers, not entire ledgers (for the sake of performance).  For any given ledger L, an ensemble is the group of bookies storing the entries in L.

    Metadata about bookie and ledgers are stored in Zookkeeper

    Ledger
        Segments
            Entries(record)
                msg1
                msg2

    Each ledger is stored by multiple bookie

    Each bookie has it's own disk storage

#### Storage disk

#### Pulsar broker

    Bookie clients, which integrates with zookeeper and bookie storage servers

#### Autorecovery

#### Herd DB

    A db which run inside the same java process(like SQLite) but at the same time also replicated.
    Why embedded db - No need to maintain the db process, sits with app process
    Herb db stores the data as key-value pair but still uses JDBC and SQL Clients, it uses Apache Calcite(https://calcite.apache.org/)
    sql parser.

- Protocol Buffers - https://developers.google.com/protocol-buffers/
- Apache Avro - https://avro.apache.org/
- Netty and Jetty

### Minors

- http://hdrhistogram.org/

### Te Be Added

- [ ] Tech behind pulsar broker
- [ ] Engine behind pulsar function worker
