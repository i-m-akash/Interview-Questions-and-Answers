🟢 Basic Kafka Interview Questions
1️⃣ What is Apache Kafka?

Answer:
Kafka is a distributed streaming platform used for:

Publish-subscribe messaging

Real-time data streaming

Event-driven architecture

Data pipeline integration

It’s highly scalable, fault-tolerant, and designed for high throughput.

2️⃣ What are the main components of Kafka?

Answer:

Producer: Sends messages to Kafka topics.

Consumer: Reads messages from topics.

Broker: Kafka server that stores and serves data.

Topic: Logical channel for data (like a table).

Partition: Subset of a topic enabling parallelism.

Zookeeper (legacy) / KRaft (new): Manages metadata, leader election, etc.

3️⃣ What is a Kafka Topic?

Answer:
A topic is a category or feed name to which records are published.
Example:

Topic: order_events

Producers → publish order data

Consumers → read from it

4️⃣ What is a Partition in Kafka?

Answer:

Each topic is divided into partitions to allow parallel reads/writes.

Each partition is an ordered, immutable log of messages.

Messages within a partition have a unique offset.

5️⃣ What is an Offset?

Answer:
The offset is a unique sequence ID for each message in a partition.
It helps Kafka keep track of which messages have been read.

6️⃣ What is a Consumer Group?

Answer:
A consumer group is a collection of consumers that share the load of reading messages from a topic’s partitions.
Kafka ensures each partition is read by only one consumer in a group.

7️⃣ Difference between Kafka and RabbitMQ?
Feature	Kafka	RabbitMQ
Message model	Distributed log	Queue-based
Ordering	Ordered per partition	Not guaranteed
Speed	High throughput	Lower throughput
Storage	Persistent log	Short-lived
Use case	Event streaming	Task queues
8️⃣ What is Kafka Broker?

Answer:
A Kafka broker is a server that stores data and serves clients.

Each broker can handle thousands of partitions.

A Kafka cluster consists of multiple brokers.

9️⃣ What is Kafka Producer Acknowledgement (acks)?

Answer:
Determines when a producer considers a message as "successfully sent":

acks	Meaning
0	Producer doesn’t wait for acknowledgment
1	Waits for leader acknowledgment
all / -1	Waits for all replicas to acknowledge
🔟 What is Kafka Retention Policy?

Answer:
It decides how long messages are kept:

Based on time (e.g., log.retention.hours=168)

Or size (e.g., log.retention.bytes=1073741824)

Even if consumed, data isn’t deleted until retention expires.

🟡 Intermediate Kafka Interview Questions
1️⃣1️⃣ How does Kafka ensure message durability?

Answer:
Kafka writes messages to disk (using append-only log) and replicates them across brokers.
So, even if one broker fails, data is not lost.

1️⃣2️⃣ What is Kafka Replication Factor?

Answer:
Defines how many copies of data Kafka keeps across brokers.
Example:
replication.factor=3 → 1 leader + 2 followers.

This ensures fault tolerance.

1️⃣3️⃣ What happens when a Kafka broker fails?

Answer:

The leader for its partitions becomes unavailable.

A follower replica is elected as the new leader (via Zookeeper/KRaft).

Producers and consumers reconnect to the new leader automatically.

1️⃣4️⃣ What is Kafka ISR (In-Sync Replica)?

Answer:
ISR = List of replicas that are fully synchronized with the leader.
Only ISRs can become new leaders if the current leader fails.

1️⃣5️⃣ How does Kafka handle backpressure?

Answer:
Kafka allows:

Batching: producers send multiple messages in one request.

Flow control: producers can adjust the send rate or block when the buffer is full.

1️⃣6️⃣ Explain At-most-once, At-least-once, Exactly-once delivery in Kafka.
Mode	Description
At-most-once	Messages may be lost, but never redelivered
At-least-once	No message loss, but duplicates possible
Exactly-once	No loss, no duplication (supported via idempotent producers & transactions)
1️⃣7️⃣ How do you achieve exactly-once delivery in Kafka?

Answer:
Use:

Idempotent producers (enable.idempotence=true)

Transactional producer API for atomic writes

Kafka Streams API (built-in exactly-once semantics)

1️⃣8️⃣ What is the role of Zookeeper (or KRaft)?

Answer:
Used for:

Leader election

Metadata management

Cluster coordination
(Note: Kafka 2.8+ introduced KRaft mode removing the need for Zookeeper.)

1️⃣9️⃣ How does Kafka ensure order of messages?

Answer:
Kafka guarantees order within a partition, not across partitions.
To preserve order for specific keys, use a key-based partitioner.

2️⃣0️⃣ What is Log Compaction in Kafka?

Answer:
A cleanup policy (compact) that keeps only the latest message per key, ensuring compacted topics never grow indefinitely.
Useful for maintaining latest state snapshots (e.g., user profile updates).

🔴 Advanced & Scenario-Based Kafka Questions
2️⃣1️⃣ Scenario: How do you handle message duplication in Kafka consumers?

Answer:

Use idempotent writes on the consumer side (e.g., deduplicate via unique keys).

Store last processed offset in a transactional store (DB or Kafka itself).

Use exactly-once semantics if supported.

2️⃣2️⃣ Scenario: Your consumer is too slow; messages keep piling up. How to fix?

Possible Solutions:

Increase number of consumer instances in the group.

Increase partitions for parallelism.

Optimize consumer logic.

Use backpressure or batch processing.

2️⃣3️⃣ Scenario: Kafka producer sending messages too slowly. How do you optimize throughput?

Answer:
Tune producer settings:

batch.size=65536
linger.ms=10
compression.type=snappy
acks=1


Explanation:

Larger batches reduce overhead.

Linger adds a small delay to allow batching.

Compression reduces network IO.

2️⃣4️⃣ Scenario: How do you ensure data is not lost during broker restart?

Answer:

Enable replication factor ≥ 3.

Use acks=all for producers.

Use min.insync.replicas=2.

Ensure log.flush.interval.messages and log.flush.interval.ms are properly set.

2️⃣5️⃣ Scenario: How do you migrate data from one Kafka cluster to another?

Answer:
Use MirrorMaker 2.0, which replicates topics, offsets, and consumer groups across clusters.

2️⃣6️⃣ How does Kafka Streams differ from Kafka Connect?
Component	Purpose
Kafka Streams	Build real-time stream processing apps (Java API).
Kafka Connect	Move data in/out of Kafka from external systems (like MySQL, Elasticsearch).
2️⃣7️⃣ What are Kafka Connectors?

Answer:
Pre-built integrations for data movement:

Source Connectors: Pull data into Kafka (e.g., JDBC Source).

Sink Connectors: Push data from Kafka to external systems (e.g., Elasticsearch Sink).

2️⃣8️⃣ How do you monitor Kafka performance?

Answer:
Use tools like:

Prometheus + Grafana

Kafka Manager / Confluent Control Center
Metrics to watch:

Lag per consumer group

Under-replicated partitions

Broker CPU/disk usage

Message throughput

2️⃣9️⃣ What is Kafka’s default message ordering guarantee?

Answer:

Messages within the same partition are strictly ordered.

Across partitions, order is not guaranteed.

3️⃣0️⃣ How does Kafka achieve fault tolerance?

Answer:

Replication: data copied across brokers.

Leader election: automatic failover.

Acknowledgments: ensure delivery confirmation.

Durable logs: data persisted to disk.
