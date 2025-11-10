# Modern System Design - Refactored Sample

This file demonstrates the improved structure and note-taking style for system design documentation.

## Key Improvements Applied

1. **📌 Quick Summary blocks** at the top of each section
2. **🎯 Key Concepts** sections with highlighted terms
3. **Comparison tables** for decision-making
4. **💡 When to Use** sections with clear guidance
5. **⚖️ Trade-offs** explicitly called out with pros/cons
6. **⚠️ Common Pitfalls** with solutions
7. **Real-world examples** with company names and scale
8. **📝 Quick Reference** for scannable takeaways
9. **Emojis** for visual hierarchy and scannability
10. **Structured formatting** with consistent heading levels

---

## 1.1 RPC (Remote Procedure Call)

> **📌 Quick Summary**: Abstraction that makes remote function calls look and feel like local procedure calls  
> **Use Cases**: Microservices communication, distributed systems | **Complexity**: ⭐⭐⭐☆☆

### What is RPC?

**Remote Procedure Call (RPC)** is an interprocess communication protocol widely used in distributed systems. It provides an abstraction layer that hides the complexities of network communication, allowing developers to invoke functions on remote servers as if they were local calls.

**Key characteristics**:
- 📦 **Automatic marshaling**: Serializes function arguments and return values
- 🌐 **Network transparency**: Abstracts away network details
- 🔄 **Retry handling**: Manages network failures automatically
- 📍 **OSI Layer**: Spans transport and application layers

### 🎯 Key Concepts

- **Stub**: Client-side proxy that packages the request
- **Skeleton**: Server-side handler that unpacks and processes the request  
- **Marshaling**: Converting data structures into transmittable format
- **IDL** (Interface Definition Language): Contract defining available remote methods
- **Separate address space**: Called procedure executes in different process/computer

### RPC vs Alternative Communication Patterns

| Feature | RPC | REST | GraphQL | Message Queue |
|---------|-----|------|---------|---------------|
| **Coupling** | Tight | Loose | Loose | Very Loose |
| **Latency** | Low (1-5ms) | Medium (10-50ms) | Medium (10-50ms) | High (>100ms) |
| **Type Safety** | Strong | Weak | Medium | Weak |
| **Debugging** | Hard | Easy | Medium | Hard |
| **Best For** | Internal services | Public APIs | Complex queries | Async tasks |

### 💡 When to Use RPC

**Choose RPC when**:
- ✅ Low latency is critical (<10ms required)
- ✅ Strong type safety needed
- ✅ Both client and server under your control
- ✅ Internal microservices communication
- ✅ High-performance requirements

**Avoid RPC when**:
- ❌ Building public-facing APIs (use REST)
- ❌ Need browser/HTTP compatibility
- ❌ Require loose coupling between services
- ❌ Cross-platform/language flexibility is priority

### ⚖️ Trade-offs

**Pros**:
- ✅ **Performance**: Lowest latency for inter-service calls
- ✅ **Type safety**: Compile-time checking with IDL
- ✅ **Developer experience**: Feels like local function calls
- ✅ **Efficiency**: Binary protocols (Protocol Buffers, Thrift)

**Cons**:
- ❌ **Tight coupling**: Changes require coordinated deployments
- ❌ **Debugging difficulty**: Distributed traces required
- ❌ **Network assumptions**: Assumes reliable, fast network
- ❌ **Limited ecosystem**: Fewer tools vs REST

### Real-World Examples

- **Google**: gRPC for internal service mesh (millions of RPCs/sec)
- **Netflix**: Mix of gRPC (internal) + REST (public APIs)
- **Uber**: Apache Thrift for high-performance service communication
- **Twitter**: Finagle RPC framework for microservices

### 📝 Quick Reference

**One-liner**: Makes distributed function calls transparent to developers  
**Popular Frameworks**: gRPC, Apache Thrift, Apache Avro  
**Typical Latency**: 1-5ms within data center  
**Best Alternative**: REST for public APIs, Message Queues for async

---

## 1.2 Fault Tolerance

> **📌 Quick Summary**: Techniques to keep systems running despite component failures  
> **Use Cases**: Mission-critical systems, distributed architectures | **Complexity**: ⭐⭐⭐⭐☆

### What is Fault Tolerance?

**Fault tolerance** is the ability of a system to continue operating properly in the event of failures. In distributed systems, failures are not exceptional—they're expected. Components will fail, networks will partition, and data centers will go down.

### 🎯 Key Technique: Checkpointing

**Checkpointing** is a technique that saves the system's state in stable storage for later retrieval in case of failures.

**How it works**:
1. 📸 **Snapshot**: Periodically save system state to durable storage
2. ⏱️ **Interval-based**: Performed at different time intervals
3. 🔄 **Recovery**: On failure, restore from last checkpoint
4. ▶️ **Resume**: Continue processing from checkpoint state

### Checkpointing Strategies

| Strategy | Frequency | Recovery Time | Overhead | Use Case |
|----------|-----------|---------------|----------|----------|
| **Coordinated** | Sync across nodes | Fast | High | Strong consistency |
| **Uncoordinated** | Independent | Slower | Low | Eventual consistency |
| **Log-based** | Per operation | Fastest | Medium | Transaction systems |
| **Incremental** | Changed data only | Medium | Low | Large state spaces |

### 💡 Design Decisions

**Checkpoint frequency trade-offs**:
- **More frequent**: Faster recovery, higher overhead, less data loss
- **Less frequent**: Lower overhead, slower recovery, more data loss

**Optimal frequency depends on**:
- 💰 Cost of failure (business impact)
- ⚡ Performance overhead tolerance
- 📊 State change rate
- 💾 Storage costs

### ⚖️ Trade-offs

**Pros**:
- ✅ **Fast recovery**: Resume from known good state
- ✅ **Data preservation**: Minimize data loss
- ✅ **Predictable**: Known recovery point
- ✅ **Debugging**: State snapshots aid troubleshooting

**Cons**:
- ❌ **Overhead**: CPU, memory, I/O costs during checkpoint
- ❌ **Storage**: Requires persistent storage
- ❌ **Consistency**: Coordinating across distributed nodes is complex
- ❌ **Stale data**: Gap between last checkpoint and failure

### Complementary Fault Tolerance Techniques

1. **Replication**: Multiple copies of data/services
2. **Redundancy**: Backup components ready to take over
3. **Failover**: Automatic switching to standby systems
4. **Circuit breakers**: Prevent cascading failures
5. **Graceful degradation**: Reduced functionality vs complete failure

### Real-World Applications

- **Apache Spark**: Checkpoints RDD lineage for fault recovery
- **Kubernetes**: Saves etcd state periodically
- **Database systems**: Write-ahead logging (WAL) + periodic snapshots
- **Apache Flink**: Distributed snapshots for stream processing

### 📝 Quick Reference

**One-liner**: Save state periodically to recover from failures quickly  
**Best for**: Long-running computations, stateful distributed systems  
**Typical interval**: Seconds to minutes (depends on workload)  
**Alternatives**: Replication, transaction logs, event sourcing

---

## 1.3 Load Balancer

> **📌 Quick Summary**: Distributes incoming traffic across multiple servers to optimize resource utilization  
> **Use Cases**: High-traffic applications, scalability, fault tolerance | **Complexity**: ⭐⭐⭐☆☆

### What is a Load Balancer?

A **load balancer** acts as a traffic cop, distributing client requests across multiple backend servers. It ensures no single server becomes a bottleneck, improving both performance and availability.

### 🎯 Load Balancing Algorithms

#### 1. **Round-Robin Scheduling**
- **How it works**: Forwards requests to servers in sequential rotation
- **Best for**: Servers with equal capacity, short-lived connections
- **Complexity**: O(1)
- **Statefulness**: Stateless

#### 2. **Weighted Round-Robin**
- **How it works**: Assigns weights to servers based on capacity; higher weight = more requests
- **Best for**: Heterogeneous servers (different specs)
- **Example**: Server A (weight=3) gets 3x requests vs Server B (weight=1)
- **Statefulness**: Stateless

#### 3. **Least Connections**
- **How it works**: Routes new requests to server with fewest active connections
- **Best for**: Long-lived connections, variable request durations
- **Complexity**: O(log n) with priority queue
- **Statefulness**: Stateful (tracks connections)

#### 4. **Least Response Time**
- **How it works**: Routes to server with fastest average response time
- **Best for**: Performance-sensitive applications, real-time services
- **Requires**: Active health checks and metrics
- **Statefulness**: Stateful (tracks response times)

#### 5. **IP Hash**
- **How it works**: Hashes client IP to consistently route to same server
- **Best for**: Session affinity, caching benefits
- **Formula**: `hash(client_IP) % num_servers`
- **Statefulness**: Stateless (deterministic)

#### 6. **URL Hash**
- **How it works**: Hashes request URL to route to specific server
- **Best for**: Cache partitioning, service-specific routing
- **Example**: `/api/users/*` → Server A, `/api/products/*` → Server B
- **Statefulness**: Stateless (deterministic)

### Algorithm Selection Matrix

| Scenario | Recommended Algorithm | Reasoning |
|----------|----------------------|-----------|
| Equal server capacity | Round-robin | Simple, no state needed |
| Different server specs | Weighted round-robin | Proportional to capacity |
| Long-lived connections | Least connections | Prevents connection pile-up |
| Session stickiness needed | IP hash | Same client → same server |
| Cache optimization | URL/IP hash | Maximizes cache hits |
| Latency-critical | Least response time | Routes to fastest server |
| Microservices routing | URL hash | Service-specific backends |

### 💡 Design Decisions

**Stateless vs Stateful Load Balancing**:

| Aspect | Stateless | Stateful |
|--------|-----------|----------|
| **Complexity** | Low | High |
| **Performance** | Fast | Slower (state lookup) |
| **Scalability** | Easy (no state sync) | Hard (state replication) |
| **Accuracy** | Basic | Precise |
| **Examples** | Round-robin, Hash-based | Least connections, LRT |

### ⚖️ Trade-offs

**Pros**:
- ✅ **Scalability**: Horizontal scaling of backend servers
- ✅ **High availability**: Automatic failover to healthy servers
- ✅ **Performance**: Distributes load, prevents hot spots
- ✅ **Flexibility**: Add/remove servers without downtime

**Cons**:
- ❌ **Single point of failure**: LB itself needs redundancy
- ❌ **Added latency**: Extra network hop
- ❌ **Complexity**: State management for stateful algorithms
- ❌ **Session handling**: May need sticky sessions or distributed sessions

### ⚠️ Common Pitfalls

1. **Uneven distribution with round-robin**: If requests have variable durations
   - **Solution**: Use least connections instead

2. **Cache inefficiency**: Different servers = cold cache
   - **Solution**: Use consistent hashing or IP/URL hash

3. **Health checks missing**: Routing to dead servers
   - **Solution**: Implement active health checks (heartbeats)

4. **No LB redundancy**: LB becomes SPOF
   - **Solution**: Deploy LBs in HA pairs (active-passive or active-active)

### Real-World Examples

- **NGINX**: Supports all algorithms, widely used for HTTP load balancing
- **HAProxy**: High-performance TCP/HTTP load balancer
- **AWS ELB**: Managed load balancing (ALB, NLB, CLB)
- **Google Cloud Load Balancer**: Global load balancing with anycast

### 📝 Quick Reference

