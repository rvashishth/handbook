- [FAQ](#faq)
- [Multi Region Cluster](#multi-region-cluster)
- [Benchmarking](#benchmarking)

- **Subscription modes** 1. Exclusive, 2. Failover 3. Shared 4. Shared Key
  - multiple consumer can use the same subscription name
  - on one topic we can have multiple consumers connecting to that topic via subscription
  - we can restrict one subscription name to be used by one consumer, to create messaging queue type of concept
- **Namespace** - is an administrative unit for topics, which also act as grouping mechanisms for related topics, following are controlled at namespace level
  - retention policy
  - expiry policy
  - access control
  - de-duplication
- **broker configuration** this includes cluster side config done on topics using namespace configuration
  - add retention policy(retention by size, retention by type)
- **producer configuration**
- **consumer configuration**
  - if a consumer want to redeliver a msg, it has to send a **negative acknowledgement** to broker
  - consumer can also configure **acknowledgement timeout**
  - each consumer can configure it's dead letter topic name
- **Dead letter topic** - messages which consumer failed to consume are moved on dead letter topic, which a consumer can re-process letter
- **topic**
  - topics can be persistent or non-persistent
  - You don't need to explicitly create topics in Pulsar. If a client attempts to write or receive messages to/from a topic that does not yet exist, Pulsar will automatically create that topic
  - **BUT** partitioned topic must be explicitly created by admin api
  - message on **non-persistent** topics lives on memory
  - By default, after 60 seconds of creation, topics are considered inactive and deleted automatically to prevent from generating trash data.
- **retention and expiry**
  - all message retention and expiry is managed at namespace level
  - retention can be by size, or by time

## FAQ

1. Run pulsar standalone in local

```bash
$ docker run -it -p 6651:6650 -p 8080:8080 -v $PWD/data:/pulsar/data apachepulsar/pulsar:2.4.1 bin/pulsar standalone
```

2. Use [Pulsar-Express](https://github.com/bbonnin/pulsar-express) for local use only

```bash
$ cd /Users/rvashish/.npm-global/bin
$ ./pulsar-express
```

3. Build pulsar benchmark in local

```bash
$ git clone https://github.com/openmessaging/openmessaging-benchmark
$ cd openmessaging-benchmark
$ mvn clean install -Dlicense.skip=true
```

4. How to run a benchmark test

```bash
$ ./benchmark --drivers driver-pulsar/pulsar.yaml workloads/1-topic-16-partitions-1kb.yaml
```

Issues

- /bin/benchmark.sh is unable to pass classes to `java -server -cp` command.
- Test Benchmark.java class by passing required parameters

## Multi Region Cluster

Geo-replication is the replication of persistently stored message data across multiple clusters of a Pulsar instance.

My consumer/producers would need to connect with respective closest cluster.

You must enable geo-replication on a per-tenant basis in Pulsar. While creating a tenant, we need to give multi cluster access to our tenant

Any message published on any topic in that namespace is replicated to all clusters in the specified set.

We can also enable replication subscription

one pulsar instance can have multiple pulsar cluster in it
cluster within an instance can replicate data within themselves

per cluster specific zookeeper and a global zookeeper

some topic are global or everything global

## Benchmarking

`bin/benchmark   --drivers driver-pulsar/pulsar-effectively-once.yaml   workloads/1-topic-1-partition-1kb.yaml`