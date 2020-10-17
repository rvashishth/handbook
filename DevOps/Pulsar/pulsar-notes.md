- [Notes form streamnative TGI live](#notes-form-streamnative-tgi-live)
- [message ordering guarantee](#message-ordering-guarantee)
- [Storage Configuration](#storage-configuration)
      - [Rough notes](#rough-notes)
    - [config](#config)
- [Performance test](#performance-test)
- [Geo replication](#geo-replication)
    - [Questions](#questions)
- [Scaling](#scaling)
- [Sizing the cluster](#sizing-the-cluster)
- [Large Message Size At Pulsar](#large-message-size-at-pulsar)
- [FAQ](#faq)
- [Questions](#questions-1)

## Notes form streamnative TGI live

- keep default message size to a low number, and ask client to enableChunking for large messages.
- future pulsar version will support the topic level policy. intrestingly - topic level policy will be stored in system topics.
- now consumer can set retry delay for each message, max retries and `retryLaterTopic` to send failed messages on an another topic

## message ordering guarantee

Message ordering - Each Pulsar message belongs to an ordered sequence id on its topic and
are guaranteed to be read from that topic in the same order

**ordering is guaranteed when** </br>
A producer sends messages to a single topic
for a partitioned topic, all message produced with the same key will be in order and placed in the same partition
for a partitioned topic, all messages from the same producer sent to one partition will be in order

**ordering is not guaranteed when**</br>
When consumers are subscribed using shared and key_shared subscription
When a producer send messages to multiple topics ordering is not guaranteed

## Storage Configuration

- `writeQuorum` and `ackQuorum` - in bookie we can define how many copy of a message we want to keep

By default pulsar

- For a topic which don't have a subscription, all message produced to this topic will be deleted immediately
- For a topic which has subscriptions, pulsar will keep the message in backlog storage until it is not consumed by all subscriptions. Once consumed message is marked as ready for deletion.

Backlog quota and TTL applies to unacked messages while Retention policy applies to acked messages. Both policies for acked and unacked works independently to each other.

**Backlog quota** - Pulsar stores all unacknowledged messages in backlogs per topic until they are processed and acknowledged. Set the backlogQuotaDefaultLimitGB to control this.

**TTL** - only apply to unacknowledged messages. It dictates how long a message should be available for consumption. Pulsar does not delete a message once it's TTL is over, instead it mark the message as acknowledged. For now TTL can be set on namespace, but in future release 2.7.0 TTL would be set on Topic as well.

**Retention** - only apply to message which are already consumed(acknowledged) by all subscriptions and keep the messages in storage which are even consumed/acked by all subscriptions. Retention can be applied by time or size, whichever occurs first will apply.

**persistent storage** - accounts for total segment size of all un-deleted messages? be it unacknowledged or acknowledged messages.

**Message Backlog** - how many messages there are not consumed by subscription. Each subscription has it's own backlog. this backlog only have cursor on the message position. and does not contain actual message. Actual messages are stored in topic level backlog.

Pulsar don't delete individual message, instead each message is a part of segment and pulsar deletes and segment when all message of that segment are ready for deletion.

**Subscription** A subscription is a "view" on a topic, it represents what you have and haven't seen yet. subscription is basically a pointer into a topic, talking about acknowledged messages only really applies to a subscription, not the underlying topic, SubscriptionInitialPosition.Earliest means earliest message from topic storage(including acked message). So the consumer would start reading from the earliest message from storage.

##### Rough notes

https://github.com/apache/pulsar/issues/7500
https://apache-pulsar.slack.com/archives/C5Z4T36F7/p1594348193122700
https://app.slack.com/client/T5Z3B324U/C5Z4T36F7/thread/C5Z4T36F7-1594923017.331600
https://apache-pulsar.slack.com/archives/C5Z4T36F7/p1588804698285600?thread_ts=1588792171.257300&cid=C5Z4T36F7

The retention policy settings do not affect unacknowledged messages on topics with subscriptions
the message in a topic itself does not have an acknowledgment flag,

Tiered Storage merely extends the disk space. It does not govern the message retention policy. Retention policy also applies to tiered storage.

Backlog quota puts a limit on how many unacked messages on a subscription.

How to estimate a storage size = write quorum \* retention size

#### config

broker - defaultRetentionTimeInMinutes
broker - defaultRetentionSizeInMB
broker - backlogQuotaDefaultLimitGB

## Performance test

- https://locust.io/ (python specific)

## Geo replication

- use global zookeeper
- A topic will be in all cluster
- Whenever a message is produced to any topic, that will be replicated to all clusters
- You must enable geo-replication on a per-tenant basis in Pulsar. then after we need to create namespace with enabled replication.
- Once you create a geo-replication namespace, any topics that producers or consumers create within that namespace is replicated across clusters.
- You can restrict replication selectively by specifying a replication list for a message, and then that message is replicated only to the subset in the replication list.
- For each topic pulsar will create a producer which will read the data from topic in one cluster and will push that to another topic in different cluster
- To ensure no data de-duplication we need to enable message de-duplication on brokers.

- async vs sync data replication

  - in sync - The write request is typically only acknowledged to the client when the majority of the data centers have issued a confirmation that the write has been persisted
  - in async - ack is sent right after the one data center confirm the write

- three patterns
  - Active - Active with global zookeeper
  - Active - Active with distributed zookeeper
  - Active - Passive(fail-over)

**Azure routing for Geo replication** </br>

- use azure Traffic Manager, as pulsar binary protocol is non-http.

* keep it by def

#### Questions

1. How does a client will connect to a specific cluster/ How TF manager will route request to specific backend if we don't use cluster replication for all namespace/topics?
2. how does replication works for functions
3. should we use enable de-duplication?
4. Should the replication works for all namespace and topics?

## Scaling

Zookeeper keep copy of same data on each node. We can scale up/down the zookeeper by keeping the quorum in place

Bookkeeper store parts of data under on node, to scale down a bookkeeper
https://bookkeeper.apache.org/docs/latest/admin/decomission/
https://github.com/streamnative/charts/blob/master/scripts/pulsar/decommission_bookies.sh

## Sizing the cluster

ODX - 288 TB per hour
600,000 per hour
20 topic
12 mb size
2 write quorum
600000 x 20 x 12 x 2 / 1000 / 1000 = 288 TB

eFR - 10 TB per hour
500,000 per hour
10 mb per msg
2 write quorum
500000 x 10 x 2 / 1000 / 1000 = 10 TB

## Large Message Size At Pulsar

https://github.com/apache/pulsar/wiki/PIP-37%3A-Large-message-size-handling-in-Pulsar
https://medium.com/streamnative/whats-new-in-apache-pulsar-2-4-0-d646f6727642#:~:text=Previously%2C%20Pulsar%20limits%20the,aka%20MaxMessageSize)%20to%205%20MB.
https://docs.microsoft.com/en-us/azure/architecture/patterns/claim-check

## FAQ

1. What is the use Load Balancer in broker </br>
   **Answer:** this is for load balancing of resources for a namespace. this auto balance topics around brokers.
2. How do we control the number of message copies we want to keep in bookies <br>
   **Answer:** `writeQuorum` and `ackQuorum` - in bookie we can define how many copy of a message we want to keep

## Questions

1. Broker/partitions
   1. how many partitions a broker can have
   2. can we have more number of partitions then the number of brokers
   3. what is the benefit of having multiple partition of one topic within one broker
   4. If we apply a backlog quota on topic, and create multiple partitioned topic, does this quota applies to all partition in total or to individual partition
2. what is write quorum and the default value for write quorum in bookie
3. What is a offline topic?
4. How can i performance test of tcp endpoints using java
5. if you create a subscription in advance, does the consumer who later start using that subscription will read the oldest message or not
6. Why we need to keep the message in storage after all subscriptions has consumed the messages
7. Pulsar bookie journel vs ledger storage, why we have 4 PVC for journal storage and 4 for ledger storage