**One-liner**: Distributes traffic across servers to optimize performance and availability  
**Default choice**: Round-robin for simple cases, least connections for variable loads  
**Typical setup**: Active-passive LB pair + 3+ backend servers  
**Monitoring**: Track requests/sec, error rate, response time per server

---
<!-- TOC --><a name="14-database"></a>
## 1.4 Database Selection

> **📌 Quick Summary**: Choosing between SQL and NoSQL databases based on consistency, scalability, and data structure requirements  
> **Use Cases**: All data-driven applications | **Complexity**: ⭐⭐⭐⭐☆

### SQL vs NoSQL: The Fundamental Choice

The database selection is one of the most critical architectural decisions. The choice between **relational (SQL)** and **non-relational (NoSQL)** databases impacts scalability, consistency, and development complexity.

### 🎯 Key Concepts

- **RDBMS**: Relational database with structured schema, ACID transactions, SQL queries
- **NoSQL**: Non-relational database optimized for scalability, flexible schema
- **Impedance mismatch**: Complexity of mapping object-oriented code to relational tables
- **Horizontal scaling**: Adding more machines vs vertical scaling (bigger machine)
- **CAP theorem**: Can only guarantee 2 of 3: Consistency, Availability, Partition tolerance

### SQL vs NoSQL Comparison

| Dimension | SQL (RDBMS) | NoSQL |
|-----------|-------------|-------|
| **Schema** | Fixed, predefined | Flexible, dynamic |
| **Scalability** | Vertical (scale up) | Horizontal (scale out) |
| **Transactions** | ACID compliant | Eventually consistent (typically) |
| **Joins** | Complex joins supported | Limited or no joins |
| **Use Case** | Complex queries, relationships | High throughput, simple queries |
| **Cost** | Expensive licenses | Often open source |
| **Data Model** | Tables with rows | Documents, key-value, graph, column |

