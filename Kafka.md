### 1️⃣ What is Apache Kafka?
Kafka is a **distributed event streaming platform** used to build **real-time data pipelines** and **streaming applications**.  
It supports **publish-subscribe messaging**, **fault tolerance**, and **high throughput**.

---

### 2️⃣ What are the main components of Kafka?
- **Producer:** Publishes messages to topics.  
- **Consumer:** Reads messages from topics.  
- **Broker:** Kafka server storing and serving messages.  
- **Topic:** Logical channel of data.  
- **Partition:** Division of topic data for scalability.  
- **Zookeeper / KRaft:** Handles metadata and coordination.

---

### 3️⃣ What is a Kafka Topic?
A topic is a **stream of messages** of a particular category. Producers write to it; consumers read from it.

---

### 4️⃣ What is a Partition?
Each topic is divided into **partitions** for parallel processing.  
Messages within a partition are **ordered** and assigned a unique **offset**.

---

### 5️⃣ What is an Offset?
A **unique ID** assigned to each message in a partition. Consumers use it to track their read position.

---

### 6️⃣ What is a Kafka Broker?
A **server** in a Kafka cluster that stores and serves messages.  
Each broker manages multiple partitions.

---

### 7️⃣ What is a Consumer Group?
A group of consumers that share the workload of reading from a topic.  
Each partition is read by **only one consumer** in a group.

---

### 8️⃣ Difference between Kafka and RabbitMQ

| Feature | Kafka | RabbitMQ |
|----------|--------|-----------|
| Model | Distributed log | Queue-based |
| Ordering | Per-partition | Not guaranteed |
| Throughput | High | Moderate |
| Persistence | Log-based | Message queue |
| Use Case | Real-time streams | Task queue |

---

### 9️⃣ What is Kafka Retention Policy?
Determines how long Kafka retains messages:
- **Time-based:** `log.retention.hours`
- **Size-based:** `log.retention.bytes`  
Even consumed messages are retained until the limit.

---

### 🔟 What is a Producer Acknowledgment (`acks`)?
Controls how producers wait for confirmation:
| acks | Meaning |
|------|----------|
| 0 | Fire and forget |
| 1 | Wait for leader |
| all / -1 | Wait for all replicas |

---

## 🟡 Intermediate Questions

---

### 1️⃣1️⃣ How does Kafka ensure durability?
- Messages are **persisted to disk**.  
- Data is **replicated** across brokers.  
- Acks confirm safe writes.

---

### 1️⃣2️⃣ What is Replication Factor?
Defines how many copies of data exist across brokers.  
Example: `replication.factor=3` means 1 leader + 2 followers.

---

### 1️⃣3️⃣ What happens when a broker fails?
Kafka automatically **elects a new leader** for affected partitions from the **in-sync replicas (ISR)** list.

---

### 1️⃣4️⃣ What is ISR (In-Sync Replica)?
List of replicas that are fully caught up with the leader.  
Only ISRs are eligible for leader election.

---

### 1️⃣5️⃣ What is the difference between At-most-once, At-least-once, and Exactly-once delivery?

| Mode | Guarantee |
|------|------------|
| At-most-once | Messages may be lost but never redelivered |
| At-least-once | No loss but may have duplicates |
| Exactly-once | No loss, no duplication |

---

### 1️⃣6️⃣ How do you achieve Exactly-once delivery in Kafka?
Use:
- **Idempotent producers** (`enable.idempotence=true`)
- **Transactional producer API**
- **Kafka Streams** (supports exactly-once processing)

---

### 1️⃣7️⃣ What is Log Compaction?
A cleanup policy that retains **only the latest message per key**, removing older ones.  
Useful for maintaining the latest state (e.g., user profiles).

---

### 1️⃣8️⃣ What is Kafka Streams?
A Java library for **real-time stream processing** built on top of Kafka.  
Supports **stateful**, **windowed**, and **exactly-once** processing.

---

### 1️⃣9️⃣ What is Kafka Connect?
A framework for integrating Kafka with external systems (like MySQL, MongoDB, or Elasticsearch) using **connectors**.

---

### 2️⃣0️⃣ What is the difference between Kafka Streams and Kafka Connect?

| Feature | Kafka Streams | Kafka Connect |
|----------|----------------|---------------|
| Purpose | Stream processing | Data integration |
| Type | Java API | Framework |
| Example | Word count app | Sync MySQL → Kafka |

---

## 🔴 Advanced & Scenario-Based Questions

---

### 2️⃣1️⃣ Scenario: How does Kafka maintain message order?
Messages are **strictly ordered within a partition**, but **not across partitions**.  
To preserve order, use a **consistent key** for partitioning.

---

### 2️⃣2️⃣ Scenario: Consumer is reading too slowly. What can you do?
- Increase number of consumers in the group.  
- Add more partitions for parallel reads.  
- Optimize consumer logic or use batch fetch.

---

### 2️⃣3️⃣ Scenario: Producer is too slow. How to improve throughput?
Tune producer configs:
```properties
batch.size=65536
linger.ms=10
compression.type=snappy
acks=1
````

→ Enables batching, compression, and faster sends.

---

### 2️⃣4️⃣ Scenario: Broker restarts caused data loss. Why?

Possible causes:

* `acks=0` or `acks=1`
* `replication.factor < 3`
* `min.insync.replicas` not enforced
  ✅ Fix by using:

```properties
acks=all
min.insync.replicas=2
replication.factor=3
```

---

### 2️⃣5️⃣ Scenario: Duplicate messages seen in consumers. How to avoid?

* Use **idempotent writes** in consumers.
* Use **exactly-once** semantics.
* Maintain last processed offset in a transactional store.

---

### 2️⃣6️⃣ Scenario: How do you replicate data across Kafka clusters?

Use **MirrorMaker 2.0**, which replicates topics, offsets, and consumer groups across clusters.

---

### 2️⃣7️⃣ Scenario: Kafka topic keeps growing indefinitely. Why?

* Retention policy not configured.
  ✅ Fix:

```properties
log.cleanup.policy=delete
log.retention.hours=72
```

---

### 2️⃣8️⃣ Scenario: Consumer offset management

Kafka stores offsets in the topic **`__consumer_offsets`**.
Consumers can:

* Commit offsets automatically (`enable.auto.commit=true`)
* Or manually (`commitSync()` / `commitAsync()`)

---

### 2️⃣9️⃣ Scenario: How does Kafka handle leader election?

Kafka uses **Zookeeper** or **KRaft** to elect a new leader from ISR replicas if the current leader fails.

---

### 3️⃣0️⃣ How do you monitor Kafka performance?

**Key Metrics:**

* Consumer lag (`kafka.consumer:type=consumer-fetch-manager-metrics`)
* Under-replicated partitions
* Broker CPU/disk usage
* Message throughput


