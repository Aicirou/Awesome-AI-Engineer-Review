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