![image](https://github.com/user-attachments/assets/feecf1b2-efe1-4beb-b9d0-6ee735e9fd29)

### 💡 NoSQL Advantages

**When NoSQL Excels**:
- ✅ **Simple design**: No impedance mismatch—store data as documents vs multiple joined tables
- ✅ **Horizontal scaling**: Distribute data across clusters easily as user count grows
- ✅ **High availability**: Node replacement without downtime, automatic replication
- ✅ **Flexible schema**: Handle unstructured/semi-structured data (JSON, XML, BSON)
- ✅ **Cost-effective**: Open source options, runs on commodity hardware

**Example**: Employee data in one document vs 5 joined tables → simpler code, easier maintenance

### SQL Advantages

**When SQL Excels**:
- ✅ **ACID transactions**: Strong consistency guarantees
- ✅ **Complex queries**: Rich query language with joins, aggregations
- ✅ **Data integrity**: Foreign keys, constraints enforce correctness
- ✅ **Mature ecosystem**: 40+ years of tooling, optimization
- ✅ **Strong typing**: Schema validation catches errors early

### Decision Framework

| Requirement | Choose SQL | Choose NoSQL |
|-------------|------------|--------------|
| **Data structure** | Highly structured, normalized | Unstructured, denormalized |
| **Scalability need** | Moderate (10K-100K QPS) | High (>100K QPS) |
| **Consistency** | Strong consistency required | Eventual consistency acceptable |
| **Query complexity** | Complex joins, aggregations | Simple key-based lookups |
| **Schema changes** | Rare, planned | Frequent, unpredictable |
| **Budget** | Sufficient for licenses | Cost-conscious |

### ⚖️ Replication Trade-offs

**Synchronous Replication**:
- ✅ **Pros**: Strong consistency, data durability
- ❌ **Cons**: Higher latency, reduced availability

**Asynchronous Replication**:
- ✅ **Pros**: Low latency, high availability
- ❌ **Cons**: Eventual consistency, potential data loss

### 💼 Real-World Example: Financial Trading Platform

**Scenario**: Global real-time trading platform demanding <10ms latency

**Requirements**:
- Ultra-low latency for split-second decisions
- Global distribution
- High throughput (millions of trades/day)
- Eventual consistency acceptable

**Recommended Approach**:
- ✅ **Asynchronous replication**: Update primary first, replicate later
- ✅ **NoSQL database**: Cassandra or ScyllaDB for horizontal scalability
- ✅ **Multi-region**: Deploy close to trading hubs (NYC, London, Tokyo, Singapore)
- ✅ **Caching layer**: Redis for sub-millisecond reads

**Why asynchronous over synchronous?**
- Primary node responds immediately without waiting for secondary acknowledgments
- Reduces latency from ~50ms (sync) to <5ms (async)
- Trade-off: Replicas may lag by milliseconds, acceptable for this use case
- Business priority: Speed > perfect consistency

### ⚠️ Common Pitfalls

1. **Choosing NoSQL for strong consistency needs**
   - **Problem**: NoSQL typically offers eventual consistency
   - **Solution**: Use SQL or NewSQL (CockroachDB, Spanner) for ACID requirements

2. **Over-normalizing in NoSQL**
   - **Problem**: Applying SQL normalization principles to NoSQL
   - **Solution**: Denormalize, duplicate data for query efficiency

3. **Ignoring data access patterns**
   - **Problem**: Designing schema without understanding queries
   - **Solution**: Design schema based on read/write patterns first

4. **Premature NoSQL adoption**
   - **Problem**: Choosing NoSQL for "scale" before needed
   - **Solution**: Start with SQL, migrate to NoSQL when scaling limits hit

### Popular Database Choices

| Type | Database | Best For | Scale |
|------|----------|----------|-------|
| **SQL** | PostgreSQL | Complex queries, integrity | Moderate |
| **SQL** | MySQL | Web applications, read-heavy | Moderate |
| **Document** | MongoDB | Flexible schema, documents | High |
| **Key-Value** | Redis | Caching, real-time | Very High |
| **Column** | Cassandra | Time-series, high writes | Massive |
| **Graph** | Neo4j | Relationships, social networks | Moderate |

### 📝 Quick Reference

**One-liner**: SQL for structure and consistency, NoSQL for scale and flexibility  
**Default choice**: PostgreSQL for most applications until scale demands NoSQL  
**Scale threshold**: Consider NoSQL at >100K QPS or >10TB data  
**Hybrid approach**: Use both—SQL for transactional data, NoSQL for analytics/caching

---

<!-- TOC --><a name="15-key-value-store"></a>
## 1.5 Key-Value Store

> **📌 Quick Summary**: Distributed hash table for storing key-value pairs with consistent hashing and Merkle trees for synchronization  
> **Use Cases**: Caching, session storage, distributed systems | **Complexity**: ⭐⭐⭐⭐⭐

### Overview

Key-value stores are fundamental building blocks of distributed systems, providing simple `get(key)` and `put(key, value)` operations. Examples include **Redis**, **DynamoDB**, **Cassandra**, and **Riak**.

### 🎯 Key Design Challenges

1. **Load distribution**: How to distribute keys evenly across nodes?
2. **Replication**: How to keep replicas synchronized?
3. **Hotspots**: How to avoid overloading single nodes?
4. **Consistency**: How to detect and resolve data inconsistencies?

---

<!-- TOC --><a name="151consistent-hashing"></a>
### 1.5.1 Consistent Hashing

> **📌 Quick Summary**: Hash function that minimizes key remapping when nodes join/leave the cluster

### The Problem

Traditional hashing (`hash(key) % N nodes`) requires remapping most keys when nodes change. For 1 billion keys and 100 nodes, adding 1 node requires moving ~990 million keys.

### The Solution: Hash Ring

**Consistent hashing** maps both keys and nodes to a circular hash space (0 to 2^160-1). Keys are assigned to the next node found clockwise on the ring.

**Benefits**:
- Adding/removing nodes only affects adjacent nodes
- Minimal key movement (approximately `keys/N` instead of `most keys`)
- Predictable load distribution

### Virtual Nodes Strategy

**Problem**: Simple consistent hashing can create **hotspots**—nodes handling disproportionate load.

**Solution**: Each physical node gets multiple positions on the ring using multiple hash functions.

### How Virtual Nodes Work

```
Physical node A → hash1(A), hash2(A), hash3(A) → 3 positions on ring
Physical node B → hash1(B), hash2(B), hash3(B) → 3 positions on ring
Physical node C → hash1(C), hash2(C), hash3(C) → 3 positions on ring
```

**Benefits**:
1. More uniform load distribution
2. Higher-capacity nodes can have more virtual nodes
3. Graceful handling of node failures (load distributed to multiple nodes)

### Example: 3 Nodes with 3 Virtual Nodes Each

```
Ring positions: [0 ---- 25 ---- 50 ---- 75 ---- 100 ---- 125 ---- 150 ---- 175 ---- 200]
                  A1     B1      C1      A2       B2       C2       A3       B3       C3

Request for key "user:1234" → hash = 67 → lands between C1 and A2 → handled by A2
```

### 💡 When to Use Virtual Nodes

| Scenario | Virtual Nodes Count | Reasoning |
|----------|---------------------|-----------|
| **Homogeneous cluster** | 100-150 per node | Standard distribution |
| **Heterogeneous hardware** | Proportional to capacity | 2x RAM → 2x virtual nodes |
| **Small cluster (<10 nodes)** | 150-200 per node | Compensate for fewer nodes |
| **Large cluster (>100 nodes)** | 50-100 per node | Already good distribution |

### Traditional vs Consistent Hashing

| Aspect | Traditional Hashing | Consistent Hashing |
|--------|--------------------|--------------------|
| **Keys moved when adding node** | ~N/(N+1) keys | ~1/N keys |
| **Keys moved when removing node** | ~N/(N-1) keys | ~1/(N-1) keys |
| **Hotspot risk** | Low | High (without virtual nodes) |
| **Implementation complexity** | Simple | Moderate |
| **Load distribution** | Perfect | Requires virtual nodes |

### ⚠️ Common Pitfalls

1. **Too few virtual nodes**
   - **Problem**: Uneven load distribution, hotspots
   - **Solution**: Use 100-150 virtual nodes per physical node

2. **Ignoring node capacity differences**
   - **Problem**: Treating all nodes equally when hardware differs
   - **Solution**: Assign virtual nodes proportional to capacity

3. **Static virtual node count**
   - **Problem**: Can't adapt to changing load patterns
   - **Solution**: Monitor load, adjust virtual node count dynamically

### 📝 Quick Reference

**One-liner**: Hash ring with virtual nodes for minimal key movement during cluster changes  
**Virtual nodes sweet spot**: 100-150 per physical node  
**Primary benefit**: Adding/removing nodes affects only neighbors  
**Real-world usage**: Cassandra, DynamoDB, Riak, Chord DHT

---

<!-- TOC --><a name="152-merkle-tree"></a>
### 1.5.2 Merkle Tree

> **📌 Quick Summary**: Hash tree enabling efficient detection and repair of replica inconsistencies

![image](https://github.com/user-attachments/assets/b1fcfa70-5e0b-4658-888c-52cec3b60e8a)

### The Problem

Distributed systems with replicated data need to detect inconsistencies efficiently. Comparing entire datasets is slow and wasteful for large key-value stores.

### How Merkle Trees Work

A **Merkle tree** (hash tree) organizes data hierarchically:

1. **Leaves**: Hash of individual key-value pairs
2. **Internal nodes**: Hash of concatenated child hashes
3. **Root**: Hash representing entire dataset

### Synchronization Workflow

```
Step 1: Compare root hashes
   Node A root: 5f4d   Node B root: 5f4d   → ✅ Replicas in sync

Step 2: If roots differ, compare children
   Node A: [2a3b, 8c9d]   Node B: [2a3b, 9f1e]   → Right branch differs

Step 3: Recurse into differing branch
   Continue until reaching differing leaves

Step 4: Exchange only the inconsistent keys
   Only keys in differing branches are transferred
```

### 🎯 Key Concepts

- **Anti-entropy**: Process of ensuring all replicas eventually converge to same state
- **Incremental verification**: Check subtrees independently without full scan
- **Minimal data transfer**: Only exchange hashes and differing keys
- **Logarithmic complexity**: O(log N) comparisons to find differences

### Merkle Tree Structure Example

```
                    Root: hash(A||B)
                   /                \
            A: hash(C||D)        B: hash(E||F)
           /          \          /          \
       C: hash(K1) D: hash(K2) E: hash(K3) F: hash(K4)
         |            |          |            |
       Key1        Key2        Key3         Key4
```

### Advantages vs Disadvantages

| Advantages ✅ | Disadvantages ❌ |
|---------------|------------------|
| **Fast inconsistency detection** (O(log N) vs O(N)) | **Tree recalculation** when nodes join/leave |
| **Minimal data transfer** (only differing keys) | **Storage overhead** for hash tree |
| **Independent branch verification** (parallel checks) | **Complexity** in implementation |
| **Reduced disk I/O** during sync | **Multiple key ranges** affected per node change |

### 💡 When to Use Merkle Trees

✅ **Good for**:
- Distributed databases with replicas (Cassandra, DynamoDB)
- Version control systems (Git uses Merkle trees)
- Peer-to-peer networks (Bitcoin, IPFS)
- Anti-entropy repairs in eventually consistent systems

❌ **Not ideal for**:
- Small datasets (overhead not justified)
- Strongly consistent systems (don't need anti-entropy)
- Frequently changing node membership (expensive recalculations)

### Real-World Examples

| System | Usage |
|--------|-------|
| **Amazon DynamoDB** | Replica synchronization across regions |
| **Apache Cassandra** | Anti-entropy repair between nodes |
| **Bitcoin** | Transaction verification in blocks |
| **Git** | Content-addressable storage, commit history |
| **IPFS** | Content addressing in distributed filesystem |

### Merkle Trees in Cassandra

Cassandra uses Merkle trees for **nodetool repair**:
1. Each node builds Merkle tree for its token range
2. Neighboring replicas exchange tree hashes
3. Only inconsistent ranges are identified and repaired
4. Trees are recalculated periodically (weekly typical)

### ⚠️ Common Pitfalls

1. **Rebuilding trees too frequently**
   - **Problem**: High CPU cost for recalculation
   - **Solution**: Schedule rebuilds during low-traffic periods

2. **Not tuning tree depth**
   - **Problem**: Too shallow → large repair ranges; too deep → overhead
   - **Solution**: Balance depth based on dataset size (typically 15-20 levels)

3. **Ignoring network costs**
   - **Problem**: Many small hash exchanges can be inefficient
   - **Solution**: Batch hash comparisons, use compression

### 📝 Quick Reference

**One-liner**: Hash tree that finds replica differences in O(log N) comparisons  
**Primary benefit**: Minimize data transfer during synchronization  
**Typical usage**: Anti-entropy in Cassandra, DynamoDB, distributed filesystems  
**Tree depth**: 15-20 levels for most use cases  
**Recalculation cost**: Schedule during off-peak hours

---

<!-- TOC --><a name="14-database"></a>
## 1.4 Database Selection

> **📌 Quick Summary**: Choosing between SQL and NoSQL databases based on consistency, scalability, and data structure requirements  
> **Use Cases**: All data-driven applications | **Complexity**: ⭐⭐⭐⭐☆

### SQL vs NoSQL: The Fundamental Choice

The database selection is one of the most critical architectural decisions. The choice between **relational (SQL)** and **non-relational (NoSQL)** databases impacts scalability, consistency, and development complexity.

### 🎯 Key Concepts

- **RDBMS**: Relational database with structured schema, ACID transactions, SQL queries
- **NoSQL**: Non-relational database optimized for scalability, flexible schema
- **Impedance mismatch**: Complexity of mapping object-oriented code to relational tables
- **Horizontal scaling**: Adding more machines vs vertical scaling (bigger machine)
- **CAP theorem**: Can only guarantee 2 of 3: Consistency, Availability, Partition tolerance

### SQL vs NoSQL Comparison

| Dimension | SQL (RDBMS) | NoSQL |
|-----------|-------------|-------|
| **Schema** | Fixed, predefined | Flexible, dynamic |
| **Scalability** | Vertical (scale up) | Horizontal (scale out) |
| **Transactions** | ACID compliant | Eventually consistent (typically) |
| **Joins** | Complex joins supported | Limited or no joins |
| **Use Case** | Complex queries, relationships | High throughput, simple queries |
| **Cost** | Expensive licenses | Often open source |
| **Data Model** | Tables with rows | Documents, key-value, graph, column |

![image](https://github.com/user-attachments/assets/feecf1b2-efe1-4beb-b9d0-6ee735e9fd29)

### 💡 NoSQL Advantages

**When NoSQL Excels**:
- ✅ **Simple design**: No impedance mismatch—store data as documents vs multiple joined tables
- ✅ **Horizontal scaling**: Distribute data across clusters easily as user count grows
- ✅ **High availability**: Node replacement without downtime, automatic replication
- ✅ **Flexible schema**: Handle unstructured/semi-structured data (JSON, XML, BSON)
- ✅ **Cost-effective**: Open source options, runs on commodity hardware

**Example**: Employee data in one document vs 5 joined tables → simpler code, easier maintenance

### SQL Advantages

**When SQL Excels**:
- ✅ **ACID transactions**: Strong consistency guarantees
- ✅ **Complex queries**: Rich query language with joins, aggregations
- ✅ **Data integrity**: Foreign keys, constraints enforce correctness
- ✅ **Mature ecosystem**: 40+ years of tooling, optimization
- ✅ **Strong typing**: Schema validation catches errors early

### Decision Framework

| Requirement | Choose SQL | Choose NoSQL |
|-------------|------------|--------------|
| **Data structure** | Highly structured, normalized | Unstructured, denormalized |
| **Scalability need** | Moderate (10K-100K QPS) | High (>100K QPS) |
| **Consistency** | Strong consistency required | Eventual consistency acceptable |
| **Query complexity** | Complex joins, aggregations | Simple key-based lookups |
| **Schema changes** | Rare, planned | Frequent, unpredictable |
| **Budget** | Sufficient for licenses | Cost-conscious |

### ⚖️ Replication Trade-offs

**Synchronous Replication**:
- ✅ **Pros**: Strong consistency, data durability
- ❌ **Cons**: Higher latency, reduced availability

**Asynchronous Replication**:
- ✅ **Pros**: Low latency, high availability
- ❌ **Cons**: Eventual consistency, potential data loss

### 💼 Real-World Example: Financial Trading Platform

**Scenario**: Global real-time trading platform demanding <10ms latency

**Requirements**:
- Ultra-low latency for split-second decisions
- Global distribution
- High throughput (millions of trades/day)
- Eventual consistency acceptable

**Recommended Approach**:
- ✅ **Asynchronous replication**: Update primary first, replicate later
- ✅ **NoSQL database**: Cassandra or ScyllaDB for horizontal scalability
- ✅ **Multi-region**: Deploy close to trading hubs (NYC, London, Tokyo, Singapore)
- ✅ **Caching layer**: Redis for sub-millisecond reads

**Why asynchronous over synchronous?**
- Primary node responds immediately without waiting for secondary acknowledgments
- Reduces latency from ~50ms (sync) to <5ms (async)
- Trade-off: Replicas may lag by milliseconds, acceptable for this use case
- Business priority: Speed > perfect consistency

### ⚠️ Common Pitfalls

1. **Choosing NoSQL for strong consistency needs**
   - **Problem**: NoSQL typically offers eventual consistency
   - **Solution**: Use SQL or NewSQL (CockroachDB, Spanner) for ACID requirements

2. **Over-normalizing in NoSQL**
   - **Problem**: Applying SQL normalization principles to NoSQL
   - **Solution**: Denormalize, duplicate data for query efficiency

3. **Ignoring data access patterns**
   - **Problem**: Designing schema without understanding queries
   - **Solution**: Design schema based on read/write patterns first

4. **Premature NoSQL adoption**
   - **Problem**: Choosing NoSQL for "scale" before needed
   - **Solution**: Start with SQL, migrate to NoSQL when scaling limits hit

### Popular Database Choices

| Type | Database | Best For | Scale |
|------|----------|----------|-------|
| **SQL** | PostgreSQL | Complex queries, integrity | Moderate |
| **SQL** | MySQL | Web applications, read-heavy | Moderate |
| **Document** | MongoDB | Flexible schema, documents | High |
| **Key-Value** | Redis | Caching, real-time | Very High |
| **Column** | Cassandra | Time-series, high writes | Massive |
| **Graph** | Neo4j | Relationships, social networks | Moderate |

### 📝 Quick Reference

**One-liner**: SQL for structure and consistency, NoSQL for scale and flexibility  
**Default choice**: PostgreSQL for most applications until scale demands NoSQL  
**Scale threshold**: Consider NoSQL at >100K QPS or >10TB data  
**Hybrid approach**: Use both—SQL for transactional data, NoSQL for analytics/caching

---

<!-- TOC --><a name="15-key-value-store"></a>
## 1.5 Key-Value Store

> **📌 Quick Summary**: Distributed hash table for storing key-value pairs with consistent hashing and Merkle trees for synchronization  
> **Use Cases**: Caching, session storage, distributed systems | **Complexity**: ⭐⭐⭐⭐⭐

### Overview

Key-value stores are fundamental building blocks of distributed systems, providing simple `get(key)` and `put(key, value)` operations. Examples include **Redis**, **DynamoDB**, **Cassandra**, and **Riak**.

### 🎯 Key Design Challenges

1. **Load distribution**: How to distribute keys evenly across nodes?
2. **Replication**: How to keep replicas synchronized?
3. **Hotspots**: How to avoid overloading single nodes?
4. **Consistency**: How to detect and resolve data inconsistencies?

---

<!-- TOC --><a name="151consistent-hashing"></a>
### 1.5.1 Consistent Hashing

> **📌 Quick Summary**: Hash function that minimizes key remapping when nodes join/leave the cluster

### The Problem

Traditional hashing (`hash(key) % N nodes`) requires remapping most keys when nodes change. For 1 billion keys and 100 nodes, adding 1 node requires moving ~990 million keys.

### The Solution: Hash Ring

**Consistent hashing** maps both keys and nodes to a circular hash space (0 to 2^160-1). Keys are assigned to the next node found clockwise on the ring.

**Benefits**:
- Adding/removing nodes only affects adjacent nodes
- Minimal key movement (approximately `keys/N` instead of `most keys`)
- Predictable load distribution

### Virtual Nodes Strategy

**Problem**: Simple consistent hashing can create **hotspots**—nodes handling disproportionate load.

**Solution**: Each physical node gets multiple positions on the ring using multiple hash functions.

### How Virtual Nodes Work

```
Physical node A → hash1(A), hash2(A), hash3(A) → 3 positions on ring
Physical node B → hash1(B), hash2(B), hash3(B) → 3 positions on ring
Physical node C → hash1(C), hash2(C), hash3(C) → 3 positions on ring
```

**Benefits**:
1. More uniform load distribution
2. Higher-capacity nodes can have more virtual nodes
3. Graceful handling of node failures (load distributed to multiple nodes)

### Example: 3 Nodes with 3 Virtual Nodes Each

```
Ring positions: [0 ---- 25 ---- 50 ---- 75 ---- 100 ---- 125 ---- 150 ---- 175 ---- 200]
                  A1     B1      C1      A2       B2       C2       A3       B3       C3

Request for key "user:1234" → hash = 67 → lands between C1 and A2 → handled by A2
```

### 💡 When to Use Virtual Nodes

| Scenario | Virtual Nodes Count | Reasoning |
|----------|---------------------|-----------|
| **Homogeneous cluster** | 100-150 per node | Standard distribution |
| **Heterogeneous hardware** | Proportional to capacity | 2x RAM → 2x virtual nodes |
| **Small cluster (<10 nodes)** | 150-200 per node | Compensate for fewer nodes |
| **Large cluster (>100 nodes)** | 50-100 per node | Already good distribution |

### Traditional vs Consistent Hashing

| Aspect | Traditional Hashing | Consistent Hashing |
|--------|--------------------|--------------------|
| **Keys moved when adding node** | ~N/(N+1) keys | ~1/N keys |
| **Keys moved when removing node** | ~N/(N-1) keys | ~1/(N-1) keys |
| **Hotspot risk** | Low | High (without virtual nodes) |
| **Implementation complexity** | Simple | Moderate |
| **Load distribution** | Perfect | Requires virtual nodes |

### ⚠️ Common Pitfalls

1. **Too few virtual nodes**
   - **Problem**: Uneven load distribution, hotspots
   - **Solution**: Use 100-150 virtual nodes per physical node

2. **Ignoring node capacity differences**
   - **Problem**: Treating all nodes equally when hardware differs
   - **Solution**: Assign virtual nodes proportional to capacity

3. **Static virtual node count**
   - **Problem**: Can't adapt to changing load patterns
   - **Solution**: Monitor load, adjust virtual node count dynamically

### 📝 Quick Reference

**One-liner**: Hash ring with virtual nodes for minimal key movement during cluster changes  
**Virtual nodes sweet spot**: 100-150 per physical node  
**Primary benefit**: Adding/removing nodes affects only neighbors  
**Real-world usage**: Cassandra, DynamoDB, Riak, Chord DHT

---

<!-- TOC --><a name="152-merkle-tree"></a>
### 1.5.2 Merkle Tree

> **�� Quick Summary**: Hash tree enabling efficient detection and repair of replica inconsistencies

![image](https://github.com/user-attachments/assets/b1fcfa70-5e0b-4658-888c-52cec3b60e8a)

### The Problem

Distributed systems with replicated data need to detect inconsistencies efficiently. Comparing entire datasets is slow and wasteful for large key-value stores.

### How Merkle Trees Work

A **Merkle tree** (hash tree) organizes data hierarchically:

1. **Leaves**: Hash of individual key-value pairs
2. **Internal nodes**: Hash of concatenated child hashes
3. **Root**: Hash representing entire dataset

### Synchronization Workflow

```
Step 1: Compare root hashes
   Node A root: 5f4d   Node B root: 5f4d   → ✅ Replicas in sync

Step 2: If roots differ, compare children
   Node A: [2a3b, 8c9d]   Node B: [2a3b, 9f1e]   → Right branch differs

Step 3: Recurse into differing branch
   Continue until reaching differing leaves

Step 4: Exchange only the inconsistent keys
   Only keys in differing branches are transferred
```

### 🎯 Key Concepts

- **Anti-entropy**: Process of ensuring all replicas eventually converge to same state
- **Incremental verification**: Check subtrees independently without full scan
- **Minimal data transfer**: Only exchange hashes and differing keys
- **Logarithmic complexity**: O(log N) comparisons to find differences

### Merkle Tree Structure Example

```
                    Root: hash(A||B)
                   /                \
            A: hash(C||D)        B: hash(E||F)
           /          \          /          \
       C: hash(K1) D: hash(K2) E: hash(K3) F: hash(K4)
         |            |          |            |
       Key1        Key2        Key3         Key4
```

### Advantages vs Disadvantages

| Advantages ✅ | Disadvantages ❌ |
|---------------|------------------|
| **Fast inconsistency detection** (O(log N) vs O(N)) | **Tree recalculation** when nodes join/leave |
| **Minimal data transfer** (only differing keys) | **Storage overhead** for hash tree |
| **Independent branch verification** (parallel checks) | **Complexity** in implementation |
| **Reduced disk I/O** during sync | **Multiple key ranges** affected per node change |

### 💡 When to Use Merkle Trees

✅ **Good for**:
- Distributed databases with replicas (Cassandra, DynamoDB)
- Version control systems (Git uses Merkle trees)
- Peer-to-peer networks (Bitcoin, IPFS)
- Anti-entropy repairs in eventually consistent systems

❌ **Not ideal for**:
- Small datasets (overhead not justified)
- Strongly consistent systems (don't need anti-entropy)
- Frequently changing node membership (expensive recalculations)

### Real-World Examples

| System | Usage |
|--------|-------|
| **Amazon DynamoDB** | Replica synchronization across regions |
| **Apache Cassandra** | Anti-entropy repair between nodes |
| **Bitcoin** | Transaction verification in blocks |
| **Git** | Content-addressable storage, commit history |
| **IPFS** | Content addressing in distributed filesystem |

### Merkle Trees in Cassandra

Cassandra uses Merkle trees for **nodetool repair**:
1. Each node builds Merkle tree for its token range
2. Neighboring replicas exchange tree hashes
3. Only inconsistent ranges are identified and repaired
4. Trees are recalculated periodically (weekly typical)

### ⚠️ Common Pitfalls

1. **Rebuilding trees too frequently**
   - **Problem**: High CPU cost for recalculation
   - **Solution**: Schedule rebuilds during low-traffic periods

2. **Not tuning tree depth**
   - **Problem**: Too shallow → large repair ranges; too deep → overhead
   - **Solution**: Balance depth based on dataset size (typically 15-20 levels)

3. **Ignoring network costs**
   - **Problem**: Many small hash exchanges can be inefficient
   - **Solution**: Batch hash comparisons, use compression

### 📝 Quick Reference

**One-liner**: Hash tree that finds replica differences in O(log N) comparisons  
**Primary benefit**: Minimize data transfer during synchronization  
**Typical usage**: Anti-entropy in Cassandra, DynamoDB, distributed filesystems  
**Tree depth**: 15-20 levels for most use cases  
**Recalculation cost**: Schedule during off-peak hours

---

<!-- TOC --><a name="2-content-delivery-network-cdn"></a>
# 2. Content Delivery Network (CDN)

> **📌 Quick Summary**: Globally distributed network of proxy servers that cache and deliver content closer to end users  
> **Use Cases**: Video streaming, static assets, software downloads | **Complexity**: ⭐⭐⭐⭐⭐

![image](https://github.com/user-attachments/assets/a258e1d0-0278-4c5c-8276-dd416f6e4fa4)

### Why CDN?

**The Problem**: Data-intensive applications face challenges over long distances:
- 🌐 **Network path complexity**: Traffic traverses multiple ISPs with varying characteristics
- 📉 **Reduced throughput**: Smaller MTU (Message Transmission Unit) links limit bandwidth
- 🔄 **Redundant transfers**: Origin server sends same content to millions of users individually
- ⏱️ **High latency**: Geographical distance increases round-trip time
- 🔥 **Origin server bottleneck**: Single point becomes overwhelmed at scale

**The Solution**: Distribute content to edge locations near users, reducing latency and origin load.

### 🎯 Key Concepts

- **Edge server**: Proxy server at the CDN edge, closest to users
- **Origin server**: Source of truth for content, updated by content providers
- **Cache hit ratio**: Percentage of requests served from cache vs origin
- **TTL (Time To Live)**: How long content stays cached before refresh
- **Purge/Invalidation**: Manually removing stale content from cache

---

## CDN Architecture Components

![image](https://github.com/user-attachments/assets/33b90165-2d44-44e1-8d0a-72e684fe5168)

### Component Responsibility Table

| Component | Responsibility | Technology Examples | Scale |
|-----------|----------------|---------------------|-------|
| **Clients** | Request content from CDN | Browsers, mobile apps, IoT devices | Billions |
| **Routing System** | Direct clients to nearest edge | DNS, Anycast, GeoDNS | Global |
| **Scrubber Servers** | Filter malicious traffic | DDoS protection, WAF | As needed |
| **Proxy/Edge Servers** | Serve cached content from RAM | NGINX, Varnish | Thousands |
| **Distribution System** | Push content to edge servers | Multicast, P2P protocols | Regional |
| **Origin Servers** | Source of truth for content | Web servers, object storage | Hundreds |
| **Management System** | Monitor metrics & billing | Prometheus, custom analytics | Centralized |

### Component Details

#### 1. Clients
End users accessing content through various devices: browsers, smartphones, smart TVs, gaming consoles.

#### 2. Routing System
**Purpose**: Direct each user to the optimal edge server based on:
- 📍 Geographic proximity (lowest latency)
- 📊 Server load (avoid overloaded nodes)
- 🌐 Network conditions (congestion, available bandwidth)
- 📦 Content availability (which edge has the requested content)

**Implementation**: See section 2.2 DNS Redirection for details.

#### 3. Scrubber Servers
**Purpose**: Security layer that filters traffic during attacks
- **When active**: Triggered upon attack detection
- **Function**: Separate legitimate traffic from malicious requests
- **Protection**: DDoS mitigation, bot filtering, rate limiting
- **Routing**: Clean traffic forwarded to edge servers

#### 4. Proxy/Edge Servers
**Core function**: Serve content directly to users
- ⚡ **Hot data**: Stored in RAM for <5ms access time
- 💾 **Cold data**: Less popular content on SSD/HDD
- 📊 **Accounting**: Track requests, bandwidth, cache hits
- 🔄 **Content updates**: Receive from distribution system

**Performance characteristics**:
- RAM cache: 1-5ms latency
- SSD cache: 10-20ms latency
- Origin fallback: 100-500ms latency

#### 5. Distribution System
**Purpose**: Efficiently propagate content to all edge servers
- 🌳 **Tree structure**: Hierarchical distribution to reduce origin load
- 📡 **Broadcast-like**: Push updates to multiple edges simultaneously
- 🔄 **Pull on demand**: Edge fetches content when first requested
- 📦 **Chunked transfer**: Large files distributed in pieces

#### 6. Origin Servers
**Purpose**: Authoritative source for content
- 📤 **Content upload**: Receive new/updated content from providers
- 🔍 **Metadata storage**: File info, versions, access controls
- 🆘 **Cache miss fallback**: Serve content when not in CDN cache
- 🔧 **Dynamic content**: Generate personalized responses

#### 7. Management System
**Purpose**: Operations, monitoring, and business analytics
- 📈 **Metrics**: Latency, cache hit ratio, bandwidth, error rates
- 💰 **Billing**: Track usage for third-party CDN customers
- 🚨 **Alerts**: Notify on performance degradation or outages
- 📊 **Analytics**: Geographic distribution, popular content

---

<!-- TOC --><a name="21-multi-tier-cdn-architecture"></a>
## 2.1 Multi-tier CDN Architecture

> **📌 Quick Summary**: Hierarchical tree structure for efficient content distribution from origin to edge servers

### The Challenge

Distributing content from origin to **thousands of edge servers simultaneously** creates massive load. If origin server directly updates all edges, it becomes a bottleneck.

### The Solution: Tree Hierarchy

```
                        Origin Server (Root)
                              |
            +-----------------+-----------------+
            |                 |                 |
     Regional Hub 1    Regional Hub 2    Regional Hub 3
        (Parent)          (Parent)          (Parent)
            |                 |                 |
    +-------+-------+   +-----+-----+     +-----+-----+
    |       |       |   |     |     |     |     |     |
  Edge1  Edge2  Edge3 Edge4 Edge5 Edge6 Edge7 Edge8 Edge9
```

### How It Works

1. **Origin → Regional Hubs**: Content pushed to handful of regional parent nodes
2. **Hubs → Edge Servers**: Regional hubs distribute to their child edges
3. **Parallel distribution**: Multiple paths reduce single-point bottleneck
4. **Load spreading**: Each node only manages its direct children

### 💡 Benefits

- ✅ **Reduced origin load**: Origin serves 10s of hubs, not 1000s of edges
- ✅ **Faster propagation**: Parallel distribution paths
- ✅ **Scalability**: Add more edges without increasing origin burden
- ✅ **Resilience**: Tree redundancy handles node failures

### Long-Tail Content Handling

**Research insight**: Content popularity follows **long-tail distribution**
- 📊 **Head**: Small number of very popular items (viral videos, trending news)
- 📉 **Tail**: Large number of less popular items (niche content)

**Multi-layer cache strategy**:
- **Layer 1 (Edge)**: Cache only hot content (top 10-20%)
- **Layer 2 (Regional)**: Cache moderately popular content (next 30%)
- **Layer 3 (Origin)**: Store everything, serve rarely-accessed tail

**Trade-off**: 
- Edge storage cost vs cache hit ratio
- Optimal: 80%+ requests served from edge, 15% from regional, 5% from origin

---

<!-- TOC --><a name="22-dns-redirection"></a>
## 2.2 DNS Redirection

> **📌 Quick Summary**: Two-step process to route clients to optimal edge server using DNS

### How DNS Redirection Works

#### Step 1: Geographic Mapping
Map client to appropriate network location (nearest CDN data center)
- Uses client's IP address geolocation
- Considers network topology and latency
- Returns IP of nearby CDN facility

#### Step 2: Load Distribution
Within selected facility, distribute load across proxy servers
- Round-robin or least-connections algorithm
- Health checks ensure only healthy servers receive traffic
- Session affinity if needed (sticky sessions)

### 🎯 Factors Considered

1. **Network distance**: Geographic proximity, round-trip time
2. **Server load**: Current requests per server, CPU usage
3. **Content availability**: Which edges have cached the content
4. **Network conditions**: Congestion, packet loss, bandwidth

### DNS Redirection Flow

```
1. User requests: video.example.com
2. DNS resolver queries CDN nameserver
3. CDN evaluates:
   - User location (IP geolocation)
   - Edge server loads
   - Content availability
4. CDN returns IP: edge-seattle.cdn.example.com (closest, least loaded)
5. User connects directly to Seattle edge server
6. Edge serves content from cache (or fetches from origin)
```

### Benefits

- ✅ **Reduced latency**: Users connect to nearby edge
- ✅ **Load balancing**: Distribute across multiple servers
- ✅ **Automatic failover**: DNS returns healthy servers only
- ✅ **Global reach**: Works across continents

### 📝 Quick Reference

**Typical latency reduction**: 200-500ms (origin) → 20-50ms (edge)  
**DNS TTL**: 60-300 seconds (balance between flexibility and DNS load)  
**Fallback**: Origin server serves if all edges unavailable

---

<!-- TOC --><a name="24-distributed-cache"></a>
## 2.4 Distributed Cache

> **📌 Quick Summary**: Horizontally scaled caching system to handle massive data volumes with low latency  
> **Use Cases**: Session storage, API responses, database query results | **Complexity**: ⭐⭐⭐⭐☆

### Why Distributed Cache?

**Problem**: Single-server cache limitations at scale
- ❌ **Single Point of Failure (SPOF)**: One server down = entire cache lost
- ❌ **Capacity limits**: RAM constraints limit cacheable data size
- ❌ **Performance bottleneck**: One server can't handle millions of QPS
- ❌ **Data coupling**: Sensitive data from different layers mixed together

**Solution**: Distribute cache across multiple servers (shards)

### 🎯 Distribution Benefits

1. **Layer-specific caching**: Each system layer has its own cache
   - Web tier: Session data, user preferences
   - Application tier: Business logic results, computed values
   - Database tier: Query results, row-level data

2. **Reduced latency**: Cache closer to where data is needed
3. **Fault tolerance**: Multiple servers provide redundancy
4. **Horizontal scalability**: Add more cache servers as needed

---

<!-- TOC --><a name="241-internals-of-cache-server"></a>
### 2.4.1 Internals of Cache Server

> **📌 Quick Summary**: Three core mechanisms for efficient caching: hash map, doubly linked list, eviction policy

### Architecture Components

#### 1. Hash Map (O(1) lookup)
**Purpose**: Fast key-value lookups
- Stores pointers to cached values
- Constant-time access on average
- Handles collision with chaining or open addressing

**Example**:
```
HashMap<String, Pointer>
"user:12345" → Pointer to value in RAM
"product:789" → Pointer to value in RAM
```

#### 2. Doubly Linked List (O(1) updates)
**Purpose**: Maintain access order for eviction
- Most recently used (MRU) at head
- Least recently used (LRU) at tail
- Constant-time insert/delete operations

**Structure**:
```
HEAD <-> [Node: user:999] <-> [Node: product:123] <-> [Node: user:456] <-> TAIL
         (Most recent)                                      (Least recent)
```

#### 3. Eviction Policy
**Purpose**: Remove entries when cache is full

**LRU (Least Recently Used)** - Most common:
- Evict items not accessed for longest time
- Assumption: Recently used data likely to be used again
- Implementation: Move accessed items to list head

**Alternative policies**:
- **LFU** (Least Frequently Used): Evict items accessed fewest times
- **FIFO** (First In First Out): Evict oldest items regardless of access
- **TTL** (Time To Live): Evict after fixed duration
- **Random**: Evict random item (simple, surprisingly effective)

### Cache Operation Flow

**GET operation**:
```
1. Hash key to find bucket in HashMap
2. Retrieve value pointer (O(1))
3. Move node to head of linked list (mark as recently used)
4. Return value to client
```

**PUT operation**:
```
1. Check if key exists in HashMap
2. If exists: Update value, move to list head
3. If not exists:
   - Check capacity
   - If full: Evict LRU item (remove tail node)
   - Add new entry to HashMap
   - Insert new node at list head
```

---

<!-- TOC --><a name="242-high-performance"></a>
### 2.4.2 High Performance Characteristics

> **�� Quick Summary**: Multiple optimizations combine for sub-millisecond cache performance

### Performance Optimizations

| Optimization | Time Complexity | Impact |
|--------------|-----------------|--------|
| **Consistent hashing** | O(log N) | Find cache shard efficiently |
| **Hash table lookup** | O(1) average | Locate key within shard |
| **LRU updates** | O(1) | Access/update cache entries |
| **TCP/UDP protocols** | <1ms local | Fast client-server communication |
| **Replication** | N/A | Distribute load, handle failures |
| **RAM storage** | <1µs | Ultra-low latency access |

### Performance Metrics

- **Latency**: 0.5-5ms typical (vs 50-100ms database)
- **Throughput**: 100K+ QPS per server
- **Cache hit ratio**: 80-95% typical
- **Memory efficiency**: ~100 bytes overhead per entry

### 🎯 Key Design Decisions

1. **Consistent hashing for sharding**
   - Minimal key redistribution when adding/removing servers
   - Virtual nodes for even load distribution
   - O(log N) lookup time acceptable for cache use case

2. **RAM-only storage**
   - 1000x faster than SSD
   - Volatile but acceptable for cache (not source of truth)
   - Cost-effective for hot data

3. **Replication for reliability**
   - Typically 2-3 replicas per shard
   - Read operations distributed across replicas
   - Reduces single-server overload
   - Provides fault tolerance

4. **Asynchronous replication**
   - Primary responds immediately
   - Replicas updated in background
   - Trade-off: Eventual consistency for lower latency

---

<!-- TOC --><a name="243-memcached-vs-redis-feature-comparison"></a>
### 2.4.3 Memcached vs. Redis: Feature Comparison

> **📌 Quick Summary**: Two popular distributed cache solutions with different trade-offs

### Feature Comparison Matrix

| Feature | Memcached | Redis | Winner |
|---------|-----------|-------|--------|
| **Low latency** | ✅ Yes | ✅ Yes | Tie |
| **Persistence** | ⚠️ 3rd-party tools | ✅ Built-in (RDB, AOF) | **Redis** |
| **Multilanguage support** | ✅ Yes | ✅ Yes | Tie |
| **Data sharding** | ⚠️ 3rd-party | ✅ Built-in (Cluster mode) | **Redis** |
| **Ease of use** | ✅ Simple | ✅ Simple | Tie |
| **Multithreading** | ✅ Yes | ❌ No (single-threaded) | **Memcached** |
| **Data structures** | Objects only | Lists, Sets, Sorted Sets, Hashes, Streams | **Redis** |
| **Transactions** | ❌ No | ✅ Yes (MULTI/EXEC) | **Redis** |
| **Eviction policies** | LRU only | LRU, LFU, Random, TTL, volatile-* | **Redis** |
| **Lua scripting** | ❌ No | ✅ Yes | **Redis** |
| **Geospatial support** | ❌ No | ✅ Yes (GEOADD, GEORADIUS) | **Redis** |
| **Pub/Sub messaging** | ❌ No | ✅ Yes | **Redis** |
| **Memory efficiency** | High | Moderate | **Memcached** |
| **Performance (reads)** | ~1M ops/sec | ~100K ops/sec | **Memcached** |

### 💡 When to Choose

#### Choose Memcached when:
- ✅ **Pure caching**: Simple key-value storage, no complex data structures
- ✅ **Maximum throughput**: Need highest possible ops/sec
- ✅ **Multithreading**: Want to utilize all CPU cores
- ✅ **Memory efficiency**: Limited RAM, need maximum cache entries
- ✅ **Simple use case**: No need for persistence or advanced features

#### Choose Redis when:
- ✅ **Rich data structures**: Need lists, sets, sorted sets, hashes
- ✅ **Persistence required**: Cache should survive restarts
- ✅ **Pub/Sub messaging**: Real-time event distribution
- ✅ **Transactions**: Need atomic multi-key operations
- ✅ **Lua scripting**: Complex server-side logic
- ✅ **Geospatial queries**: Location-based features
- ✅ **Flexible eviction**: Need LFU or custom policies

### ⚖️ Trade-offs

**Memcached**:
- ✅ **Pros**: Higher throughput, multi-threaded, memory efficient
- ❌ **Cons**: Limited features, no persistence, objects only

**Redis**:
- ✅ **Pros**: Feature-rich, persistence, data structures, scripting
- ❌ **Cons**: Single-threaded, higher memory overhead, lower peak throughput

### Real-World Usage

| Company | Choice | Reasoning |
|---------|--------|-----------|
| **Facebook** | Memcached | Pure caching, maximum throughput (billions of requests) |
| **Twitter** | Redis | Timelines (sorted sets), pub/sub for real-time updates |
| **GitHub** | Redis | Job queues (lists), background processing |
| **Pinterest** | Redis | Feed storage (sorted sets), geospatial queries |
| **Stack Overflow** | Redis | Leaderboards (sorted sets), session storage |

### 📝 Quick Reference

**Default choice**: Redis for most use cases (richer features, minimal performance difference)  
**Memcached niche**: Pure caching at extreme scale (>1M QPS per server)  
**Hybrid approach**: Use both—Memcached for hot paths, Redis for complex operations  
**Migration path**: Start with Redis, move hot data to Memcached if needed

---

<!-- TOC --><a name="3-rate-limiter"></a>
# 3. Rate Limiter

> **📌 Quick Summary**: Controls request rate to prevent system overload and abuse  
> **Use Cases**: API protection, DDoS mitigation, resource management | **Complexity**: ⭐⭐⭐⭐☆

![image](https://github.com/user-attachments/assets/eecc87db-84e4-4e63-a96a-dea8a8479bab)

### Why Rate Limiting?

**Purpose**: Protect system resources from overload and abuse
- 🛡️ **Prevent abuse**: Block malicious users, bots, DDoS attacks
- ⚡ **Resource management**: Ensure fair resource allocation across users
- 💰 **Cost control**: Limit expensive operations (API calls, database queries)
- 📊 **SLA enforcement**: Implement tiered service levels (free vs paid)
- 🔄 **Graceful degradation**: Maintain service quality under load

### 🎯 Key Concepts

- **Throttling**: Limiting the rate of requests from a client
- **Token bucket**: Algorithm storing tokens that replenish over time
- **Sliding window**: Time-based window that moves continuously
- **Rate limit**: Maximum requests allowed per time unit (e.g., 100 req/min)
- **Quota**: Total resource allocation over a period (e.g., 10K API calls/month)

---

## Rate Limiting Algorithms

### 1. Token Bucket Algorithm

> **📌 Most Popular**: Used by Amazon, Stripe, Google Cloud

**How it works**:
```
1. Bucket holds tokens (capacity: N)
2. Tokens added at fixed rate (R tokens/sec)
3. Each request consumes 1 token
4. If bucket empty → request rejected
5. If tokens available → request proceeds, token consumed
```

**Example**: Bucket capacity = 10, refill rate = 2 tokens/sec
- User makes 10 requests instantly → all succeed (10 tokens consumed)
- Next request immediately → rejected (bucket empty)
- After 5 seconds → 10 more requests possible (10 tokens refilled)

**Characteristics**:
- ✅ **Burst handling**: Allows temporary spikes up to bucket capacity
- ✅ **Smooth rate**: Long-term rate controlled by refill rate
- ✅ **Memory efficient**: O(1) per user
- ⚠️ **Configuration**: Requires tuning bucket size and refill rate

### 2. Leaky Bucket Algorithm

> **📌 Queue-based**: Processes requests at constant rate

**How it works**:
```
1. Requests enter queue (bucket)
2. Processed at fixed rate (leak rate)
3. If queue full → request rejected
4. Otherwise → request queued
```

**Characteristics**:
- ✅ **Constant output rate**: Smooth, predictable processing
- ✅ **Good for batch processing**: Handles bursty traffic gracefully
- ❌ **No burst allowance**: Cannot process multiple requests simultaneously
- ❌ **Queuing delay**: Adds latency to requests

**Difference from Token Bucket**:
- Token bucket: Allows bursts up to capacity
- Leaky bucket: Enforces strict constant rate

### 3. Fixed Window Counter

> **📌 Simplest**: Easy to implement but has edge case issues

**How it works**:
```
1. Time divided into fixed windows (e.g., 1-minute windows)
2. Counter tracks requests in current window
3. If counter < limit → allow request, increment counter
4. If counter >= limit → reject request
5. Counter resets at window boundary
```

**Example**: Limit = 100 requests/minute
- Window 1 (00:00-01:00): 100 requests allowed
- At 01:00: Counter resets to 0
- Window 2 (01:00-02:00): 100 requests allowed

**Problem - Boundary Case**:
```
00:30-00:59 → 100 requests (allowed)
01:00-01:29 → 100 requests (allowed)
Total in 1 minute (00:30-01:30) → 200 requests! (2x limit)
```

**Characteristics**:
- ✅ **Simple implementation**: Single counter per user
- ✅ **Low memory**: O(1) per user
- ❌ **Boundary burst**: 2x traffic possible at window boundaries
- ❌ **Unfair**: Users at window start vs end have different experiences

### 4. Sliding Window Log

> **📌 Most Accurate**: Eliminates boundary issues but higher overhead

**How it works**:
```
1. Store timestamp of each request in sorted log
2. For new request at time T:
   - Remove entries older than (T - window_size)
   - Count remaining entries
   - If count < limit → allow request, add timestamp
   - Otherwise → reject request
```

**Example**: Limit = 100 requests/minute
- Request at 12:00:30
- Check requests between 11:59:30 and 12:00:30
- If < 100 requests in that range → allow

**Characteristics**:
- ✅ **Accurate**: No boundary burst issue
- ✅ **Fair**: True sliding window
- ❌ **High memory**: O(N) where N = requests in window
- ❌ **Expensive**: Must scan/delete old entries per request

### 5. Sliding Window Counter (Hybrid)

> **📌 Best Balance**: Combines fixed window efficiency with sliding window accuracy

**How it works**:
```
1. Maintain counters for current and previous window
2. For request at time T in current window:
   - Calculate overlap with previous window
   - Estimate: (prev_count × overlap%) + curr_count
   - If estimate < limit → allow request
   - Otherwise → reject
```

**Example**: Limit = 100 req/min, window = 1 minute
```
Previous window (00:00-01:00): 80 requests
Current window (01:00-02:00): 30 requests
Request at 01:30 (50% into current window):
  Estimate = 80 × 50% + 30 = 40 + 30 = 70 requests → ALLOW
```

**Characteristics**:
- ✅ **Good approximation**: ~99% accurate
- ✅ **Memory efficient**: O(1) per user (2 counters)
- ✅ **No boundary burst**: Smooth rate limiting
- ⚠️ **Slight inaccuracy**: Assumes uniform distribution

---

## Algorithm Comparison Matrix

| Algorithm | Accuracy | Memory | Burst Support | Complexity | Use Case |
|-----------|----------|--------|---------------|------------|----------|
| **Token Bucket** | Good | O(1) | ✅ Yes | Medium | API gateways, general purpose |
| **Leaky Bucket** | Excellent | O(N) | ❌ No | Medium | Message queues, batch processing |
| **Fixed Window** | Poor | O(1) | ⚠️ At boundaries | Low | Simple rate limiting, non-critical |
| **Sliding Log** | Excellent | O(N) | ✅ Yes | High | Billing, strict enforcement |
| **Sliding Counter** | Very Good | O(1) | ✅ Yes | Medium | **Recommended for most cases** |

![image](https://github.com/user-attachments/assets/be78a775-8195-4dfb-8802-3c194be21692)

---

## 💡 Implementation Decisions

### Where to Implement Rate Limiting?

| Location | Pros | Cons | Best For |
|----------|------|------|----------|
| **Client-side** | Reduces unnecessary requests | Easily bypassed | Cost savings, UX improvement |
| **API Gateway** | Centralized, consistent | Single point of failure | Microservices, multi-service |
| **Application** | Fine-grained control | Must implement everywhere | Specific business logic |
| **Load Balancer** | Early rejection | Limited context | DDoS protection |

**Recommended**: API Gateway + Application hybrid
- Gateway: Coarse-grained limits (per IP, per user)
- Application: Fine-grained limits (per endpoint, per operation)

### Distributed Rate Limiting

**Challenge**: Multiple servers need to share rate limit state

**Solutions**:

1. **Centralized Store (Redis)**
   ```
   Key: user:123:requests
   Value: count
   TTL: window duration
   Operations: INCR (atomic), EXPIRE
   ```
   - ✅ Consistent across servers
   - ✅ Atomic operations
   - ❌ Single point of failure
   - ❌ Network latency

2. **Sticky Sessions**
   - Route same user to same server
   - ✅ No distributed coordination needed
   - ❌ Uneven load distribution
   - ❌ Doesn't work with autoscaling

3. **Rate Limiter Service**
   - Dedicated microservice for rate limiting
   - ✅ Specialized, optimized
   - ✅ Can use local cache + sync
   - ❌ Additional complexity

---

## ⚖️ Trade-offs

**Strict vs Lenient**:
- **Strict**: Reject at exact limit → Can frustrate legitimate users
- **Lenient**: Allow slight overage → Better UX but less protection

**Granularity**:
- **Coarse** (per IP): Simple but can block many users behind NAT
- **Fine** (per user + endpoint): Accurate but higher overhead

**Response Strategy**:
- **Reject (429)**: Clear signal, client can retry with backoff
- **Queue**: Better UX but requires resources, can hide problems
- **Degrade**: Serve cached/simplified response

---

## 💼 Real-World Examples

| Service | Rate Limits | Algorithm | Notes |
|---------|-------------|-----------|-------|
| **Twitter API** | 900 req/15min (user), 450 req/15min (app) | Token bucket | Different limits per endpoint |
| **GitHub API** | 5000 req/hour (authenticated) | Sliding window | Returns rate limit in headers |
| **Stripe API** | 100 req/sec | Token bucket | Burst allowed, then throttle |
| **AWS API Gateway** | 10K req/sec, 5K burst | Token bucket | Configurable per API |
| **Google Maps API** | Varies by plan | Quota-based | Daily/monthly quotas |

---

## ⚠️ Common Pitfalls

1. **Not returning rate limit info**
   - **Problem**: Client doesn't know when to retry
   - **Solution**: Include headers: `X-RateLimit-Remaining`, `X-RateLimit-Reset`, `Retry-After`

2. **Same limit for all endpoints**
   - **Problem**: Expensive operations (video upload) same as cheap (ping)
   - **Solution**: Different limits per endpoint cost

3. **IP-based limiting with CDN/proxy**
   - **Problem**: All users appear from same IP
   - **Solution**: Use `X-Forwarded-For` header, user authentication

4. **Not handling clock skew**
   - **Problem**: Distributed systems have time differences
   - **Solution**: Use logical clocks or tolerate small differences

5. **Cascading failures**
   - **Problem**: Rate limiter down → all requests fail
   - **Solution**: Fail open (allow requests) with monitoring alerts

---

## 📝 Quick Reference

**One-liner**: Protect systems by controlling request rate per user/IP/endpoint  
**Default choice**: Sliding window counter (balance of accuracy and efficiency)  
**Storage**: Redis with atomic operations (INCR, EXPIRE)  
**Response codes**: 429 Too Many Requests, include Retry-After header  
**Distributed**: Centralized Redis or rate limiter service with local caching

---

<!-- TOC --><a name="4-distributed-search"></a>
# 4. Distributed Search

> **📌 Quick Summary**: Horizontally scaled search system using inverted indexes and MapReduce  
> **Use Cases**: Full-text search, document retrieval, log analysis | **Complexity**: ⭐⭐⭐⭐⭐

### Why Distributed Search?

**Challenge**: Searching billions of documents in milliseconds
- 📚 **Scale**: TB-PB of data, billions of documents
- ⚡ **Latency**: Sub-second query response required
- 🔍 **Complexity**: Boolean queries, phrase matching, fuzzy search
- 🔄 **Updates**: Real-time indexing of new documents

**Solution**: Distribute indexing and search across multiple nodes

---

<!-- TOC --><a name="41-blob-store"></a>
## 4.1 Blob Store

> **📌 Quick Summary**: Storage system for large binary objects (images, videos, files)

### Blob Storage Components

| Component | Purpose | Implementation |
|-----------|---------|----------------|
| **Blob metadata** | Efficient storage and retrieval metadata | Key-value store (size, type, checksum, creation time) |
| **Partitioning** | Distribute blobs across data nodes | Consistent hashing, range-based |
| **Blob indexing** | Efficient blob searching | Inverted index on metadata |
| **Pagination** | Retrieve limited results | Cursor-based or offset-based |
| **Replication** | High availability | 3x replication typical (different racks/zones) |
| **Garbage collection** | Delete without impacting performance | Mark-and-sweep, background process |
| **Streaming** | Chunk-by-chunk delivery | HTTP range requests, chunked encoding |
| **Caching** | Improve response time | CDN for hot blobs, local cache for metadata |

### 🎯 Key Design Decisions

**Replication Strategy**:
- **3x replication**: Balance between availability and storage cost
- **Placement**: Different racks/zones to handle hardware failures
- **Consistency**: Eventually consistent (okay for immutable blobs)

**Garbage Collection**:
- **Mark-and-sweep**: Mark deleted blobs, sweep in background
- **Compaction**: Reclaim space periodically
- **Timing**: Off-peak hours to minimize impact

**Streaming**:
- **Chunking**: 1-10MB chunks typical
- **Range requests**: Support seeking in videos
- **Adaptive bitrate**: Multiple resolutions for video

---

<!-- TOC --><a name="42-index"></a>
## 4.2 Inverted Index

> **📌 Quick Summary**: Core data structure enabling fast full-text search

### What is an Inverted Index?

An **inverted index** is a HashMap-like structure that maps terms to documents (reverse of document-to-terms).

**Forward Index** (document → terms):
```
Doc1: "The quick brown fox"
Doc2: "The lazy dog"
Doc3: "Quick brown dogs"
```

**Inverted Index** (term → documents):
```
"quick" → [Doc1, Doc3]
"brown" → [Doc1, Doc3]
"fox"   → [Doc1]
"lazy"  → [Doc2]
"dog"   → [Doc2]
"dogs"  → [Doc3]
```

![image](https://github.com/user-attachments/assets/f637f109-db53-442e-ab43-b30386246e5f)

### Building Inverted Index: Step-by-Step

#### Phase 1: Tokenization
```
1. Split documents into terms (words)
2. Normalize: lowercase, remove punctuation
3. Remove stop words: "the", "is", "at", "which", "on"
4. Apply stemming: "running" → "run", "dogs" → "dog"
```

**Example**:
```
Input: "The quick BROWN fox is running"
Tokens: ["quick", "brown", "fox", "running"]
Stemmed: ["quick", "brown", "fox", "run"]
```

#### Phase 2: Create Term-Document Matrix
```
For each token:
  - Create entry: term → [doc_id, position]
  - Store metadata: frequency, document length
```

**Result**:
```
{
  "quick": [(Doc1, pos=1), (Doc3, pos=0)],
  "brown": [(Doc1, pos=2), (Doc3, pos=1)],
  "fox":   [(Doc1, pos=3)],
  "run":   [(Doc1, pos=5)]
}
```

#### Phase 3: Compression & Optimization
```
1. Variable-length encoding for doc IDs
2. Delta encoding for positions
3. Skip lists for fast intersection
4. Bloom filters for quick negative lookups
```

### MapReduce for Inverted Index

> **📌 Distributed index building across cluster**

![image](https://github.com/user-attachments/assets/aec48b4a-8451-4b04-8d9a-3077a272e051)

**Map Phase**:
```python
def map(doc_id, content):
    tokens = tokenize(content)
    for token in tokens:
        emit(token, doc_id)
```

**Shuffle Phase**:
```
Group by key (term):
  "quick" → [Doc1, Doc3, Doc5]
  "brown" → [Doc1, Doc3, Doc7, Doc9]
```

**Reduce Phase**:
```python
def reduce(term, doc_list):
    posting_list = sort_and_dedupe(doc_list)
    emit(term, posting_list)
```

**Distributed Execution**:
```
1. Split documents across N map workers
2. Each mapper processes chunk, emits (term, doc_id) pairs
3. Shuffle groups all doc_ids for each term
4. Reducers create final posting lists
5. Merge and store in distributed index
```

---

## Search Query Execution

### Single-Term Query
```
Query: "quick"
1. Lookup "quick" in inverted index
2. Retrieve posting list: [Doc1, Doc3, Doc5]
3. Fetch documents, rank, return Top-K
```

### Boolean AND Query
```
Query: "quick AND brown"
1. Lookup both terms:
   "quick" → [Doc1, Doc3, Doc5, Doc7]
   "brown" → [Doc1, Doc3, Doc9]
2. Intersect posting lists: [Doc1, Doc3]
3. Return matching documents
```

**Optimization**: Process shortest list first

### Phrase Query
```
Query: "quick brown fox"
1. Lookup all terms
2. Check positions are consecutive
3. "quick" at pos 1, "brown" at pos 2, "fox" at pos 3 → Match!
```

---

## ⚖️ Advantages vs Disadvantages

### Advantages ✅

1. **Fast full-text search**
   - O(1) term lookup in hash table
   - O(K) for Top-K results (K << total docs)
   - Typical: <100ms for billions of documents

2. **Efficient counting**
   - Term frequency pre-computed
   - No need to scan documents at query time

3. **Complex query support**
   - Boolean operators (AND, OR, NOT)
   - Phrase queries, proximity searches
   - Fuzzy matching, wildcards

### Disadvantages ❌

1. **Storage overhead**
   - Index size: 20-50% of original data
   - Trade-off: Space for speed

2. **Maintenance cost**
   - **Insert**: Extract terms, update multiple posting lists
   - **Update**: Re-index entire document
   - **Delete**: Mark deleted in posting lists, compact later

3. **Freshness challenge**
   - Batch indexing: Minutes to hours delay
   - Real-time indexing: More expensive, complex

---

## 💡 Distributed Search Architecture

### Index Sharding Strategies

#### 1. Document Partitioning (Horizontal)
```
Shard 1: Documents 1-1M
Shard 2: Documents 1M-2M
Shard 3: Documents 2M-3M
```

**Query execution**:
- Scatter: Query all shards in parallel
- Gather: Merge and rank results

✅ **Pros**: Balanced storage, simple
❌ **Cons**: Query touches all shards

#### 2. Term Partitioning (Vertical)
```
Shard 1: Terms A-F
Shard 2: Terms G-M
Shard 3: Terms N-Z
```

**Query execution**:
- Query only relevant shards for query terms
- Merge posting lists

✅ **Pros**: Fewer shards touched per query
❌ **Cons**: Load imbalance (common terms hot)

**Recommendation**: Document partitioning for most use cases

---

## 💼 Real-World Examples

| System | Scale | Strategy | Notes |
|--------|-------|----------|-------|
| **Elasticsearch** | Billions of docs | Document sharding | Distributed, RESTful search |
| **Apache Solr** | Petabytes | Document sharding | Built on Lucene |
| **Google Search** | Trillions of pages | Multi-tier indexing | Real-time index + full index |
| **Twitter Search** | 1 trillion tweets | Time-based sharding | 15-second indexing latency |
| **Amazon Product Search** | Billions of products | Hybrid partitioning | Real-time updates critical |

---

## ⚠️ Common Pitfalls

1. **Not tuning tokenization**
   - **Problem**: Poor relevance, missed matches
   - **Solution**: Language-specific analyzers, custom tokenizers

2. **Ignoring relevance scoring**
   - **Problem**: Documents returned in arbitrary order
   - **Solution**: TF-IDF, BM25, or learning-to-rank

3. **Single-threaded indexing**
   - **Problem**: Slow index updates
   - **Solution**: Parallel indexing, MapReduce

4. **No real-time requirements**
   - **Problem**: Batch indexing causes stale results
   - **Solution**: Dual index (real-time + batch)

---

## 📝 Quick Reference

**One-liner**: Term-to-document mapping enabling O(1) search lookup  
**Storage overhead**: 20-50% of original data size  
**Query latency**: <100ms typical for billions of documents  
**Sharding strategy**: Document partitioning for balanced load  
**Popular implementations**: Elasticsearch, Solr (Lucene), Algolia

---

<!-- TOC --><a name="5-distributed-task-scheduler"></a>
# 5. Distributed Task Scheduler

> **📌 Quick Summary**: Reliable execution of periodic and delayed tasks across distributed systems  
> **Use Cases**: Cron jobs, background processing, workflow orchestration | **Complexity**: ⭐⭐⭐⭐⭐

![image](https://github.com/user-attachments/assets/5de17b3e-0048-4481-8507-1391d2e2e749)

### Why Distributed Task Scheduler?

**Purpose**: Execute tasks reliably at scale
- ⏰ **Scheduled tasks**: Cron jobs, periodic reports, data cleanup
- 🔄 **Delayed execution**: Rate limiting, retry logic, reminders
- 📊 **Batch processing**: ETL jobs, data aggregation, backups
- 🎯 **Guaranteed execution**: At-least-once or exactly-once semantics
- 📈 **Scalability**: Handle millions of tasks across thousands of workers

### 🎯 Key Concepts

- **Task**: Unit of work to be executed (function + parameters)
- **Schedule**: When to execute (cron expression, delay, timestamp)
- **Worker**: Node that executes tasks
- **Task queue**: Persistent storage of pending tasks
- **Coordinator**: Assigns tasks to workers, handles failures
- **Idempotency**: Task can be safely executed multiple times

---

## Core Components

### 1. Task Queue
**Purpose**: Store pending tasks persistently
- **Implementation**: Kafka, RabbitMQ, Redis, database
- **Priority queues**: High-priority tasks executed first
- **Partitioning**: Distribute tasks across multiple queues

### 2. Scheduler Service
**Purpose**: Determine when tasks should run
- **Cron parsing**: Convert `0 0 * * *` to execution times
- **Delay calculation**: Future timestamp for delayed tasks
- **Trigger generation**: Create task instances at scheduled times

### 3. Worker Pool
**Purpose**: Execute tasks in parallel
- **Horizontal scaling**: Add workers to increase capacity
- **Specialization**: Different worker types for different tasks
- **Resource limits**: CPU, memory, timeout per task

### 4. Coordinator/Master
**Purpose**: Orchestrate task distribution
- **Task assignment**: Route tasks to appropriate workers
- **Health monitoring**: Track worker status, handle failures
- **Load balancing**: Distribute work evenly
- **Consistency**: Use ZooKeeper or etcd for coordination

### 5. Result Store
**Purpose**: Track task execution status
- **States**: Pending, Running, Completed, Failed
- **Retry tracking**: Attempt count, last error
- **Audit log**: Execution history for debugging

---

## Task Scheduling Patterns

### 1. Cron-Style Periodic Tasks
```
Pattern: "0 0 * * *" (daily at midnight)
Use case: Daily reports, backup jobs

Implementation:
1. Scheduler generates next execution time
2. Creates task instance at scheduled time
3. Worker executes, scheduler computes next run
```

### 2. Delayed Tasks
```
Pattern: Execute after 5 minutes
Use case: Reminder notifications, rate limiting

Implementation:
1. Store task with future timestamp
2. Scheduler polls for ready tasks
3. Dispatch to workers when timestamp reached
```

### 3. Dependent Tasks (DAG)
```
Pattern: Task B runs after Task A completes
Use case: ETL pipelines, workflows

Implementation:
1. Store task dependencies as DAG
2. Execute tasks in topological order
3. Wait for dependencies before scheduling
```

### 4. One-Time Tasks
```
Pattern: Execute once, now or at specific time
Use case: User-triggered actions, webhooks

Implementation:
1. Enqueue task immediately
2. Mark as one-time (no rescheduling)
3. Delete after execution
```

---

## 💡 Design Decisions

### At-Least-Once vs Exactly-Once

**At-Least-Once**:
- ✅ **Simple**: Retry on failure
- ⚠️ **Requirement**: Tasks must be idempotent
- **Implementation**: Ack after execution, retry on timeout

**Exactly-Once**:
- ✅ **Correctness**: No duplicate executions
- ❌ **Complex**: Distributed transactions, deduplication
- **Implementation**: Unique task ID, store execution state before running

**Recommendation**: At-least-once + idempotent tasks (simpler, performant)

### Failure Handling

| Failure Type | Detection | Mitigation |
|--------------|-----------|------------|
| **Worker crash** | Heartbeat timeout | Reassign task to different worker |
| **Task timeout** | Execution timer | Kill task, retry on another worker |
| **Coordinator failure** | Leader election | Standby takes over via Raft/Paxos |
| **Task failure** | Exception caught | Exponential backoff retry |
| **Network partition** | Health checks | Wait for partition to heal, use quorum |

### Retry Strategy

**Exponential Backoff**:
```
Attempt 1: Immediate
Attempt 2: Wait 1 second
Attempt 3: Wait 2 seconds
Attempt 4: Wait 4 seconds
Attempt 5: Wait 8 seconds
Max attempts: 5, then dead-letter queue
```

**Jitter**: Add randomness to avoid thundering herd
```python
delay = base_delay * (2 ** attempt) + random(0, jitter)
```

---

## ⚖️ Trade-offs

**Centralized Coordinator**:
- ✅ **Pros**: Simple, consistent view
- ❌ **Cons**: Single point of failure, scaling bottleneck
- **Solution**: Leader election with standby replicas

**Distributed Coordination**:
- ✅ **Pros**: No SPOF, highly scalable
- ❌ **Cons**: Complex, eventual consistency challenges
- **Solution**: Use consensus algorithms (Raft, Paxos)

**Push vs Pull**:
- **Push**: Coordinator assigns tasks to workers
  - ✅ Better load balancing
  - ❌ Coordinator must track worker state
- **Pull**: Workers request tasks from queue
  - ✅ Self-healing, simpler
  - ❌ Potential idle time between polls

---

## 💼 Real-World Examples

| System | Scale | Use Case | Technology |
|--------|-------|----------|------------|
| **Apache Airflow** | 1000s of DAGs | ETL pipelines, data workflows | Python, PostgreSQL |
| **Celery** | Millions of tasks/day | Async task queue for Python | RabbitMQ, Redis |
| **Quartz** | Enterprise scale | Java job scheduling | JDBC, clustering |
| **Kubernetes CronJob** | Container orchestration | Scheduled batch jobs | etcd, containers |
| **AWS Step Functions** | Serverless | Workflow orchestration | Managed service |
| **Temporal** | Millions of workflows | Durable execution | Cassandra/MySQL |

---

## ⚠️ Common Pitfalls

1. **Non-idempotent tasks**
   - **Problem**: Duplicate executions cause corruption
   - **Solution**: Design tasks to be safely retried

2. **No dead-letter queue**
   - **Problem**: Failed tasks lost forever
   - **Solution**: Move failed tasks to DLQ after max retries

3. **Unbounded queues**
   - **Problem**: Memory exhaustion, task starvation
   - **Solution**: Queue size limits, backpressure

4. **Single coordinator**
   - **Problem**: SPOF, scaling limit
   - **Solution**: Leader election, standby replicas

5. **Ignoring task dependencies**
   - **Problem**: Tasks execute in wrong order
   - **Solution**: DAG representation, topological sort

---

## 📝 Quick Reference

**One-liner**: Reliable execution of scheduled/delayed tasks across distributed workers  
**Default choice**: At-least-once execution with idempotent tasks  
**Coordinator**: Use leader election (ZooKeeper, etcd, Raft)  
**Queue**: Kafka for high throughput, Redis for simplicity  
**Retry**: Exponential backoff with jitter, max 5 attempts  
**Popular**: Airflow (workflows), Celery (Python), Temporal (durable execution)

---

<!-- TOC --><a name="6-sharded-counters"></a>
# 6. Sharded Counters

> **📌 Quick Summary**: Distributed counter system to handle high-frequency increments  
> **Use Cases**: View counts, likes, real-time analytics | **Complexity**: ⭐⭐⭐⭐☆

![image](https://github.com/user-attachments/assets/0d3f9fff-271a-4693-a6d2-138add59b885)

### Why Sharded Counters?

**Problem**: Single counter becomes bottleneck
- 🔥 **Hot key**: One counter receives millions of increments/sec
- 🔒 **Contention**: Lock/transaction conflicts degrade performance
- 📉 **Latency**: Write amplification as traffic increases

**Solution**: Split counter across multiple shards, aggregate on read

---

## How Sharded Counters Work

### Architecture

```
Counter "video:12345:views" with 10 shards:

Shard 0: counter = 1,234
Shard 1: counter = 1,189
Shard 2: counter = 1,305
...
Shard 9: counter = 1,276

Total = Sum of all shards = 12,500
```

### Operations

**Increment (Write)**:
```python
def increment(counter_id, value=1):
    shard_id = hash(counter_id + random()) % num_shards
    atomic_increment(f"{counter_id}:shard:{shard_id}", value)
```

**Read (Get Total)**:
```python
def get_count(counter_id):
    total = 0
    for shard_id in range(num_shards):
        total += get(f"{counter_id}:shard:{shard_id}")
    return total
```

---

## 🎯 Key Design Decisions

### Number of Shards

**Trade-off**: More shards = higher write throughput but slower reads

| Shards | Write Throughput | Read Latency | Use Case |
|--------|------------------|--------------|----------|
| **1** | 10K writes/sec | 1ms | Low traffic |
| **10** | 100K writes/sec | 10ms | Moderate traffic |
| **100** | 1M writes/sec | 100ms | High traffic |
| **1000** | 10M writes/sec | 1s | Extreme traffic |

**Formula**: `num_shards = expected_writes_per_sec / 10K`

**Dynamic Sharding**: Start with 10, increase if bottleneck detected

### Shard Selection

**Random Distribution**:
```python
shard_id = random.randint(0, num_shards - 1)
```
- ✅ Even distribution
- ✅ No coordination needed
- ⚠️ Non-deterministic

**Hash-Based**:
```python
shard_id = hash(user_id + counter_id) % num_shards
```
- ✅ Deterministic (same user → same shard)
- ⚠️ Potential hotspots if user is very active

**Recommendation**: Random for better load distribution

---

## ⚖️ Read Optimization Strategies

### 1. Periodic Aggregation
```
Background job every 5 minutes:
- Sum all shards
- Store result in "counter:total" key
- Serve reads from cached total

Trade-off: Stale data (up to 5 min old)
```

### 2. Approximate Counts
```
Sample subset of shards:
- Read 10% of shards
- Multiply by 10

Trade-off: ±10% accuracy for 90% faster reads
```

### 3. Lazy Aggregation
```
On read:
- Check if cached total exists and is fresh
- If yes, return cached value
- If no, aggregate shards, cache for 30 seconds

Trade-off: First read slow, subsequent reads fast
```

### 4. Read Replicas
```
Replicate aggregated total to read replicas:
- Periodic sync from primary
- Reads served from replicas

Trade-off: Infrastructure cost, eventual consistency
```

---

## 💼 Real-World Examples

| Platform | Counter Type | Shards | Update Frequency |
|----------|--------------|--------|------------------|
| **YouTube** | Video views | 100-1000 | Real-time |
| **Twitter** | Tweet likes | 50-100 | Sub-second |
| **Reddit** | Post upvotes | 10-50 | Real-time |
| **Instagram** | Post likes | 100-500 | Real-time |
| **LinkedIn** | Profile views | 20-50 | Near real-time |

---

## Implementation Example: Redis

```python
import redis
import random

class ShardedCounter:
    def __init__(self, redis_client, num_shards=10):
        self.redis = redis_client
        self.num_shards = num_shards
    
    def increment(self, counter_id, value=1):
        shard_id = random.randint(0, self.num_shards - 1)
        key = f"{counter_id}:shard:{shard_id}"
        self.redis.incrby(key, value)
    
    def get_count(self, counter_id):
        total = 0
        for shard_id in range(self.num_shards):
            key = f"{counter_id}:shard:{shard_id}"
            count = self.redis.get(key) or 0
            total += int(count)
        return total
    
    def get_approximate(self, counter_id, sample_size=3):
        """Sample subset of shards for faster reads"""
        total = 0
        for _ in range(sample_size):
            shard_id = random.randint(0, self.num_shards - 1)
            key = f"{counter_id}:shard:{shard_id}"
            count = self.redis.get(key) or 0
            total += int(count)
        # Extrapolate from sample
        return int(total * (self.num_shards / sample_size))
```

---

## ⚠️ Common Pitfalls

1. **Too few shards**
   - **Problem**: Still have write contention
   - **Solution**: Monitor latency, increase shards dynamically

2. **Too many shards**
   - **Problem**: Slow reads, high storage overhead
   - **Solution**: Use aggregation strategies

3. **No caching on reads**
   - **Problem**: Every read scans all shards
   - **Solution**: Cache aggregated value with TTL

4. **Not handling shard failures**
   - **Problem**: Missing counts if shard unavailable
   - **Solution**: Replicate shards, return partial result if needed

5. **Exact counts when approximate is sufficient**
   - **Problem**: Unnecessary read latency
   - **Solution**: Use approximate reads for non-critical display

---

## 📝 Quick Reference

**One-liner**: Split high-traffic counter across shards to eliminate write bottleneck  
**Optimal shards**: 10-100 for most use cases  
**Read optimization**: Cache aggregated total with 30-60s TTL  
**Accuracy**: Exact for writes, approximate acceptable for reads (views, likes)  
**Implementation**: Redis INCRBY with random shard selection  
**When to use**: >10K increments/sec on single counter

---
