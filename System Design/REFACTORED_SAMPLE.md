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

**🔍 Image Description - SQL vs NoSQL Architecture Comparison**: This diagram illustrates the fundamental architectural differences between SQL and NoSQL database systems:
- **Left side (SQL)**: Traditional relational database architecture with structured tables (rows and columns), showing normalized data across multiple related tables with foreign key relationships. Entities like "Users," "Orders," and "Products" are connected through primary-foreign key relationships, emphasizing rigid schema and ACID transaction boundaries.
- **Right side (NoSQL)**: Document-oriented/key-value store architecture showing denormalized data stored as self-contained JSON/BSON documents. Each document contains all related data (embedded objects), eliminating joins. Illustrates horizontal scalability with data sharded across multiple nodes.
- **Key visual elements**: Arrows showing write/read patterns, replication flows (synchronous for SQL vs asynchronous for NoSQL), scale-out topology differences (vertical scaling via larger single server vs horizontal scaling via distributed cluster).
- **Modern context (2024-2025)**: Hybrid approaches like PostgreSQL with JSONB columns, CockroachDB (distributed SQL), YugabyteDB, and MongoDB with ACID transactions blur these boundaries, enabling SQL semantics at NoSQL scale.

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

**🔍 Image Description**: This diagram depicts a Merkle Tree structure used for efficient replica synchronization in distributed systems. The visualization demonstrates:
- **Tree structure**: Binary tree with leaf nodes at the bottom representing individual data blocks/key-value pairs (e.g., "Block 0", "Block 1", "Block 2", "Block 3"), each containing a cryptographic hash (SHA-256) of the actual data.
- **Internal nodes**: Each parent node contains the hash of concatenated child hashes (e.g., Hash(0-1) = SHA256(Hash(0) + Hash(1))), building up the hierarchy level by level.
- **Root hash**: Single top-level hash representing the entire dataset's fingerprint, enabling O(1) comparison between two replicas to detect any inconsistencies.
- **Synchronization workflow**: Side-by-side comparison showing two replicas (Node A and Node B) with arrows illustrating the comparison process:
  1. **Step 1**: Compare root hashes - if identical, datasets match (early exit)
  2. **Step 2**: If roots differ, recursively compare child subtree hashes to locate divergent branches
  3. **Step 3**: Drill down to specific differing leaf nodes containing the actual inconsistent data
  4. **Step 4**: Transfer only the minimal set of differing key-value pairs (highlighted in red/different color)
- **Efficiency highlight**: Instead of comparing all N data items (O(N) bandwidth), only log(N) hash comparisons needed, with minimal data transfer for repairs. For a 1M-item dataset, this means ~20 hash comparisons vs 1M item comparisons.
- **Real-world applications**: Used in Cassandra anti-entropy repairs, Bitcoin/blockchain verification, Git commit histories, and Amazon Dynamo replica synchronization (modern implementations use Merkle trees in DynamoDB, Riak, and distributed file systems like IPFS).

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

**🔍 Image Description - SQL vs NoSQL Architecture Comparison**: This diagram illustrates the fundamental architectural differences between SQL and NoSQL database systems:
- **Left side (SQL)**: Traditional relational database architecture with structured tables (rows and columns), showing normalized data across multiple related tables with foreign key relationships. Entities like "Users," "Orders," and "Products" are connected through primary-foreign key relationships, emphasizing rigid schema and ACID transaction boundaries.
- **Right side (NoSQL)**: Document-oriented/key-value store architecture showing denormalized data stored as self-contained JSON/BSON documents. Each document contains all related data (embedded objects), eliminating joins. Illustrates horizontal scalability with data sharded across multiple nodes.
- **Key visual elements**: Arrows showing write/read patterns, replication flows (synchronous for SQL vs asynchronous for NoSQL), scale-out topology differences (vertical scaling via larger single server vs horizontal scaling via distributed cluster).
- **Modern context (2024-2025)**: Hybrid approaches like PostgreSQL with JSONB columns, CockroachDB (distributed SQL), YugabyteDB, and MongoDB with ACID transactions blur these boundaries, enabling SQL semantics at NoSQL scale.

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

**🔍 Image Description - CDN Global Distribution Overview**: This diagram illustrates the high-level CDN architecture showing content distribution from origin to users:
- **Geographic distribution**: World map visualization showing CDN edge servers (PoPs - Points of Presence) strategically placed across continents (North America, Europe, Asia, South America, Africa, Australia). Each region contains multiple edge server clusters.
- **User-to-Edge routing**: Arrows showing how users (represented by laptop/mobile icons) connect to their geographically nearest edge server, reducing latency from ~200ms (transcontinental) to ~10-30ms (local).
- **Edge-to-Origin flow**: Dashed lines showing how edge servers fetch content from the central origin server (located in cloud/data center) on cache miss, then cache it locally for subsequent requests.
- **Traffic flow pattern**: Solid arrows for user requests (green) and dashed arrows for origin fetches (blue), illustrating the "fan-out" pattern where one origin fetch serves millions of edge requests.
- **Scale indicators**: Labels showing typical metrics like "50-200 PoPs worldwide" for enterprise CDNs (Cloudflare, Akamai, Fastly), "10-50ms latency to users," and "90-95% cache hit ratio."
- **Modern CDN features (2024-2025)**: Edge computing annotations showing Cloudflare Workers, AWS Lambda@Edge, and Fastly Compute@Edge enabling serverless functions at CDN edge for dynamic content generation, authentication, and A/B testing without origin round-trips.

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

**🔍 Image Description - CDN Component Architecture & Data Flow**: This diagram shows the detailed internal architecture of a CDN system with all major components and their interactions:
- **Client layer**: Multiple client types (web browsers, mobile apps, IoT devices) at the top, representing billions of concurrent users making HTTP/HTTPS requests.
- **Routing/DNS layer**: GeoDNS and Anycast routing systems directing clients to optimal edge servers based on geographic proximity, server load, and network conditions. Shows DNS resolution flow: Client → GeoDNS → IP of nearest edge server.
- **Security layer (Scrubber Servers)**: DDoS protection and Web Application Firewall (WAF) filtering malicious traffic before reaching edge servers. Handles volumetric attacks (10-100+ Gbps), protecting both edge and origin infrastructure.
- **Edge/Proxy server tier**: Thousands of NGINX/Varnish servers distributed globally, each with:
  - **RAM cache** (hot data, microsecond access, 90%+ hit ratio)
  - **SSD cache** (warm data, millisecond access)
  - **Content validation** (ETags, If-Modified-Since headers)
- **Distribution system**: Content propagation layer between origin and edge servers:
  - **Push model**: Origin proactively sends content updates to edge servers (used for critical updates, new releases)
  - **Pull model**: Edge servers fetch content on-demand from origin on first cache miss (used for long-tail content)
  - **Multicast/P2P protocols**: Efficient distribution for large files (software updates, video segments)
- **Origin server tier**: Source of truth containing original content. Shows connection pooling, rate limiting to prevent origin overload, and origin shield (intermediate cache layer) reducing origin requests by 70-90%.
- **Management/Control plane**: Monitoring dashboard showing real-time metrics: cache hit ratio, bandwidth usage, error rates, geographic distribution of requests, and billing/analytics data.
- **Arrow annotations**: Color-coded data flows showing:
  - **Green**: Cache hit path (client → edge → response, <50ms total)
  - **Yellow**: Cache miss path (client → edge → origin → edge → response, 200-500ms)
  - **Red**: Purge/invalidation commands (management → edge servers, propagates in seconds)
- **Modern enhancements (2024-2025)**: Edge compute boxes showing serverless function execution at CDN edge (Cloudflare Workers, AWS Lambda@Edge, Deno Deploy), enabling dynamic content generation, API gateways, and real-time personalization without origin server involvement. Also shows HTTP/3 (QUIC) protocol support for 30-50% latency reduction vs HTTP/2.

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

**🔍 Image Description - Rate Limiter System Architecture**: This diagram shows the complete rate limiting workflow and system architecture:
- **Request flow path**: Client → API Gateway (with rate limiter middleware) → Backend Services, with rate limiter as the gatekeeper before requests reach application servers.
- **Rate limiter components**:
  - **Rules Engine**: Stores rate limit policies (e.g., "100 requests/minute for free tier, 1000 requests/minute for paid tier") with user tier mapping (IP-based, API key-based, user ID-based).
  - **Redis/Memcached counter store**: Distributed cache holding request counters per user/IP with TTL expiration. Shows key structure like "rate:user:{user_id}:{minute}" with atomic increment operations.
  - **Token bucket/Sliding window data structures**: Visual representation of token bucket (capacity, refill rate, current tokens) and sliding window log (timestamps of recent requests).
- **Decision logic flowchart**: 
  1. Extract identifier (IP, API key, user ID) from request
  2. Fetch current request count from Redis
  3. Check against rate limit threshold
  4. If under limit: Increment counter, forward request (HTTP 200)
  5. If over limit: Reject request (HTTP 429 Too Many Requests) with "Retry-After" header
- **Response headers**: Shows standard rate limit headers returned to clients:
  - `X-RateLimit-Limit: 100` (total quota)
  - `X-RateLimit-Remaining: 45` (remaining requests)
  - `X-RateLimit-Reset: 1634567890` (Unix timestamp when quota resets)
- **Multi-tier architecture**: Shows how rate limiting applies at multiple layers:
  - **Layer 7 (Application)**: API-specific limits per endpoint (e.g., "/search": 10 req/s, "/upload": 1 req/min)
  - **Layer 4 (Connection)**: TCP connection rate limiting (prevent SYN floods)
  - **Layer 3 (Network)**: IP-based blocking for DDoS mitigation
- **Distributed rate limiting**: Shows synchronization between multiple API Gateway instances using Redis Pub/Sub or centralized counter, ensuring consistent limits across the cluster.
- **Modern implementations (2024-2025)**: Annotations showing cloud-native solutions like AWS API Gateway throttling, Google Cloud Armor adaptive rate limiting (ML-based), Kong/NGINX rate limit plugins, and Envoy proxy global rate limiting with token bucket algorithm. Also shows emerging patterns like adaptive rate limiting (adjusting limits based on system load) and priority-based queuing (VIP users get higher limits).

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

<!-- TOC --><a name="7-youtube"></a>
# 7. YouTube - Video Streaming Platform

> **📌 Quick Summary**: Large-scale video sharing platform with encoding, storage, and CDN delivery  
> **Scale**: 2B+ users, 500hrs uploaded/min, 1B+ hours watched daily | **Complexity**: ⭐⭐⭐⭐⭐

![image](https://github.com/user-attachments/assets/cdd2be4a-a2b6-4cf8-8f17-e753afd0ad9d)

### System Requirements

**Functional**:
- ✅ Upload videos (multiple formats, up to 256GB)
- ✅ Stream videos with adaptive bitrate
- ✅ Search videos by title, description, tags
- ✅ Like, comment, subscribe functionality
- ✅ Generate and serve thumbnails

**Non-Functional**:
- ⚡ **Latency**: <200ms for video start (CDN), <2s for upload processing
- 📈 **Scalability**: Handle billions of daily views
- 🔒 **Availability**: 99.9% uptime for streaming
- 📊 **Consistency**: Eventual consistency acceptable (view counts, likes)
- 💾 **Storage**: Petabytes of video data

---

## High-Level Architecture

### Upload Flow

```
1. User uploads video → Load Balancer
2. Web Server receives video → Store in Upload Storage (temporary)
3. Metadata saved to Database (MySQL)
4. Video handed to Encoder Queue
5. Encoder + Transcoder process video:
   - Compress video
   - Generate multiple resolutions (2160p, 1440p, 1080p, 720p, 480p, 360p)
   - Extract audio tracks
   - Generate thumbnails
6. Processed videos stored in Blob Storage (GFS/S3)
7. Popular videos pushed to CDN
8. User notified of successful upload
```

### Streaming Flow

```
1. User requests video → DNS resolves to nearest CDN
2. CDN cache hit → Serve directly (1-5ms)
3. CDN cache miss → Fetch from Blob Storage → Cache → Serve
4. Analytics: Track views, watch time, engagement
```

![image](https://github.com/user-attachments/assets/34180cb8-fefd-40b0-8d0a-48944237cd43)

---

## Component Breakdown

| Component | Responsibility | Technology | Scale |
|-----------|----------------|------------|-------|
| **Load Balancers** | Distribute user requests | NGINX, HAProxy | 100s of instances |
| **Web Servers** | Handle API requests | Node.js, Go | 1000s of instances |
| **Application Servers** | Business logic, video processing orchestration | Java, Python | 1000s of instances |
| **User Database** | User accounts, subscriptions, preferences | MySQL (sharded) | 100TB+ |
| **Video Metadata DB** | Video info, descriptions, tags, stats | MySQL (sharded) | 500TB+ |
| **Bigtable** | Thumbnails storage (key-value) | Bigtable, Cassandra | Petabytes |
| **Upload Storage** | Temporary video storage | Distributed FS | 100TB+ |
| **Encoders** | Video compression, transcoding | FFmpeg, custom | 10,000s of workers |
| **Blob Storage** | Permanent video storage | GFS, S3, Colossus | 1+ Exabyte |
| **CDN** | Edge caching for popular content | Akamai, Cloudflare, custom | 1000s of PoPs |
| **Colocation Sites** | Regional caching (not full CDN) | Custom | 100s worldwide |
| **Search Index** | Video search by metadata | Elasticsearch | 100TB+ |
| **Analytics System** | View counts, engagement metrics | BigQuery, Kafka | Petabytes/day |

---

## 🎯 Key Design Decisions

### 1. Why Bigtable for Thumbnails?

**Requirements**:
- Each video has 4-10 thumbnails (different timestamps)
- Thumbnails are small (<100KB each)
- High read throughput (displayed in search results, recommendations)
- Billions of thumbnails total

**Bigtable characteristics**:
- ✅ Optimized for small objects (<10MB)
- ✅ High throughput for key-value lookups
- ✅ Horizontal scalability
- ✅ Lower cost than blob storage for small files

**Key structure**: `video_id:thumbnail_number` → image bytes

### 2. Separate User and Video Metadata Storage

**Why decoupled?**
- **Different scale**: Videos grow faster than users
- **Different access patterns**: 
  - User data: Strong consistency needed (authentication, subscriptions)
  - Video metadata: Eventual consistency acceptable (views, likes)
- **Different optimization**: 
  - User DB: ACID transactions, normalized
  - Video metadata: Denormalized for read performance

### 3. Multi-Tier Video Storage

**Three tiers**:

| Tier | Content | Storage | Access Latency |
|------|---------|---------|----------------|
| **CDN (Edge)** | Viral/trending videos (top 5%) | RAM/SSD | 1-5ms |
| **Colocation** | Moderately popular (next 15%) | SSD/HDD | 10-50ms |
| **Origin (Blob)** | Long-tail content (80%) | HDD/Tape | 100-500ms |

**Benefits**:
- Cost optimization (most videos rarely watched)
- Low latency for popular content
- Scalable storage for massive archive

---

## Video Encoding Pipeline

### Multi-Resolution Encoding

**Why multiple resolutions?**
- 📱 Different devices (mobile, tablet, TV, 4K displays)
- 🌐 Different network speeds (4G, WiFi, fiber)
- 💰 Bandwidth cost optimization

**Resolution ladder**:
```
2160p (4K):  3840×2160, 35-45 Mbps  (< 5% of views)
1440p (2K):  2560×1440, 16-24 Mbps  (< 10% of views)
1080p (FHD): 1920×1080, 8-12 Mbps   (~30% of views)
720p (HD):   1280×720,  5-7 Mbps    (~35% of views)
480p (SD):   854×480,   2-3 Mbps    (~15% of views)
360p:        640×360,   1-2 Mbps    (~5% of views)
```

### Adaptive Bitrate Streaming (ABR)

**How it works**:
1. Video split into 5-10 second chunks
2. Each chunk available in all resolutions
3. Player measures bandwidth
4. Dynamically switches resolution per chunk
5. Smooth playback, no buffering

**Protocol**: HLS (HTTP Live Streaming) or DASH (Dynamic Adaptive Streaming)

**Example**:
```
Good WiFi (10 Mbps)    → Stream 1080p chunks
Network drops (3 Mbps) → Switch to 480p chunks
Recovers (8 Mbps)      → Resume 720p chunks
```

---

## ⚖️ Trade-offs Analysis

### Consistency vs Availability

**Design Choice**: Prefer availability and low latency over strong consistency

**Acceptable inconsistencies**:
- ❌ **View counts**: May differ across regions (eventually consistent)
- ❌ **Like counts**: Updates propagate within seconds
- ❌ **New video visibility**: Subscribers may see at different times
- ✅ **User authentication**: Strong consistency required
- ✅ **Payment data**: ACID transactions required

**Why acceptable?**
- Video consumption is read-heavy (100:1 read:write ratio)
- Exact counts not critical for user experience
- CAP theorem: Choose AP over CP for video metadata

### Distributed Cache Strategy

**Choice**: Memcached over Redis for video metadata

**Reasoning**:
| Factor | Memcached | Redis | Winner |
|--------|-----------|-------|--------|
| **Throughput** | 1M+ ops/sec | 100K ops/sec | Memcached |
| **Data structure** | Simple objects | Complex types | Memcached (don't need) |
| **Persistence** | No | Yes | Memcached (cache not source) |
| **LRU eviction** | Built-in | Yes | Tie |
| **Memory efficiency** | High | Moderate | Memcached |

**Cache layers**:
1. **Browser cache**: Video chunks (30 min TTL)
2. **CDN cache**: Popular videos (24 hrs)
3. **Application cache**: Metadata (5 min TTL)
4. **Database cache**: Query results (1 min TTL)

### MySQL vs NoSQL for Different Data

**MySQL for User Data**:
- ✅ Strong consistency (authentication, subscriptions)
- ✅ ACID transactions (payment, account changes)
- ✅ Complex queries (user analytics)
- ✅ Structured schema (well-defined user attributes)
- Scale: Vertical + sharding by user_id

**NoSQL (Bigtable) for Thumbnails**:
- ✅ Horizontal scalability (billions of thumbnails)
- ✅ High throughput (key-value lookups)
- ✅ Cost-effective (commodity hardware)
- ✅ Simple access pattern (get by video_id)

**Blob Storage for Videos**:
- ✅ Optimized for large files (GB-sized videos)
- ✅ Petabyte+ scale
- ✅ Integration with CDN
- ✅ Cost-effective for cold storage

![image](https://github.com/user-attachments/assets/bfdff910-3eec-4158-a205-24eea8b51b96)

---

## Scalability Techniques

### 1. Database Sharding

**User Database**: Shard by `user_id % 1000` (1000 shards)
- Each shard: 2M users, ~100GB data
- Allows independent scaling per shard

**Video Metadata**: Shard by `video_id % 10000` (10,000 shards)
- Each shard: 500K videos, ~50GB metadata
- Hot videos may span multiple shards (replicas)

### 2. Asynchronous Processing

**Upload pipeline**:
```
Synchronous:
- Receive video (upload)
- Store temporarily
- Return success to user immediately

Asynchronous (background):
- Encode video (5-30 minutes depending on length)
- Generate thumbnails (1-2 minutes)
- Extract metadata (tags, captions)
- Populate search index
- Update recommendation system
```

**Benefits**:
- User doesn't wait for encoding
- Can batch/schedule encoding jobs
- Retry failures independently

### 3. Content Popularity Prediction

**Machine Learning model predicts**:
- Which videos will go viral (pre-cache to CDN)
- Which old videos trending (move from cold → hot storage)
- Regional popularity (cache in specific geos)

**Signals**:
- Upload channel popularity (subscriber count)
- Initial engagement (likes/views in first hour)
- Social media mentions
- Search trends

---

## 💼 Real-World Architecture Evolution

### YouTube's Storage Journey

**2005-2010**: MySQL + Local storage
- Single data center
- Manual replication
- ~500K videos

**2010-2015**: MySQL sharding + GFS (Google File System)
- Multiple data centers
- Automatic failover
- ~100M videos

**2015-2020**: Bigtable + Colossus (next-gen GFS)
- Global distribution
- Multi-region writes
- ~500M videos

**2020-Present**: Spanner (NewSQL) + Cloud CDN
- Strong consistency with global scale
- Auto-scaling
- ~800M+ videos

---

## ⚠️ Common Pitfalls

1. **Single encoding pipeline**
   - **Problem**: Bottleneck during viral upload spikes
   - **Solution**: Horizontal scaling with job queues (Kafka)

2. **No CDN for all content**
   - **Problem**: Origin servers overwhelmed
   - **Solution**: Multi-tier caching (CDN + Colocation + Origin)

3. **Synchronous video processing**
   - **Problem**: User waits 10+ minutes for upload
   - **Solution**: Asynchronous with notification

4. **Ignoring long-tail distribution**
   - **Problem**: Cache all videos = high cost
   - **Solution**: Adaptive caching based on popularity

5. **No adaptive bitrate**
   - **Problem**: Buffering on slow networks
   - **Solution**: HLS/DASH with multiple resolutions

---

## 📝 Quick Reference

**Scale**: 2B users, 500hrs uploaded/min, 1B hrs watched daily  
**Storage**: 1+ Exabyte of video data  
**Encoding**: 10,000+ parallel workers, 5-30 min per video  
**CDN**: 1000+ PoPs worldwide, 95%+ cache hit ratio  
**Database**: MySQL (users) + Bigtable (thumbnails) + Blob (videos)  
**Consistency**: Eventual for views/likes, strong for auth/payments  
**Key insight**: Content popularity follows power law (5% videos = 95% traffic)

---

<!-- TOC --><a name="8-quora"></a>
# 8. Quora - Q&A Platform

> **📌 Quick Summary**: Knowledge-sharing platform with ML-powered recommendations and ranking  
> **Scale**: 300M+ monthly users, 400M+ questions, real-time content | **Complexity**: ⭐⭐⭐⭐☆

![image](https://github.com/user-attachments/assets/c5b8ad91-5008-46e4-b699-7fef5c446e8b)

### System Requirements

**Functional**:
- ✅ Ask questions, provide answers
- ✅ Upvote/downvote answers
- ✅ Comment on answers
- ✅ Follow topics, users, questions
- ✅ Personalized feed with recommendations
- ✅ Real-time notifications

**Non-Functional**:
- ⚡ **Read latency**: <100ms for feed, <50ms for cached content
- 📈 **Write latency**: <500ms for posts, async for notifications
- 🔒 **Consistency**: Strong for core data (questions, answers), eventual for counts
- 📊 **Availability**: 99.95% uptime
- 🤖 **ML inference**: <200ms for ranking/recommendations

---

## Component Breakdown

| Component | Responsibility | Technology | Purpose |
|-----------|----------------|------------|---------|
| **Load Balancers** | Traffic distribution | HAProxy, NGINX | Handle millions of concurrent users |
| **Web Servers** | API endpoints | Python (Django) | Serve HTTP requests |
| **App Servers** | Business logic | Python, Java | Question/answer processing |
| **MySQL** | Critical data | MySQL (sharded) | Questions, answers, comments, votes |
| **HBase (NoSQL)** | Analytics data | HBase, Cassandra | View counts, scores, extracted features |
| **Blob Storage** | Media files | S3, GCS | Images, videos in posts |
| **Memcached** | Hot data cache | Memcached | Frequently accessed critical data |
| **Redis** | Online counters | Redis | Real-time view counts (INCR) |
| **CDN** | Static content | Cloudflare | Images, videos, CSS, JS |
| **Compute Cluster** | ML/Ranking | Spark, TensorFlow | Recommendations, answer ranking |
| **Kafka** | Event streaming | Apache Kafka | Async tasks, notifications, analytics |
| **Search Engine** | Full-text search | Elasticsearch | Question/answer search |

![image](https://github.com/user-attachments/assets/f0db48d7-31a8-4d82-8053-92c3a8f7d62b)

---

## 🎯 Key Design Decisions

### 1. MySQL vs HBase: Data Segregation

**MySQL for Critical Data** (Strong Consistency):
- Questions, answers, comments (immutable after creation)
- Upvotes, downvotes (transactional updates)
- User profiles, followers
- **Why**: Need ACID, complex queries (joins), strong consistency
- **Scale**: Sharded by `question_id` and `user_id`

**HBase for Analytics Data** (Eventually Consistent):
- View counts (millions of updates/day)
- Answer scores (recomputed periodically)
- ML feature vectors (precomputed, updated batch)
- **Why**: High write throughput, horizontal scaling, BigTable-like interface
- **Scale**: Handles billions of writes/day

**Benefits of separation**:
- MySQL not overwhelmed by high-frequency writes
- HBase optimized for time-series analytics
- Independent scaling of OLTP vs OLAP workloads

### 2. Dual Cache Strategy: Memcached + Redis

**Memcached** (General Caching):
```python
# Frequently accessed questions/answers from MySQL
cache_key = f"question:{question_id}"
cached_data = memcached.get(cache_key)
if cached_data:
    return cached_data
else:
    data = mysql.query(f"SELECT * FROM questions WHERE id={question_id}")
    memcached.set(cache_key, data, ttl=600)  # 10 min
    return data
```

**Redis** (Online View Counter):
```python
# Real-time view count with in-memory increment
def record_view(answer_id):
    redis.incr(f"answer:views:{answer_id}")

def get_view_count(answer_id):
    return redis.get(f"answer:views:{answer_id}") or 0

# Periodically sync to HBase
def sync_to_hbase():
    for key in redis.keys("answer:views:*"):
        answer_id = extract_id(key)
        count = redis.get(key)
        hbase.put(answer_id, "stats:views", count)
        redis.delete(key)  # Clear after sync
```

**Why Redis for counters?**
- ✅ Atomic INCR operation (no race conditions)
- ✅ Sub-millisecond latency
- ✅ Persistence options (RDB/AOF) for durability
- ✅ Supports complex operations (INCRBY, EXPIRE)

**Why not use MySQL for view counts?**
- ❌ Too many writes (1M+ increments/sec)
- ❌ Lock contention on hot rows
- ❌ Disk I/O bottleneck

### 3. Compute Servers for ML Features

**Purpose**: Recommendation and ranking system

**Features extracted** (stored in HBase):
- User engagement history (topics read, answers upvoted)
- Question quality score (grammar, completeness, topic relevance)
- Answer quality score (length, upvotes, author expertise)
- Topic vectors (embeddings for similarity)

**ML Models**:
1. **Answer Ranking**:
   - Inputs: Answer quality, user preferences, recency
   - Output: Ranked list of answers for a question
   - Latency: <200ms (real-time inference)

2. **Feed Recommendations**:
   - Inputs: User history, trending topics, collaborative filtering
   - Output: Personalized question feed
   - Computation: Batch (updated hourly) + real-time adjustments

3. **Topic Suggestions**:
   - Inputs: Question text, historical tagging
   - Output: Relevant topics to follow
   - Computation: Offline (recomputed daily)

**Why separate compute cluster?**
- ML models need high RAM (feature vectors, model weights)
- GPU acceleration for deep learning models
- Independent scaling from application servers

---

## Asynchronous Task Processing

![image](https://github.com/user-attachments/assets/0695a93b-ee0b-4399-b1c9-79613dacc3fa)

### Kafka-Based Event Architecture

**Problem**: Some tasks don't need immediate execution
- View count aggregation (not urgent)
- Email notifications (can be delayed seconds)
- Analytics processing (batch overnight)
- Topic highlighting (updated hourly)

**Solution**: Decouple urgent from non-urgent tasks

```
User Action → API Server → Kafka Topics:
                           ├── view_counter_topic → Cron Job (aggregate views)
                           ├── notification_topic → Notification Service
                           ├── analytics_topic → Analytics Pipeline
                           └── trending_topic → Trending Detector
```

**Benefits**:
- ✅ API servers respond faster (no waiting for secondary tasks)
- ✅ Tasks can retry independently on failure
- ✅ Horizontal scaling of task workers
- ✅ Backpressure handling (queue absorbs spikes)

**Example Flow**:
```
1. User views answer
2. API server responds immediately (50ms)
3. Event published to Kafka: {user_id, answer_id, timestamp}
4. View counter service increments Redis (async)
5. Analytics service logs to HBase (async)
6. Notification service alerts answer author (async, batched)
```

---

## Real-Time Updates: Long Polling

![image](https://github.com/user-attachments/assets/23702cfb-22d2-4d48-a705-bca788384b80)

### Why Not Simple Polling?

**Simple Polling** (client requests every 5 seconds):
```
Client → Server: "Any new comments?"
Server → Client: "No"  (wasted request)
... 5 seconds later ...
Client → Server: "Any new comments?"
Server → Client: "No"  (wasted request)
```

**Problems**:
- 🔥 Server overload (millions of empty requests)
- 🔋 Battery drain on mobile
- ⚡ High latency (up to 5 seconds for new content)

### Long Polling Solution

```
1. Client requests: "Any new comments on answer 123?"
2. Server holds connection (doesn't respond immediately)
3. If update within 60 seconds → respond immediately
4. If no update after 60 seconds → respond "no update"
5. Client immediately makes new long poll request
```

**Benefits**:
- ✅ Real-time feel (<1s latency for new content)
- ✅ Reduces requests by 90%+
- ✅ Server can batch notifications

**Implementation**:
```python
@app.route('/poll/comments/<answer_id>')
def long_poll_comments(answer_id):
    timeout = 60  # seconds
    start_time = time.time()
    last_comment_id = request.args.get('last_id')
    
    while time.time() - start_time < timeout:
        new_comments = get_comments_since(answer_id, last_comment_id)
        if new_comments:
            return jsonify(new_comments)
        time.sleep(1)  # Check every second
    
    return jsonify([])  # No updates
```

**Why not WebSockets?**
- ❌ More complex (persistent connections)
- ❌ Harder to scale (connection state management)
- ❌ Firewall/proxy issues
- ✅ Long polling works everywhere (HTTP)

---

## ⚖️ Trade-offs Analysis

### Strong vs Eventual Consistency

| Data Type | Consistency Level | Reasoning |
|-----------|------------------|-----------|
| **Questions/Answers** | Strong (MySQL) | Core content must be immediately visible to all |
| **Upvotes/Downvotes** | Strong (MySQL) | Prevent double-voting, maintain integrity |
| **View counts** | Eventual (Redis→HBase) | Exact count not critical, high write volume |
| **Answer ranking** | Eventual (batch recompute) | Rankings updated hourly, not real-time |
| **Notifications** | Eventual (Kafka) | Delay of seconds acceptable |

### Cache Invalidation Strategy

**Write-through** (Critical Data):
```python
def update_answer(answer_id, new_text):
    # 1. Update database
    mysql.update(f"UPDATE answers SET text='{new_text}' WHERE id={answer_id}")
    # 2. Invalidate cache
    memcached.delete(f"answer:{answer_id}")
    # Next read will fetch fresh data from DB
```

**Write-behind** (Analytics):
```python
def increment_view_count(answer_id):
    # 1. Update Redis immediately
    redis.incr(f"answer:views:{answer_id}")
    # 2. Async sync to HBase (background job)
```

---

## ⚠️ Common Pitfalls

1. **Using MySQL for high-frequency counters**
   - **Problem**: Lock contention, slow updates
   - **Solution**: Redis for real-time counts, sync to HBase

2. **Synchronous ML inference in API path**
   - **Problem**: Ranking delays response (200ms+ added)
   - **Solution**: Pre-compute rankings, cache results

3. **No separation of OLTP and OLAP**
   - **Problem**: Analytics queries slow down transactional queries
   - **Solution**: MySQL for OLTP, HBase for OLAP

4. **Simple polling for updates**
   - **Problem**: Server overload, high latency
   - **Solution**: Long polling or Server-Sent Events

5. **Not using CDN for media**
   - **Problem**: Origin servers serve all images
   - **Solution**: CDN for images/videos in questions/answers

---

## 📝 Quick Reference

**Scale**: 300M users, 400M questions, 1B+ answers  
**Database**: MySQL (core) + HBase (analytics) hybrid  
**Cache**: Memcached (general) + Redis (counters) dual strategy  
**Async**: Kafka for non-urgent tasks (views, notifications, analytics)  
**Real-time**: Long polling for comments/votes (60s timeout)  
**ML**: Separate compute cluster for ranking/recommendations  
**Key insight**: Separate read-heavy (questions) from write-heavy (counters) workloads

---

<!-- TOC --><a name="9-google-map"></a>
# 9. Google Maps - Navigation & Location Services

> **📌 Quick Summary**: Real-time routing, traffic prediction, and location search at global scale  
> **Scale**: 1B+ users, 220+ countries, 25M+ updates/day | **Complexity**: ⭐⭐⭐⭐⭐

![image](https://github.com/user-attachments/assets/8cd295ae-e8eb-4784-abe6-93632629208e)

### System Requirements

**Functional**:
- ✅ Find shortest path between two locations
- ✅ Real-time traffic updates
- ✅ ETA prediction with current conditions
- ✅ Search places (businesses, landmarks)
- ✅ Turn-by-turn navigation
- ✅ Multi-modal routing (driving, transit, walking, biking)

**Non-Functional**:
- ⚡ **Routing latency**: <500ms for path calculation
- 📈 **Scalability**: Millions of concurrent route requests
- 🎯 **Accuracy**: 95%+ ETA accuracy, <1% routing errors
- 🔄 **Real-time**: Traffic updates every 1-5 minutes
- 🌍 **Global**: 220+ countries, 40M+ km of roads

---

## High-Level Architecture

![image](https://github.com/user-attachments/assets/37d09a35-828b-43f8-b0f6-cd3eca4aaa6f)

### Core Components

| Component | Responsibility | Technology | Scale |
|-----------|----------------|------------|-------|
| **Segment Database** | Road network graph | Distributed DB | 100M+ segments |
| **Segment Adder** | Add new road segments | Microservice | Batch updates |
| **Server Allocator** | Assign segments to servers | Load balancer | 10,000+ servers |
| **Key-Value Store** | Segment → Server mapping | Redis, Bigtable | 100M+ entries |
| **Graph Processing** | Path calculation | Custom C++ | 10,000+ cores |
| **Location Service** | Lat/Long resolution | Distributed search | Billions of queries |
| **Pub-Sub System** | Real-time location streams | Kafka, Pub/Sub | 25M updates/day |
| **Analytics Engine** | Traffic prediction | Spark, ML models | Petabytes/day |
| **Map Tile Server** | Render map images | Custom renderer | CDN-backed |

---

## 🎯 Segment-Based Architecture

### Why Segments?

**Problem**: Can't fit entire world road network on one server
- 40M km of roads
- Billions of intersection nodes
- Graph too large for single-server memory

**Solution**: Divide map into geographic segments

### Segment Design

**Segment properties**:
```json
{
  "segment_id": "seg_12345",
  "boundary": {
    "north": 37.8,
    "south": 37.7,
    "east": -122.3,
    "west": -122.4
  },
  "graph": {
    "nodes": [...],  // Intersections within segment
    "edges": [...]   // Roads within segment
  },
  "border_nodes": [...]  // Connections to adjacent segments
}
```

**Size**: Typically 10km × 10km (varies by road density)
- Urban areas: Smaller segments (more roads)
- Rural areas: Larger segments (fewer roads)

---

## Add Segment Workflow

```
1. New segment data arrives (from mapping team)
   - Boundary coordinates (lat/long)
   - Road network graph

2. Segment Adder processes:
   - Validate segment data
   - Generate unique segment_id (UUID)

3. Server Allocator assigns segment:
   - Find server with capacity
   - Load segment graph onto server
   - Return server_id

4. Store mapping in Key-Value Store:
   - Key: segment_id
   - Value: {server_id, boundary_coords}

5. Update index for lat/long lookups:
   - Geohash index for fast segment discovery
```

---

## Routing Request Flow

![image](https://github.com/user-attachments/assets/d043a28a-df21-4468-9d19-4e5ba7de6d37)

### Step-by-Step

```
1. User provides: Source (lat1, long1) → Destination (lat2, long2)

2. Location Service resolves coordinates:
   - Find which segment contains (lat1, long1) → segment_A
   - Find which segment contains (lat2, long2) → segment_B

3. Graph Processing Service:
   a. Query Key-Value Store:
      - segment_A → server_X
      - segment_B → server_Y
   
   b. If same segment (segment_A == segment_B):
      - Connect to server_X
      - Run Dijkstra/A* within single segment
      - Return path
   
   c. If different segments:
      - Connect to both server_X and server_Y
      - Find border nodes connecting segments
      - Run multi-segment path algorithm (described below)
      - Stitch path together
      - Return complete route

4. ETA Calculation:
   - Apply current traffic conditions
   - ML model predicts future traffic
   - Return distance, time, route polyline
```

### Multi-Segment Routing

**Challenge**: Source and destination in different segments

**Solution**: Hierarchical routing
```
1. Within segment_A:
   - Find path from source to all border nodes
   - Border node A1 is closest exit

2. Between segments (high-level graph):
   - Segment graph: Each segment is one node
   - Border connections are edges
   - Run Dijkstra on segment graph: A → B → C → ... → Z
   - Finds optimal segment sequence

3. Within segment_Z:
   - Find path from border entry to destination

4. Combine paths:
   - Source → A1 (within A)
   - A1 → B1 → ... → Z1 (cross-segment)
   - Z1 → Destination (within Z)
```

**Optimization**: Precompute shortest paths between all border nodes within each segment (done offline)

---

## Real-Time Traffic Integration

### Data Collection

**Sources**:
1. **GPS probes**: Anonymized location data from Android phones
   - 25M+ location updates per day
   - Speed, heading, timestamp
2. **Traffic sensors**: Government-installed sensors
3. **Historical patterns**: Past traffic data for time-of-day predictions

### Architecture

```
Mobile Devices → Pub-Sub System (Kafka) → Data Analytics Engine (Spark)
                                            ↓
                                      ML Models:
                                      - Current traffic speed
                                      - Congestion detection
                                      - Future traffic prediction (15-60 min ahead)
                                            ↓
                                      Traffic Database
                                            ↓
                                      Graph Processing (applies traffic to routing)
```

### Traffic Processing Pipeline

```python
# Pseudo-code
def process_location_stream(location_event):
    # 1. Aggregate locations to road segments
    segment_id = map_to_segment(location_event.lat, location_event.lng)
    road_id = snap_to_road(location_event)
    
    # 2. Calculate speed
    speed = location_event.speed  # km/h
    
    # 3. Update segment traffic
    update_traffic(segment_id, road_id, speed, timestamp)
    
    # 4. Detect anomalies
    if speed < expected_speed * 0.5:
        alert_traffic_jam(segment_id, road_id)

# Batch processing (every 5 minutes)
def update_segment_weights():
    for segment in all_segments:
        for road in segment.roads:
            avg_speed = get_avg_speed(road, last_5_min)
            travel_time = road.length / avg_speed
            update_edge_weight(road, travel_time)  # Used in routing
```

### ML-Based Traffic Prediction

**Features**:
- Historical traffic patterns (same day of week, same time)
- Current traffic speed
- Special events (concerts, sports, holidays)
- Weather conditions

**Model**: LSTM (Long Short-Term Memory) neural network
- Input: Traffic speeds for last 30 minutes
- Output: Predicted speeds for next 60 minutes
- Accuracy: 85-90% for 15-min ahead, 70-75% for 60-min ahead

**Impact on ETA**:
- Static route (no traffic): 30 min
- With current traffic: 42 min
- With predicted traffic: 45 min (accounts for worsening conditions)

---

## ⚖️ Trade-offs Analysis

### Segment Size

| Size | Pros | Cons | Use Case |
|------|------|------|----------|
| **Small (1km × 1km)** | Precise routing, balanced load | More segments, more cross-segment routing | Dense urban (NYC, Tokyo) |
| **Medium (10km × 10km)** | Good balance | - | Suburban areas |
| **Large (50km × 50km)** | Fewer segments, less overhead | Hot segments in cities, unbalanced load | Rural areas |

**Dynamic sizing**: Urban = smaller, Rural = larger

### Routing Algorithm Choice

**Dijkstra**:
- ✅ Guarantees shortest path
- ❌ Explores many unnecessary nodes
- **Use**: Short distances (<50km)

**A* (A-Star)**:
- ✅ Faster than Dijkstra (heuristic guides search)
- ✅ Still guarantees shortest path
- **Use**: Medium distances (50-500km)

**Contraction Hierarchies** (Google's choice):
- ✅ 1000x faster than Dijkstra
- ✅ Preprocessing creates highway hierarchy
- ⚠️ Requires periodic rebuild (when roads change)
- **Use**: Long distances (>500km), production systems

**How Contraction Hierarchies work**:
```
Preprocessing (offline, done once):
1. Identify "highway" nodes (major roads)
2. Contract graph: Shortcut paths through minor roads
3. Create hierarchy: Local roads → Arterials → Highways → Freeways

Query time (online):
1. Shortest path from source to nearest highway
2. Fast routing on highway network (smaller graph)
3. Shortest path from highway to destination
Result: 1000x faster, still optimal path
```

---

## ⚠️ Common Pitfalls

1. **Single-server routing**
   - **Problem**: Can't fit world map in memory
   - **Solution**: Segment-based distribution

2. **Static routing (no traffic)**
   - **Problem**: Inaccurate ETAs, frustrated users
   - **Solution**: Real-time traffic integration

3. **No path precomputation**
   - **Problem**: Slow queries (>10s for long routes)
   - **Solution**: Contraction hierarchies, border node precomputation

4. **Ignoring mobile battery**
   - **Problem**: Constant GPS drains battery
   - **Solution**: Adaptive sampling (1s when navigating, 5min when idle)

5. **Not handling edge cases**
   - **Problem**: Routing through closed roads, wrong-way streets
   - **Solution**: Road metadata (one-way, restrictions, closures)

---

## 📝 Quick Reference

**Scale**: 1B users, 220 countries, 40M km roads  
**Segments**: 10M+ geographic segments, 100K-1M roads each  
**Routing**: Contraction Hierarchies (1000x faster than Dijkstra)  
**Traffic**: 25M location updates/day, 5-min refresh rate  
**Latency**: <500ms for path calculation, <100ms for segment lookup  
**Accuracy**: 95% ETA accuracy with ML traffic prediction  
**Key insight**: Segment-based distribution + hierarchical routing = global scale

---

<!-- TOC --><a name="10-yelp"></a>
# 10. Yelp - Location-Based Business Search

> **📌 Quick Summary**: Place discovery using dynamic QuadTree segments with proximity search  
> **Scale**: 200M+ reviews, 100M+ places, <100ms search latency | **Complexity**: ⭐⭐⭐⭐☆

![image](https://github.com/user-attachments/assets/7a121032-5b3b-4d47-8f78-7940cb912c9d)

### System Requirements

**Functional**:
- ✅ Search nearby places by category (restaurants, hotels, etc.)
- ✅ View business details (ratings, reviews, photos)
- ✅ Filter by distance, rating, price
- ✅ Add/update business information
- ✅ Real-time updates as user moves

**Non-Functional**:
- ⚡ **Search latency**: <100ms for nearby search
- 🎯 **Accuracy**: Return all relevant places within radius
- 📈 **Scalability**: Handle millions of concurrent searches
- 🔄 **Real-time**: Reflect new businesses within minutes
- 🌍 **Global**: 35+ countries, 100M+ places

---

## 🎯 The Core Challenge: Dynamic Segments

### Problem: Fixed-Size Segments Don't Work

**Why not fixed grids?**
```
Manhattan (NYC): 10,000 restaurants per km²
Rural Montana:   5 restaurants per 100 km²
```

**Issues with fixed-size**:
- ❌ Urban areas: Too many places per segment (millions) → slow search
- ❌ Rural areas: Too few places per segment (5-10) → many empty segments
- ❌ Uneven load distribution → some servers overwhelmed, others idle

### Solution: Dynamic QuadTree Segmentation

**Key idea**: Split segments when they exceed a place count threshold

**Algorithm**:
```
1. Start with entire map as one segment (root node)

2. For each segment:
   - Count places in segment
   - If count > 500 → Split into 4 child segments (NW, NE, SW, SE)
   - If count ≤ 500 → Keep as leaf node

3. Result: Leaf nodes = actual searchable segments
   - Each leaf has ≤500 places
   - Smaller segments in dense areas
   - Larger segments in sparse areas
```

**Example**:
```
San Francisco (dense):
- Root segment has 50,000 restaurants
- Split into 4 → each has ~12,500
- Split again → each has ~3,125
- Split again → each has ~780
- Split again → each has ~195 ✓ (below 500 limit)
Result: Small segments (~1km × 1km)

Rural Wyoming (sparse):
- Root segment has 200 restaurants
- No split needed ✓ (below 500 limit)
Result: Large segment (100km × 100km)
```

---

## QuadTree Structure

### Tree Representation

```
                    Root (50,000 places)
                   /      |      |      \
           NW (12,500)  NE (13,000)  SW (12,000)  SE (12,500)
           /  |  |  \
      NW(3K) NE(3K) SW(3K) SE(3K)
      ... continues splitting until ≤500 places per leaf
```

### Node Data Structure

**Internal Node** (has children):
```json
{
  "node_id": "internal_123",
  "boundary": {
    "north": 37.8,
    "south": 37.7,
    "east": -122.3,
    "west": -122.4
  },
  "place_count": 3200,
  "children": [
    "child_nw_id",
    "child_ne_id",
    "child_sw_id",
    "child_se_id"
  ]
}
```

**Leaf Node** (actual segment):
```json
{
  "node_id": "leaf_456",
  "boundary": {...},
  "place_count": 450,
  "places": [
    {"place_id": "p1", "name": "Restaurant A", "lat": 37.75, "lng": -122.35},
    {"place_id": "p2", "name": "Cafe B", "lat": 37.76, "lng": -122.36},
    ...
  ],
  "neighbors": {
    "north": "leaf_457",
    "south": "leaf_458",
    "east": "leaf_459",
    "west": "leaf_460"
  }
}
```

---

## Search Workflow

### Step-by-Step Process

```
User query: "Restaurants within 5km of (37.7749, -122.4194)"

1. Find containing segment:
   - Start at root
   - Check which child contains (37.7749, -122.4194)
   - Traverse to child: NW quadrant
   - Repeat until reaching leaf node
   - Result: leaf_456 (contains user location)
   - Time: O(log N) where N = total segments

2. Get places from current segment:
   - Retrieve leaf_456.places
   - Calculate distance for each place:
     distance = haversine(user_lat, user_lng, place_lat, place_lng)
   - Filter places within 5km
   - Result: 120 restaurants

3. Check if need neighboring segments:
   - User is 1km from segment boundary
   - Search radius is 5km
   - Need to check neighbors? Yes (radius > distance to boundary)

4. Expand to neighboring segments:
   - Use doubly-linked list to find neighbors
   - leaf_456.neighbors → [leaf_457, leaf_458, leaf_459, leaf_460]
   - Fetch places from neighbor segments
   - Filter by 5km radius
   - Result: Additional 40 restaurants from neighbors

5. Aggregate and rank:
   - Total: 160 restaurants within 5km
   - Apply filters (rating > 4.0, open now, category = "Italian")
   - Result: 25 matching restaurants
   - Rank by: distance, rating, popularity
   - Return: Top 10 results
```

### Time Complexity

| Operation | Complexity | Notes |
|-----------|------------|-------|
| **Find leaf segment** | O(log N) | Tree depth ~8-12 levels |
| **Fetch places in segment** | O(1) | Direct lookup, max 500 places |
| **Distance calculation** | O(M) | M = places in segment + neighbors (~2000 max) |
| **Ranking** | O(M log M) | Sort by distance + rating |
| **Total per query** | O(log N + M log M) | Typically <50ms |

---

## Neighbor Connections: Doubly-Linked Lists

### Why Neighbors Matter

**Problem**: Search radius may cross segment boundaries
```
User location: Center of Segment A
Search radius: 5km
Segment size: 2km × 2km

Result: Must search Segment A + 8 neighboring segments
```

### How Doubly-Linked Lists Work

**Structure**: All siblings at same tree level connected
```
     Parent
    /  |  |  \
  NW←→NE←→SW←→SE  (doubly-linked list)
```

**Benefits**:
- ✅ **Fast neighbor lookup**: O(1) instead of tree traversal
- ✅ **Easy expansion**: Move forward/backward through list
- ✅ **Flexible radius**: Add neighbors incrementally until radius satisfied

**Example**:
```python
def find_neighbors(current_segment, depth=1):
    neighbors = []
    
    # Primary neighbors (share edge)
    neighbors.extend([
        current_segment.north,
        current_segment.south,
        current_segment.east,
        current_segment.west
    ])
    
    if depth > 1:
        # Secondary neighbors (diagonal)
        neighbors.extend([
            current_segment.north.east,  # NE
            current_segment.north.west,  # NW
            current_segment.south.east,  # SE
            current_segment.south.west   # SW
        ])
    
    return neighbors
```

---

## ⚖️ Trade-offs Analysis

### QuadTree vs Alternatives

| Approach | Pros | Cons | Best For |
|----------|------|------|----------|
| **QuadTree** | Adaptive sizing, balanced load | Complex implementation, tree updates | Yelp (varied density) |
| **Geohash** | Simple encoding, string prefix | Fixed grid, uneven at boundaries | Caching (Redis geospatial) |
| **S2 Geometry** | Precise coverage, no gaps | Heavy math, overkill for simple search | Google Maps (routing) |
| **Fixed Grid** | Simplest implementation | Poor for varied density | Small regions only |

### QuadTree Parameters

**Place limit per segment (500 in Yelp's case)**:

| Limit | Segment Size (Urban) | Search Time | Tree Depth | Update Cost |
|-------|----------------------|-------------|------------|-------------|
| **100** | Very small (~500m) | Fastest (<10ms) | Deep (~15 levels) | High |
| **500** | Small (~1-2km) | Fast (<50ms) | Medium (~10 levels) | Medium ✅ |
| **2000** | Medium (~5km) | Slower (100ms+) | Shallow (~7 levels) | Low |
| **5000** | Large (~10km+) | Slow (200ms+) | Very shallow (~5 levels) | Very low |

**Yelp's choice (500)**:
- ✅ Balances search speed (<100ms requirement)
- ✅ Reasonable tree depth (~10 levels)
- ✅ Manageable update cost when places added
- ✅ Enough granularity for accurate distance filtering

---

## Implementation Details

### Adding a New Place

```python
def add_place(place):
    # 1. Find leaf segment containing place
    leaf = quadtree.find_leaf(place.lat, place.lng)
    
    # 2. Add place to segment
    leaf.places.append(place)
    leaf.place_count += 1
    
    # 3. Check if split needed
    if leaf.place_count > 500:
        split_segment(leaf)
        # Split into 4 children (NW, NE, SW, SE)
        # Redistribute places among children
        # Update neighbor pointers

# Split operation (expensive but rare)
def split_segment(leaf):
    # Create 4 child nodes
    nw = create_node(leaf.north, leaf.center_lat, leaf.west, leaf.center_lng)
    ne = create_node(leaf.north, leaf.center_lat, leaf.center_lng, leaf.east)
    sw = create_node(leaf.center_lat, leaf.south, leaf.west, leaf.center_lng)
    se = create_node(leaf.center_lat, leaf.south, leaf.center_lng, leaf.east)
    
    # Redistribute places
    for place in leaf.places:
        if place.lat >= leaf.center_lat:
            if place.lng >= leaf.center_lng:
                ne.places.append(place)
            else:
                nw.places.append(place)
        else:
            if place.lng >= leaf.center_lng:
                se.places.append(place)
            else:
                sw.places.append(place)
    
    # Link neighbors
    link_siblings([nw, ne, sw, se])
    update_parent_neighbors(leaf, [nw, ne, sw, se])
    
    # Mark leaf as internal node
    leaf.children = [nw, ne, sw, se]
    leaf.places = None  # No longer stores places
```

---

## ⚠️ Common Pitfalls

1. **Fixed segment size**
   - **Problem**: Urban overload, rural waste
   - **Solution**: Dynamic QuadTree with place count threshold

2. **Missing neighbor connections**
   - **Problem**: Incomplete results near boundaries
   - **Solution**: Doubly-linked list for O(1) neighbor access

3. **Not caching segment tree**
   - **Problem**: Tree traversal on every query
   - **Solution**: Cache leaf nodes in Redis with geohash keys

4. **Synchronous tree updates**
   - **Problem**: Slow writes when splitting segments
   - **Solution**: Async queue for splits, temporary over-limit tolerated

5. **Ignoring user movement**
   - **Problem**: Stale results as user travels
   - **Solution**: Re-query every 500m or 30 seconds

---

## Real-World Optimizations

### Yelp's Production Approach

**Database sharding**:
```
- Shard by geohash prefix (first 4 characters)
- Each shard: ~10M places, ~100GB
- Hot shards (NYC, LA, SF) get more replicas
```

**Caching strategy**:
```
1. Redis cache: Leaf node data (TTL: 10 min)
   Key: geohash:9q8yy → {places: [...], neighbors: [...]}

2. CDN: Popular search results (TTL: 5 min)
   Key: "restaurants near Union Square SF" → Top 20 results

3. Client cache: Recently viewed places (TTL: 1 hour)
```

**Ranking factors**:
```python
score = (
    0.4 * (1 / distance_km) +          # Closer is better
    0.3 * rating / 5.0 +                # Higher rating better
    0.2 * log(review_count) / 10 +     # More reviews = more credible
    0.1 * is_open_now                   # Open businesses prioritized
)
```

---

## 📝 Quick Reference

**Scale**: 200M reviews, 100M places, 35 countries  
**Data structure**: QuadTree with 500 places/segment limit  
**Segment size**: 500m-2km (urban), 10km-100km (rural)  
**Search latency**: <100ms (tree traversal + distance calc + ranking)  
**Neighbors**: Doubly-linked lists for O(1) expansion  
**Update cost**: O(log N) for add, O(N log N) for split (rare)  
**Key insight**: Dynamic segmentation adapts to place density, avoiding hot spots

---

<!-- TOC --><a name="11-uber"></a>
# 11. Uber - Ride-Hailing Platform

> **📌 Quick Summary**: Real-time driver-rider matching with optimized routing and payments  
> **Scale**: 150M+ users, 7M+ drivers, 25M+ trips/day | **Complexity**: ⭐⭐⭐⭐⭐

![image](https://github.com/user-attachments/assets/dd019f80-ee5f-4bf5-b3ce-f0a8dc9248a5)

### System Requirements

**Functional**:
- ✅ Match riders with nearby available drivers
- ✅ Calculate optimal route and ETA
- ✅ Real-time location tracking during trip
- ✅ Process payments securely
- ✅ Ride history and receipts

**Non-Functional**:
- ⚡ **Matching latency**: <5s to find driver
- 📍 **Location accuracy**: <10m GPS precision
- 🔄 **Update frequency**: Driver location every 4s
- 💳 **Payment reliability**: 99.99% success rate
- 📈 **Scalability**: Handle 1M+ concurrent trips
- 🔒 **Availability**: 99.95% uptime (rides must complete)

---

## 🎯 Driver Location Management

### The Challenge: Real-Time Updates at Scale

**Problem**: 7M drivers × location update every 4 seconds = 1.75M updates/sec

**Naive approach issues**:
- ❌ Update QuadTree every 4 seconds → too slow (repartitioning overhead)
- ❌ Search entire driver database → too slow (billions of distance calculations)
- ❌ Fixed grid → hot spots in city centers

### Hybrid Solution: Hash Table + QuadTree

**Two-tier architecture**:

| Data Structure | Purpose | Update Frequency | Use Case |
|----------------|---------|------------------|----------|
| **Hash Table** | Latest driver positions | Every 4 seconds | Find nearby drivers NOW |
| **QuadTree** | Segment-based organization | Every 15 seconds | Historical view, load balancing |

**Why this works**:
- ✅ Hash table: O(1) reads, fast writes, stores latest position
- ✅ QuadTree: Batch updates reduce overhead, still provides spatial indexing
- ✅ Decoupled: Riders use hash table (real-time), analytics use QuadTree (eventual consistency)

---

## Architecture Components

### Data Flow

```
Driver App (GPS) 
   ↓ Every 4 seconds
WebSocket Server
   ↓ Async write
┌────────────────┬────────────────┐
│  Hash Table    │   QuadTree     │
│  (Instant)     │   (Batch 15s)  │
├────────────────┼────────────────┤
│ driver_id →    │ Segments with  │
│ (lat, lng,     │ ≤500 drivers   │
│  timestamp,    │ per leaf       │
│  status)       │                │
└────────────────┴────────────────┘
         ↓                ↓
    Rider Match     Analytics
```

### Hash Table Schema

**Key-Value Structure** (Redis/Cassandra):
```python
# Key: driver_id
# Value: JSON with latest state
{
  "driver_id": "d123",
  "lat": 37.7749,
  "lng": -122.4194,
  "timestamp": 1699999999,
  "status": "available",  # available, busy, offline
  "car_type": "uberX",
  "rating": 4.8
}
```

**Update operation**:
```python
def update_driver_location(driver_id, lat, lng):
    # O(1) hash table update
    redis.hset(f"driver:{driver_id}", {
        "lat": lat,
        "lng": lng,
        "timestamp": time.now(),
        "status": "available"
    })
    
    # Set expiry (cleanup stale drivers)
    redis.expire(f"driver:{driver_id}", 60)  # Expire if no update in 1 min
```

### QuadTree for Segment Organization

**Purpose**: Spatial indexing for analytics and load balancing

**Structure**: Same as Yelp's approach
- Split segment when >500 drivers
- Leaf nodes contain driver lists
- Updated every 15 seconds (batch)

**Why 500 driver limit?**
- Typical segment (2km × 2km in urban area)
- 500 drivers = ~250 drivers per km²
- Search among 500 is fast (<10ms)
- Rare to exceed in most cities

---

## Rider-Driver Matching

### Matching Algorithm

```python
def find_nearby_drivers(rider_lat, rider_lng, radius_km=5, car_type="uberX"):
    # Step 1: Get all drivers from hash table (or sharded by geohash)
    # This is the key optimization - use hash table for real-time data
    
    # Option A: Geohash-based sharding (production approach)
    rider_geohash = geohash.encode(rider_lat, rider_lng, precision=5)
    nearby_geohashes = geohash.neighbors(rider_geohash) + [rider_geohash]
    
    drivers = []
    for gh in nearby_geohashes:
        # Each geohash maps to a Redis shard
        shard_drivers = redis.hgetall(f"drivers:geo:{gh}:*")
        drivers.extend(shard_drivers)
    
    # Step 2: Filter by distance and availability
    available_drivers = []
    for driver in drivers:
        if driver.status != "available":
            continue
        if driver.car_type != car_type:
            continue
        
        distance = haversine(rider_lat, rider_lng, driver.lat, driver.lng)
        if distance <= radius_km:
            available_drivers.append({
                "driver": driver,
                "distance": distance,
                "eta": distance / 30  # Assume 30 km/h average speed
            })
    
    # Step 3: Rank drivers
    # Priority: 1) Distance, 2) Rating, 3) Acceptance rate
    available_drivers.sort(key=lambda d: (
        d["distance"],
        -d["driver"].rating,
        -d["driver"].acceptance_rate
    ))
    
    # Step 4: Send request to top 5 drivers
    return available_drivers[:5]
```

### Matching Flow

```
1. Rider requests ride: (37.7749, -122.4194) → "Downtown SF"

2. Find nearby drivers:
   - Query hash table by geohash (37.774, -122.419)
   - Get 50 drivers within 2km radius
   - Filter: available=True, car_type=uberX
   - Result: 15 available drivers

3. Rank drivers:
   - Driver A: 500m, rating 4.9, acceptance 95%  [Score: 1]
   - Driver B: 800m, rating 4.7, acceptance 90%  [Score: 2]
   - Driver C: 600m, rating 4.8, acceptance 92%  [Score: 3]
   - ...

4. Send ride request to top 5 drivers (parallel):
   - Timeout: 15 seconds for acceptance
   - First to accept gets the ride
   - Others receive cancellation notification

5. If no acceptance within 15s:
   - Expand radius to 5km
   - Retry with next 5 drivers
```

---

## Routing: Shortest Path at Scale

![image](https://github.com/user-attachments/assets/ef333239-b3a0-4777-9b4d-f934da8f6220)

### Why Not Dijkstra?

**Problem**: Dijkstra too slow for production
- NYC road network: 100K+ intersections
- Dijkstra explores 50K+ nodes for 10km route
- Time: 2-5 seconds per query
- Can't scale to 1M+ concurrent rides

### Solution: Contraction Hierarchies

**Key idea**: Preprocess road network into hierarchical layers

**Preprocessing** (done offline, once per day):
```
1. Identify road importance:
   - Freeways: Highest (Level 5)
   - Highways: High (Level 4)
   - Arterial roads: Medium (Level 3)
   - Residential: Low (Level 2)
   - Alleys: Lowest (Level 1)

2. Create shortcuts:
   - Contract minor roads into shortcuts
   - Example: A → B → C (residential) becomes A → C (shortcut)
   - Reduces graph size by 80%+

3. Build hierarchical index:
   - Bottom layer: All roads
   - Middle layers: Progressively major roads only
   - Top layer: Freeways and highways only
```

**Query time** (online, <100ms):
```
1. Source → Nearest major road (Level 3+)
   - Use precomputed paths (local roads only)
   - Time: <10ms

2. Major road routing:
   - Run A* on contracted graph (80% smaller)
   - Only explore highways and freeways
   - Time: <50ms

3. Nearest major road → Destination
   - Use precomputed paths (local roads only)
   - Time: <10ms

Total: <100ms vs 2-5 seconds (Dijkstra)
Speedup: 20-50x faster
```

### Partitioning for Parallel Processing

**Geographic partitions**:
```
Divide city into grids (e.g., 100 partitions for NYC)
- Each partition: 10km × 10km
- Precompute paths within partition
- Store border nodes (connections to other partitions)

Query spanning partitions:
- Partition A: Source → Border node A1 (10ms)
- Partition B: Border node B1 → Border node B2 (10ms)
- Partition C: Border node C1 → Destination (10ms)
- Combine paths: Total 30ms (parallelized across 3 servers)
```

---

## Payment Processing: Apache Kafka

![image](https://github.com/user-attachments/assets/ef333239-b3a0-4777-9b4d-f934da8f6220)

### Event-Driven Payment Flow

```
1. Trip ends → Driver marks "Complete"

2. Order Creator:
   - Calculate fare: distance × rate + surge + tolls
   - Generate order: {trip_id, rider_id, driver_id, amount, timestamp}
   - Publish to Kafka topic: "trip_completed"

3. Kafka processes event:
   - Persists event to disk (durability)
   - Replicated across 3+ brokers (fault tolerance)

4. Order Processor (Consumer):
   - Reads from "trip_completed" topic
   - Validates order (fraud detection, balance check)
   - Creates payment intent
   - Publishes to "payment_intent" topic

5. Payment Service Provider (PSP) Integration:
   - Reads from "payment_intent" topic
   - Calls Stripe/Braintree API
   - Processes card charge
   - Returns result: success/failure

6. Order Writer:
   - Reads PSP result from "payment_result" topic
   - Updates database: trip.status = "paid"
   - Sends receipt to rider (email/SMS)
   - Updates driver earnings

7. Analytics:
   - Kafka streams feed data warehouse
   - Real-time dashboard: payments/sec, success rate
```

### Why Kafka for Payments?

| Requirement | Kafka Benefit |
|-------------|---------------|
| **Durability** | Events persisted to disk, replicated |
| **Retry logic** | Consumers can replay events on failure |
| **Audit trail** | Complete payment history for compliance |
| **Async processing** | Rider doesn't wait for payment (UX improvement) |
| **Fault tolerance** | Multiple brokers, consumer groups |
| **Scalability** | Horizontal scaling (add partitions/consumers) |
| **Decoupling** | Services communicate via events, not direct calls |

---

## Non-Functional Requirements

### Availability (99.95% uptime)

**Strategies**:

1. **WebSocket Redundancy**:
   ```
   - Multiple WebSocket servers per region (20+ per major city)
   - Load balancer distributes connections
   - If connection drops:
     → Client reconnects via load balancer
     → Session ID preserved (stored in Redis)
     → Ride state restored from database
   ```

2. **Database Replication**:
   ```
   - Cassandra cluster (no single point of failure)
   - Replication factor: 3 (each write to 3 nodes)
   - Consistency level: QUORUM (2 of 3 nodes must ack)
   - If node fails → automatic failover to replica
   ```

3. **Multi-Region Deployment**:
   ```
   - Primary region: Handles 90% of traffic
   - Secondary region: Hot standby (replicates in real-time)
   - If primary fails → DNS failover to secondary (RTO: 2 minutes)
   ```

### Scalability

**Horizontal Scaling**:
- Stateless services (matching, routing, payment) → add more instances
- NoSQL (Cassandra) → add more nodes to cluster
- Redis cache → cluster mode with sharding

**Partitioning**:
- Drivers sharded by geohash (spatial locality)
- Trips sharded by `trip_id % 1000` (uniform distribution)
- Payments sharded by `user_id % 100` (user affinity)

---

## ⚠️ Common Pitfalls

1. **Updating QuadTree on every location change**
   - **Problem**: 1.75M updates/sec, constant repartitioning
   - **Solution**: Hash table for real-time, QuadTree batch update (15s)

2. **Synchronous payment processing**
   - **Problem**: Rider waits 5-10s for payment confirmation
   - **Solution**: Kafka async processing, notify later

3. **Single routing server**
   - **Problem**: Bottleneck, can't handle 1M concurrent routes
   - **Solution**: Geographic partitioning + contraction hierarchies

4. **No driver location expiry**
   - **Problem**: Stale drivers shown as available
   - **Solution**: Redis TTL 60s, auto-cleanup inactive drivers

5. **Not handling network partitions**
   - **Problem**: Rider and driver see different ride states
   - **Solution**: Eventual consistency + idempotency keys

---

## 📝 Quick Reference

**Scale**: 150M users, 7M drivers, 25M trips/day  
**Location updates**: 1.75M/sec (drivers every 4s)  
**Data structure**: Hash table (4s) + QuadTree (15s) hybrid  
**Routing**: Contraction Hierarchies (20-50x faster than Dijkstra)  
**Payments**: Kafka event-driven pipeline (async)  
**Database**: Cassandra (no SPOF), Redis (real-time cache)  
**Availability**: 99.95% via WebSocket redundancy + multi-region  
**Key insight**: Dual storage (hash table + QuadTree) balances real-time accuracy with spatial indexing efficiency

---

<!-- TOC --><a name="12-twitter"></a>
# 12. Twitter - Real-Time Microblogging Platform

> **📌 Quick Summary**: Distributed social network with real-time timeline, search, and analytics  
> **Scale**: 450M+ users, 500M+ tweets/day, 100ms search, 15s indexing latency | **Complexity**: ⭐⭐⭐⭐⭐

![image](https://github.com/user-attachments/assets/af48a651-4620-43b9-b9b0-677139170393)

### System Requirements

**Functional**:
- ✅ Post tweets (280 chars, media attachments)
- ✅ Follow users, view timelines
- ✅ Search tweets in real-time
- ✅ Like, retweet, reply
- ✅ Trending topics and hashtags
- ✅ Direct messaging

**Non-Functional**:
- ⚡ **Timeline latency**: <200ms for home timeline
- 🔍 **Search latency**: <100ms with 1 trillion records
- 📈 **Indexing latency**: <15s for new tweets
- 🔄 **Write throughput**: 500M tweets/day (5.8K/sec avg, 20K+/sec peak)
- 📊 **Read throughput**: 400 billion events/day
- 🎯 **Availability**: 99.9% uptime

---

## Data Storage Architecture

![image](https://github.com/user-attachments/assets/3b6dc27e-79d9-4c33-9645-2a922b35ba75)

### Storage Layer Breakdown

| Data Store | Purpose | Technology | Scale | Rationale |
|------------|---------|------------|-------|-----------|
| **Manhattan** | Tweets, accounts, DMs | Distributed KV store | Millions of QPS | Replaced Cassandra in 2014, uses RocksDB engine |
| **Blobstore** | Photos, videos | Object storage | Petabytes | Optimized for large media files |
| **MySQL/PostgreSQL** | Financial data, ads | RDBMS | 100TB+ | Strong consistency for transactions |
| **BigQuery** | Analytics, big data | Data warehouse | Exabytes | Migrated from HDFS for better analytics |
| **FlockDB** | Social graph | Graph database | Billions of edges | Stores followers, followings relationships |
| **Vertica** | Aggregated datasets | Columnar DB | 100TB+ | Fast analytical queries for dashboards |

---

## �� Manhattan: Twitter's Distributed KV Store

### Why Replace Cassandra?

**2010-2014: Cassandra Era**
- ❌ Unexpected performance degradation
- ❌ Debugging difficulties (JVM issues)
- ❌ Operational hassles (compaction storms)
- ❌ GC pauses causing latency spikes

**2014-Present: Manhattan**
- ✅ Built in-house, optimized for Twitter's workload
- ✅ RocksDB storage engine (C++, predictable performance)
- ✅ Multiple clusters by use case (tweets, accounts, DMs)
- ✅ Handles millions of QPS per cluster
- ✅ Low latency: <5ms p99 for reads, <10ms for writes

### Manhattan Architecture

**Cluster segmentation by use case**:
```
Cluster 1: Tweets (write-heavy, 10M QPS)
  - Sharded by tweet_id
  - Replication factor: 3
  - Strong consistency for writes

Cluster 2: User accounts (read-heavy, 5M QPS)
  - Sharded by user_id
  - Read replicas: 5
  - Cache hit ratio: 95%

Cluster 3: Direct messages (balanced, 2M QPS)
  - Sharded by conversation_id
  - Encryption at rest
  - Strong consistency
```

**Key-Value Schema**:
```python
# Tweets
Key: tweet_id (64-bit Snowflake ID)
Value: {
  "user_id": 12345,
  "text": "Hello world",
  "created_at": 1699999999,
  "media_urls": ["blob://..."],
  "hashtags": ["#tech"],
  "mentions": ["@user"],
  "retweet_count": 100,
  "like_count": 500
}

# User timeline (fan-out on write)
Key: f"timeline:{user_id}"
Value: [tweet_id_1, tweet_id_2, ...] (sorted by timestamp)
```

---

## Real-Time Search: Apache Lucene

### The Challenge: 1 Trillion Records

**Requirements**:
- 🔍 Search across 1 trillion tweets
- ⚡ <100ms query latency
- 🔄 <15s indexing latency for new tweets
- 📈 100K+ queries per second

### Two-Tier Index Architecture

**Tier 1: Real-Time Index (RAM)**
```
Coverage: Past 7 days of tweets
Storage: In-memory (RAM)
Size: ~10TB (hot data)
Update latency: <15 seconds
Query latency: <50ms
Technology: Apache Lucene in-memory index

Benefits:
- ✅ Ultra-fast indexing (new tweets searchable in 15s)
- ✅ Low query latency (RAM access)
- ✅ Handles 80% of search queries (users search recent tweets)
```

**Tier 2: Full Index (Disk)**
```
Coverage: All tweets (complete history)
Storage: Distributed disk (SSDs)
Size: ~1PB (100x larger than real-time index)
Update latency: Batch processing (hourly)
Query latency: 100-200ms
Technology: Apache Lucene with disk-based segments

Benefits:
- ✅ Complete historical search
- ✅ Cost-effective (disk cheaper than RAM)
- ✅ Handles 20% of queries (historical deep searches)
```

### Inverted Index Structure

**How it works**:
```
Tweet 1: "Machine learning is amazing"
Tweet 2: "Deep learning models"
Tweet 3: "Machine vision and learning"

Inverted Index:
"machine"  → [1, 3]  (appears in tweets 1 and 3)
"learning" → [1, 2, 3]  (appears in all three)
"deep"     → [2]  (only in tweet 2)
"models"   → [2]
"vision"   → [3]
"amazing"  → [1]

Query: "machine learning"
→ Intersect posting lists: [1, 3] ∩ [1, 2, 3] = [1, 3]
→ Return tweets 1 and 3
→ Rank by: recency, engagement, user relevance
```

### Search Optimization Techniques

**1. Sharding by time**:
```
Shard 1: Tweets from today (hot, RAM)
Shard 2: Tweets from yesterday (warm, SSD)
Shard 3: Tweets from last week (cold, HDD)
...
Shard N: Tweets from years ago (archived)

Query strategy:
- Search recent shards first (most queries stop here)
- Parallel search across historical shards if needed
```

**2. Bloom filters**:
```python
# Before searching a shard, check if term exists
if not bloom_filter.contains("rare_keyword"):
    skip_shard()  # Avoid expensive search
else:
    search_shard()  # Term might exist
```

**3. Query caching**:
```
Cache layer: Redis
TTL: 5 minutes for trending topics, 1 hour for rare queries
Hit ratio: ~40% (popular searches cached)
Latency: <5ms for cache hit vs 100ms for full search
```

---

## Event Processing: Kafka + Cloud Dataflow

### Architecture

```
Client events (tweets, likes, retweets) 
   ↓
Apache Kafka (on-premise)
├── Topic: tweets (500M events/day)
├── Topic: likes (2B events/day)
├── Topic: retweets (800M events/day)
└── Topic: views (10B events/day)
   ↓
Event Processor (converts Kafka → Cloud Pub/Sub)
   ↓
Google Cloud Pub/Sub
   ↓
Cloud Dataflow (streaming ETL)
├── Deduplication (same event received multiple times)
├── Real-time aggregation (count tweets per minute)
└── Windowing (5-min tumbling windows)
   ↓
┌─────────────┬──────────────────┐
│ BigQuery    │ Bigtable         │
│ (Analytics) │ (Serving system) │
└─────────────┴──────────────────┘
```

### Why Kafka + Cloud?

**On-Premise Kafka**:
- ✅ Low latency for real-time features (timelines, notifications)
- ✅ Full control over infrastructure
- ✅ Integration with existing systems

**Google Cloud Dataflow**:
- ✅ Massive scale (400B events/day)
- ✅ Managed service (no ops overhead)
- ✅ Built-in exactly-once semantics
- ✅ Auto-scaling based on backlog

**Hybrid approach benefits**:
- Real-time features use on-prem Kafka (low latency)
- Analytics use Cloud Dataflow (scale + managed)
- Best of both worlds: performance + scalability

---

## Caching: Pelikan

### Evolution of Twitter's Cache

| Era | Technology | Issues | Scale |
|-----|------------|--------|-------|
| **2010-2015** | Twemcache | Performance unpredictability, hard to debug | 100TB+ |
| **2015-2019** | Redis (Nighthawk) | Operational hassles, memory inefficiency | 200TB+ |
| **2019-Present** | Pelikan | High throughput, low latency, predictable | 500TB+ |

### Why Pelikan?

**Design principles**:
- Written in C (vs Java/C++ for competitors)
- Minimal dependencies
- Predictable memory allocation (no GC)
- Modular backends (support multiple protocols)

**Performance**:
```
Throughput: 1M+ ops/sec per instance
Latency: p99 < 1ms (vs 5-10ms for Redis)
Memory efficiency: 30% better than Redis
CPU usage: 50% lower than Memcached
```

**Backend types**:
```
pelikan_twemcache: Drop-in replacement for Twemcache
pelikan_slimcache: Lightweight Memcached/Redis alternative
pelikan_segcache: Segment-based storage (better for TTL)
```

### Cache Usage Patterns

**1. Timeline cache**:
```python
Key: f"timeline:{user_id}"
Value: [tweet_id_list] (first 100 tweets)
TTL: 5 minutes
Hit ratio: 90% (most users refresh frequently)
```

**2. User profile cache**:
```python
Key: f"user:{user_id}"
Value: {name, bio, follower_count, ...}
TTL: 1 hour
Hit ratio: 95% (profiles rarely change)
```

**3. Tweet cache**:
```python
Key: f"tweet:{tweet_id}"
Value: {text, media, engagement_counts, ...}
TTL: 10 minutes
Hit ratio: 80% (viral tweets cached longer)
```

---

## Observability: Zipkin Distributed Tracing

### The Problem: Debugging Microservices

**Scenario**: User reports "home timeline loading slow"

**Challenge**:
- Request touches 20+ services
- Which service is slow?
- Network issue or database issue?
- Affecting all users or specific users?

### Zipkin Solution

**How it works**:
```
1. Client request arrives with trace_id (or generate new one)

2. Each service:
   - Receives request with trace_id
   - Creates span (service name, operation, start_time, duration)
   - Adds span to trace
   - Forwards trace_id to downstream services

3. Spans sent to Zipkin collector:
   API Gateway     [span: 200ms]
     → Auth Service   [span: 50ms]
     → Timeline Service [span: 800ms]  ← Bottleneck found!
       → Manhattan     [span: 10ms]
       → Cache        [span: 5ms]
       → Ranking ML   [span: 780ms]  ← Root cause!

4. Zipkin UI visualizes:
   - Waterfall diagram (dependencies, latencies)
   - Service dependency graph
   - Error rates per service
```

### Sampling Strategy

**Problem**: Tracing 400B events/day = too much overhead

**Solution**: Smart sampling
```
Rule 1: Always trace errors (100%)
Rule 2: Sample 0.1% of successful requests (1 in 1000)
Rule 3: Always trace requests >1s (slow queries)
Rule 4: Sample 10% of specific user IDs (for debugging)

Result: 
- Trace 400M events/day (0.1% of 400B)
- Still catch all errors + slow requests
- Minimal overhead (<1% CPU)
```

---

## ⚖️ Trade-offs Analysis

### Manhattan vs Cassandra

| Aspect | Cassandra | Manhattan | Winner |
|--------|-----------|-----------|--------|
| **Latency** | 10-50ms (JVM GC pauses) | <5ms (C++ RocksDB) | Manhattan |
| **Debugging** | Hard (JVM black box) | Easy (controlled C++) | Manhattan |
| **Ops complexity** | High (compaction storms) | Medium (custom system) | Manhattan |
| **Community** | Large ecosystem | Twitter-specific | Cassandra |
| **Multi-tenancy** | Single cluster | Multiple clusters | Manhattan |

### Real-Time vs Full Index

| Approach | Pros | Cons | Use Case |
|----------|------|------|----------|
| **Single large index** | Simpler architecture | Slow updates, high latency | Small scale |
| **Two-tier (Twitter)** | Fast recent search, complete history | Complex, data duplication | Billions of records |
| **Time-sharded only** | Good performance | Requires querying multiple shards | Medium scale |

---

## Complete System Workflow

### Post Tweet Flow

```
1. User posts tweet via mobile app
   ↓
2. Load balancer → API Gateway
   ↓
3. Tweet service:
   - Generate tweet_id (Snowflake)
   - Store in Manhattan
   - Extract: hashtags, mentions, media
   ↓
4. Media service (if attachments):
   - Upload to Blobstore (photos/videos)
   - Generate CDN URLs
   ↓
5. Fan-out service:
   - Get follower list from FlockDB (1M followers)
   - Write to Redis (timeline cache for active followers)
   - Async write to Manhattan (timeline for all followers)
   ↓
6. Search indexing:
   - Publish to Kafka
   - Lucene ingests within 15 seconds
   ↓
7. Analytics:
   - Kafka → Cloud Pub/Sub → Dataflow → BigQuery
   ↓
8. Notifications:
   - Mentioned users get push notification
```

### Home Timeline Request Flow

```
1. User opens Twitter app
   ↓
2. Load balancer → Timeline service
   ↓
3. Check Pelikan cache:
   - Key: timeline:{user_id}
   - Cache hit (90% case) → Return cached tweets
   ↓
4. Cache miss (10% case):
   - Query Manhattan for timeline:{user_id}
   - Get list of tweet_ids
   - Batch fetch tweet details from Manhattan
   - Rank tweets (ML model):
     * Engagement score (likes, retweets)
     * Recency (newer = higher)
     * User affinity (interaction history)
   - Cache result in Pelikan (TTL: 5 min)
   ↓
5. Enrich tweets:
   - Fetch media URLs from Blobstore
   - Get user profiles from cache
   ↓
6. Return Top 100 tweets to client
```

---

## ⚠️ Common Pitfalls

1. **Single large index**
   - **Problem**: Slow updates, can't search new tweets quickly
   - **Solution**: Two-tier index (real-time + full)

2. **Fan-out for all users**
   - **Problem**: Celebrities with 100M followers = 100M writes per tweet
   - **Solution**: Hybrid (fan-out for normal users, pull for celebrities)

3. **No sampling in tracing**
   - **Problem**: Trace storage explodes, high overhead
   - **Solution**: Sample 0.1% + always trace errors

4. **Monolithic cache**
   - **Problem**: One cache cluster for everything = frequent evictions
   - **Solution**: Separate caches by use case (timeline, user, tweet)

5. **Synchronous indexing**
   - **Problem**: Tweet post blocked by search indexing
   - **Solution**: Async via Kafka, eventual consistency acceptable

---

## 📝 Quick Reference

**Scale**: 450M users, 500M tweets/day, 1 trillion indexed records  
**Storage**: Manhattan (millions QPS), Blobstore (petabytes), FlockDB (billions edges)  
**Search**: Two-tier Lucene (RAM for 7 days, disk for history, <100ms, 15s indexing)  
**Events**: 400B events/day via Kafka → Cloud Dataflow → BigQuery/Bigtable  
**Cache**: Pelikan (1M+ ops/sec, p99 <1ms, 500TB+ total)  
**Tracing**: Zipkin with 0.1% sampling (catch all errors + slow requests)  
**Key insight**: Two-tier index (hot/cold) + hybrid fan-out (celebrities pull, normal fan-out) = scale

---

<!-- TOC --><a name="13-newsfeed"></a>
# 13. Newsfeed System - Social Media Feed

> **📌 Quick Summary**: Personalized content ranking and delivery for social networks  
> **Scale**: Billions of posts, millions of concurrent users, <200ms feed load | **Complexity**: ⭐⭐⭐⭐☆

![image](https://github.com/user-attachments/assets/ec84d0b0-7edd-4ffb-a4fb-a8b4e296f0ac)

### System Requirements

**Functional**:
- ✅ Generate personalized feed for each user
- ✅ Post creation (text, images, videos)
- ✅ Interactions (like, comment, share)
- ✅ Real-time updates for new content
- ✅ Trending topics and recommendations

**Non-Functional**:
- ⚡ **Feed latency**: <200ms for first load
- 🔄 **Real-time**: New posts visible within 5s
- 📈 **Scalability**: Support 1B+ users
- 🎯 **Relevance**: Personalized ranking (ML-powered)
- 💾 **Storage**: Petabytes of user content

---

## 🎯 Core Design: Fan-Out Strategies

### The Fundamental Trade-Off

**Problem**: User posts content → which followers see it immediately?

**Two approaches**:

### 1. Fan-Out on Write (Push Model)

**How it works**:
```
User A posts → Immediately write to all follower timelines

User A (1000 followers) creates post
   ↓
Write service:
  - Write to Follower 1's timeline
  - Write to Follower 2's timeline
  - ... (repeat 1000 times)
  - Write to Follower 1000's timeline
   ↓
When follower opens app:
  - Read own timeline (already populated)
  - Fast: <50ms (simple read)
```

**Pros**:
- ✅ **Fast reads**: Timeline pre-computed, just read from database
- ✅ **Simple read logic**: No aggregation needed
- ✅ **Consistent**: All followers see post immediately

**Cons**:
- ❌ **Slow writes**: If user has 10M followers = 10M writes
- ❌ **Wasted work**: Inactive followers get updates they never see
- ❌ **Storage**: Duplicate posts across all timelines

**Best for**: Users with <10K followers (majority of users)

---

### 2. Fan-Out on Read (Pull Model)

**How it works**:
```
User A posts → Store once in User A's outbox

When follower opens app:
  - Get list of followed users (100 users)
  - Fetch recent posts from each (100 queries)
  - Merge and rank (Top 100 posts)
  - Return to user
```

**Pros**:
- ✅ **Fast writes**: Single write to author's outbox
- ✅ **No wasted work**: Only compute timeline when requested
- ✅ **Storage efficient**: One copy of each post

**Cons**:
- ❌ **Slow reads**: Must aggregate 100+ timelines on each request
- ❌ **Complex**: Merging, ranking, deduplication needed
- ❌ **Latency**: 100+ queries = 500ms+ (unacceptable)

**Best for**: Celebrities with millions of followers

---

### 3. Hybrid Approach (Production Reality)

**Strategy**: Combine both based on user tier

```
Tier 1: Regular users (<10K followers)
  → Use fan-out on write
  → Posts delivered to all followers immediately
  → Fast for both reads and writes

Tier 2: Popular users (10K-1M followers)
  → Hybrid: Fan-out to active followers + pull for inactive
  → Active followers: Last 7 days activity
  → Inactive: Fetch only when they open app

Tier 3: Celebrities (>1M followers)
  → Pure pull model
  → No fan-out (too expensive)
  → Followers pull from celebrity's outbox
  → Cache celebrity posts in CDN (served from edge)
```

### Hybrid Implementation

```python
def create_post(user_id, content):
    # Step 1: Store post in author's outbox
    post_id = generate_id()
    db.save_post(user_id, post_id, content)
    
    # Step 2: Determine fan-out strategy
    follower_count = get_follower_count(user_id)
    
    if follower_count < 10000:
        # Tier 1: Fan-out to all
        followers = get_all_followers(user_id)
        for follower_id in followers:
            write_to_timeline(follower_id, post_id)
    
    elif follower_count < 1000000:
        # Tier 2: Fan-out to active only
        active_followers = get_active_followers(user_id, days=7)
        for follower_id in active_followers:
            write_to_timeline(follower_id, post_id)
        # Inactive followers will pull on demand
    
    else:
        # Tier 3: No fan-out (pure pull)
        # Cache post in CDN for fast access
        cdn.cache(f"post:{post_id}", content, ttl=3600)

def get_timeline(user_id):
    # Step 1: Get pre-computed timeline (fan-out writes)
    timeline_posts = db.get_timeline(user_id, limit=50)
    
    # Step 2: Get posts from celebrities I follow (pull)
    celebrity_follows = get_celebrity_follows(user_id)
    celebrity_posts = []
    for celeb_id in celebrity_follows:
        posts = cdn.get_recent_posts(celeb_id, limit=10)
        celebrity_posts.extend(posts)
    
    # Step 3: Merge and rank
    all_posts = timeline_posts + celebrity_posts
    ranked_posts = rank_posts(all_posts, user_id)
    
    return ranked_posts[:100]
```

---

## Ranking Algorithm

### ML-Based Feed Ranking

**Goal**: Show most relevant posts to each user

**Features** (input to ML model):
```python
Post features:
- Recency (posted in last 5 min = higher)
- Engagement rate (likes, comments, shares per view)
- Media type (video > image > text)
- Topic/hashtags (match user interests)

User-Post affinity:
- Past interactions with author (likes, comments)
- Similarity to liked posts (content-based)
- Time of day (user active at this time?)

Social features:
- Friends who liked this post
- Comments from close friends
- Trending in user's network
```

**Model**: Gradient Boosted Trees or Deep Neural Network
```
Input: 100+ features
Output: Probability user engages with post (0 to 1)

Ranking:
- Sort posts by engagement probability
- Apply diversity (don't show 10 posts from same author)
- Insert ads (every 5th post)
- Return Top 100
```

### Ranking Pipeline

```
Raw timeline (1000 posts from fan-out + pull)
   ↓
Stage 1: Light ranking (filter top 500)
  - Simple features: recency, author popularity
  - Fast model (10ms)
   ↓
Stage 2: Heavy ranking (rank top 500)
  - All features (100+ dimensions)
  - Complex model (100ms)
   ↓
Stage 3: Diversity + business logic
  - Dedup similar posts
  - Inject ads, recommendations
  - Ensure friend posts visible
   ↓
Final feed (100 posts)
```

---

## Real-Time Updates

### Long Polling vs WebSockets

**Long Polling** (Twitter, Facebook approach):
```python
@app.route('/feed/updates')
def check_updates(user_id, last_update_time):
    timeout = 30  # seconds
    start = time.now()
    
    while time.now() - start < timeout:
        new_posts = db.get_posts_since(user_id, last_update_time)
        if new_posts:
            return jsonify(new_posts)
        time.sleep(1)  # Check every second
    
    return jsonify([])  # No updates
```

**Benefits**:
- ✅ Works with standard HTTP
- ✅ No persistent connections
- ✅ Firewall-friendly

**WebSockets** (Instagram, WhatsApp approach):
```python
# Persistent bidirectional connection
@websocket.on('connect')
def on_connect(user_id):
    join_room(f"user:{user_id}")

def new_post_notification(follower_ids, post_id):
    for follower_id in follower_ids:
        emit('new_post', {'post_id': post_id}, 
             room=f"user:{follower_id}")
```

**Benefits**:
- ✅ True real-time (<100ms latency)
- ✅ Lower overhead (no repeated requests)
- ✅ Bidirectional (server can push)

**Hybrid approach**: WebSockets for active users, long polling for others

---

## ⚖️ Trade-offs Analysis

### Fan-Out Write vs Read

| Metric | Fan-Out Write | Fan-Out Read | Hybrid |
|--------|---------------|--------------|--------|
| **Write latency** | Slow (10M writes) | Fast (1 write) | Medium |
| **Read latency** | Fast (<50ms) | Slow (500ms+) | Fast (<200ms) |
| **Storage** | High (duplicates) | Low (single copy) | Medium |
| **Consistency** | Immediate | Eventual | Mixed |
| **Best for** | Normal users | Celebrities | Production |

### Ranking Complexity

**Simple chronological**:
- ✅ Pros: Fast, predictable, fair
- ❌ Cons: Misses relevant content, no personalization

**ML-based ranking**:
- ✅ Pros: Personalized, higher engagement
- ❌ Cons: Complex, expensive, filter bubble concerns

---

## ⚠️ Common Pitfalls

1. **Fan-out to all followers always**
   - **Problem**: Celebrity tweet = 100M writes = system overload
   - **Solution**: Hybrid approach (tier users)

2. **No caching**
   - **Problem**: Recompute ranking on every request
   - **Solution**: Cache ranked feed (TTL: 5 min)

3. **Synchronous ranking**
   - **Problem**: User waits for ML inference
   - **Solution**: Pre-compute rankings offline, serve from cache

4. **Not handling deleted posts**
   - **Problem**: Deleted post still in timelines
   - **Solution**: Lazy deletion (filter on read) + async cleanup

5. **Single global ranking**
   - **Problem**: US posts dominate for Asian users
   - **Solution**: Regional ranking models

---

## 📝 Quick Reference

**Scale**: 1B users, billions of posts/day, <200ms feed load  
**Fan-out**: Hybrid (write for normal, pull for celebrities)  
**Ranking**: Two-stage ML (light filter → heavy rank)  
**Real-time**: Long polling (30s timeout) or WebSockets  
**Cache**: Redis for ranked feeds (5 min TTL)  
**Storage**: Graph DB (social graph) + KV store (posts) + Blob (media)  
**Key insight**: Hybrid fan-out + ML ranking + aggressive caching = fast personalized feeds at scale

---

<!-- TOC --><a name="14-instagram"></a>
# 14. Instagram - Photo/Video Sharing

> **📌 Quick Summary**: Media-heavy social platform with feed, stories, and explore  
> **Scale**: 2B+ users, 95M+ photos/day, 100M+ stories/day | **Complexity**: ⭐⭐⭐⭐☆

![image](https://github.com/user-attachments/assets/c295add8-8097-4665-b3cf-b73c53e1e156)

### System Requirements

**Functional**:
- ✅ Upload photos/videos (up to 10 images, 60s video)
- ✅ Stories (disappear after 24 hours)
- ✅ Feed (posts from followed users)
- ✅ Explore (discover new content)
- ✅ Direct messaging

**Non-Functional**:
- ⚡ **Feed latency**: <200ms
- 📹 **Media latency**: <100ms (CDN-served)
- 💾 **Storage**: Petabytes of images/videos
- 🎯 **Availability**: 99.9% uptime
- 📈 **Scalability**: 2B users, 95M photos/day

---

## 🎯 Core Architecture: Push vs Pull by User Tier

### The Instagram Approach

**Problem**: Different users have vastly different follower counts
- Regular user: 500 followers
- Influencer: 100K followers
- Celebrity: 100M followers

**Solution**: Segment users and apply different strategies

![image](https://github.com/user-attachments/assets/324bb9d4-9f9e-46d0-92ff-40701bbffc3e)

---

## User Segmentation

### Tier 1: Regular Users (Push-Based)

**Criteria**: Followers count < 100K

**Strategy**: Fan-out on write
```python
def post_photo(user_id, photo):
    # 1. Upload to CDN
    photo_url = cdn.upload(photo)
    
    # 2. Save post metadata
    post_id = db.save_post(user_id, photo_url, timestamp)
    
    # 3. Fan-out to all followers
    followers = graph_db.get_followers(user_id)
    for follower_id in followers:
        # Write to follower's feed cache
        redis.zadd(f"feed:{follower_id}", post_id, timestamp)
        
        # Send push notification
        if is_notification_enabled(follower_id):
            push_notification(follower_id, f"{user_id} posted")
    
    return post_id
```

**Benefits**:
- ✅ Fast feed reads (pre-computed)
- ✅ Real-time delivery (followers see immediately)
- ✅ Simple architecture

**Timeline**:
- Post upload: 2s (image processing)
- Fan-out: 5s (500 followers × 10ms each)
- Total: 7s until visible to all

---

### Tier 2: Celebrities (Pull-Based)

**Criteria**: Followers count > 100K

**Strategy**: Fan-out on read (pull model)
```python
def get_feed(user_id):
    # 1. Get users I follow
    following = graph_db.get_following(user_id)
    
    # 2. Separate celebrities from regular users
    celebrities = [u for u in following if is_celebrity(u)]
    regular_users = [u for u in following if not is_celebrity(u)]
    
    # 3. Get posts from regular users (pre-fanned out)
    regular_posts = redis.zrange(f"feed:{user_id}", 0, 50)
    
    # 4. Pull posts from celebrities (query their outbox)
    celebrity_posts = []
    for celeb_id in celebrities:
        # CDN cached (celebrities' posts are hot)
        posts = cdn.get_recent_posts(celeb_id, limit=10)
        celebrity_posts.extend(posts)
    
    # 5. Merge and rank
    all_posts = regular_posts + celebrity_posts
    ranked_posts = rank_algorithm(all_posts, user_id)
    
    return ranked_posts[:100]
```

**Why pull for celebrities?**
- ❌ Kylie Jenner posts photo → 350M followers
- ❌ Fan-out write = 350M database writes = 1 hour to complete
- ✅ Pull on read = Single write to Kylie's outbox + CDN cache
- ✅ Followers pull from CDN (cached, <50ms)

---

## CDN Strategy for Celebrity Content

### Multi-Tier CDN

```
User requests celebrity photo
   ↓
1. Edge CDN (Cloudflare/Akamai)
   - 200+ global PoPs
   - Cache hit: <20ms (90% of requests)
   ↓
2. Regional CDN (mid-tier)
   - 20 major cities
   - Cache hit: <50ms (8% of requests)
   ↓
3. Origin Storage (S3/Blob)
   - Centralized
   - Cache miss: <200ms (2% of requests)
```

### Why CDN for Celebrities?

**Without CDN**:
```
100M followers view post within 1 hour
= 100M requests to origin storage
= Origin servers crash
```

**With CDN**:
```
100M requests
├── 90M served by edge (cached after first request)
├── 8M served by regional CDN
└── 2M reach origin (spread over time)

Result: Origin handles only 2M requests vs 100M
Latency: 20ms (CDN) vs 200ms (origin)
```

---

## Feed Generation Workflow

### Hybrid Feed Architecture

```
User opens Instagram app
   ↓
1. Load balancer → API Gateway
   ↓
2. Feed Service determines strategy:
   - Check user's followed accounts
   - Classify: X regular users, Y celebrities
   ↓
3. Fetch regular users' posts (push):
   - Redis cache: feed:{user_id}
   - Pre-fanned out posts (fast read)
   ↓
4. Fetch celebrity posts (pull):
   - CDN: recent_posts:{celeb_id}
   - Cached at edge (fast read)
   ↓
5. Merge and rank:
   - Total: 500 posts (50 regular + 10 per celeb)
   - ML ranking model (engagement prediction)
   - Apply filters: seen, deleted, blocked
   ↓
6. Return Top 100 posts
   - Client renders feed
   - Prefetch next 100 in background
```

---

## Media Processing Pipeline

### Upload Flow

```
1. User uploads photo/video
   ↓
2. Client-side compression:
   - JPEG quality: 85% (balance size/quality)
   - Video: H.264, 720p default
   ↓
3. Upload to blob storage:
   - S3/Azure Blob/Google Cloud Storage
   - Generate unique URL
   ↓
4. Background processing (async):
   - Generate thumbnails (150x150, 320x320, 640x640)
   - Apply filters (if selected)
   - Video transcoding (multiple resolutions)
   - Face detection (for tagging)
   ↓
5. Push to CDN:
   - Original + thumbnails
   - Predict popularity (ML model)
   - Pre-cache if likely viral
   ↓
6. Update database:
   - Save post metadata
   - Link to CDN URLs
   ↓
7. Fan-out or publish (based on user tier)
```

### Storage Optimization

**Image versions**:
```
Original: 4MB (3024×4032 JPEG)
├── Large: 500KB (1080×1440) - Feed display
├── Medium: 150KB (640×640) - Thumbnails
└── Small: 50KB (320×320) - Grid view

Total per photo: 4.7MB
With 95M photos/day: 447TB/day
Annual: 163PB/year
```

**Cost optimization**:
```
Hot storage (CDN, <7 days):     10PB at $0.10/GB = $1M/month
Warm storage (S3, 7-90 days):   50PB at $0.02/GB = $1M/month
Cold storage (Glacier, >90 days): 100PB at $0.004/GB = $400K/month

Total: $2.4M/month for storage
```

---

## Stories Architecture

### Ephemeral Content (24-Hour Lifespan)

**Why different from regular posts?**
- ⏱️ Auto-delete after 24 hours
- 🎯 Higher engagement (FOMO effect)
- 📊 100M+ stories/day vs 95M posts/day

**Storage strategy**:
```
Stories DB (Cassandra - TTL built-in)
├── Row key: user_id:story_id
├── TTL: 86400 seconds (24 hours)
├── Automatic deletion (no manual cleanup)
└── Significantly less storage than permanent posts

Benefits:
- ✅ No cleanup jobs needed (Cassandra handles TTL)
- ✅ Storage costs 90% lower (temporary data)
- ✅ Fast writes (append-only, no updates)
```

**Viewing workflow**:
```
1. User opens Stories tab
   ↓
2. Fetch stories from followed users:
   SELECT * FROM stories 
   WHERE user_id IN (following_list)
   AND created_at > now() - 24 hours
   ORDER BY created_at DESC
   ↓
3. Prefetch videos:
   - First 3 stories downloaded
   - Rest loaded on swipe (progressive)
   ↓
4. Track views:
   - Increment story.view_count
   - Show to author (who viewed)
```

---

## ⚖️ Trade-offs Analysis

### Push vs Pull by Follower Count

| Followers | Strategy | Write Time | Read Time | Storage | Use Case |
|-----------|----------|------------|-----------|---------|----------|
| **<10K** | Push (fan-out write) | Slow (10s) | Fast (<50ms) | High | 99% of users |
| **10K-100K** | Hybrid | Medium | Medium | Medium | Influencers |
| **>100K** | Pull (fan-out read) | Fast (<1s) | Medium (100ms) | Low | Celebrities |

### CDN vs Origin Serving

**CDN**:
- ✅ Pros: Fast (20ms), scales to billions, reduces origin load
- ❌ Cons: Cost ($1M+/month), cache invalidation complexity

**Origin only**:
- ✅ Pros: Simple, no cache invalidation
- ❌ Cons: High latency (200ms+), can't handle traffic spikes

**Instagram choice**: CDN for all media, especially celebrities

---

## ⚠️ Common Pitfalls

1. **Fan-out to all followers always**
   - **Problem**: Celebrity post = system overload
   - **Solution**: Pull model for high-follower users

2. **No CDN for media**
   - **Problem**: Origin storage can't handle 100M concurrent requests
   - **Solution**: Multi-tier CDN with aggressive caching

3. **Synchronous image processing**
   - **Problem**: User waits 30s for filters/thumbnails
   - **Solution**: Async processing, show upload progress

4. **No thumbnail generation**
   - **Problem**: Loading 4MB images for small grid views
   - **Solution**: Multiple resolutions (small, medium, large)

5. **Permanent storage for stories**
   - **Problem**: Storage costs explode
   - **Solution**: Cassandra TTL (auto-delete after 24h)

---

## 📝 Quick Reference

**Scale**: 2B users, 95M photos/day, 100M stories/day  
**Strategy**: Push for normal users (<100K followers), pull for celebrities  
**CDN**: Multi-tier (edge 90% hit, regional 8%, origin 2%)  
**Storage**: S3/Blob (permanent), Cassandra TTL (stories)  
**Media**: Multiple resolutions (small/medium/large), async processing  
**Feed**: Hybrid (pre-fanned push + celebrity pull + ML ranking)  
**Key insight**: User segmentation (by follower count) enables scaling to billions while maintaining low latency

---

<!-- TOC --><a name="15-whatsapp"></a>
# 15. WhatsApp - Messaging Platform

> **📌 Quick Summary**: End-to-end encrypted messaging with groups, media, and real-time delivery  
> **Scale**: 2B+ users, 100B+ messages/day, <200ms delivery | **Complexity**: ⭐⭐⭐⭐⭐

![image](https://github.com/user-attachments/assets/13e8b5c6-48fc-4dd6-a69e-d907d38bbf28)

### System Requirements

**Functional**:
- ✅ One-on-one messaging (text, media, voice, video)
- ✅ Group chats (up to 256 members)
- ✅ Media sharing (photos, videos, documents)
- ✅ End-to-end encryption (Signal protocol)
- ✅ Message status (sent, delivered, read)
- ✅ Offline message queuing

**Non-Functional**:
- ⚡ **Latency**: <200ms message delivery
- 🔒 **Security**: End-to-end encryption for all messages
- 📈 **Scalability**: 2B users, 100B messages/day
- 🎯 **Consistency**: Messages delivered in order (FIFO)
- 💾 **Availability**: 99.9% uptime
- 📱 **Reliability**: Messages persist until delivered

---

## 🎯 Group Messaging Architecture

### The Challenge

**Problem**: User sends message to group → how to deliver to all members efficiently?

**Naive approach**:
```
User A sends to Group (10 members)
→ 10 separate WebSocket sends
→ Network overhead, latency issues
```

**WhatsApp approach**: Event-driven with Kafka

---

## Group Message Flow

![image](https://github.com/user-attachments/assets/df5a0ba0-3b05-4701-ae1c-a3466b585c1c)

### Step-by-Step Workflow

```
1. User A (connected to WebSocket server) sends message to Group/A
   ↓
2. WebSocket server forwards to Message Service
   - Message service validates:
     * User A is member of Group/A
     * Message not spam
     * Media within size limits (16MB)
   ↓
3. Generate unique message ID (Sequencer/Snowflake)
   - message_id: unique across all messages
   - timestamp: embedded in ID for ordering
   ↓
4. Publish to Kafka
   - Topic: group_messages
   - Partition key: Group/A (ensures FIFO per group)
   - Payload: {
       "message_id": "msg_12345",
       "group_id": "Group/A",
       "sender_id": "user_a",
       "content": "Hello everyone",
       "timestamp": 1699999999,
       "encrypted": true
     }
   ↓
5. Group Message Handler (Kafka consumer)
   - Read from group_messages topic
   - Query Group Service: "Who is in Group/A?"
   ↓
6. Group Service (backed by MySQL + Redis cache)
   - MySQL: Authoritative source
     * groups table: {group_id, name, created_at}
     * group_members table: {group_id, user_id, role}
   - Redis cache: Hot groups cached (TTL: 10 min)
     * Key: group:Group/A
     * Value: [user_b, user_c, user_d, ..., user_j]
   - Return: List of 10 member user_ids
   ↓
7. Group Message Handler iterates members
   For each member (user_b, user_c, ..., user_j):
   
   a. Check if user online:
      - Query WebSocket Manager: "Is user_b connected?"
   
   b. If online:
      - Find WebSocket connection
      - Push message via WebSocket
      - Update message status: "delivered"
   
   c. If offline:
      - Store in offline message queue (MySQL)
      - Push notification via FCM/APNS
      - Message delivered when user reconnects
   ↓
8. Acknowledgment flow
   - User B reads message
   - Send ACK to server
   - Update status: "delivered" → "read"
   - Notify sender (User A) of read status
```

---

## Architecture Components

### WebSocket Management

**Why WebSockets?**
- ✅ Persistent connection (no polling overhead)
- ✅ Bidirectional (server can push)
- ✅ Low latency (<50ms)
- ✅ Battery efficient (vs HTTP polling)

**WebSocket server design**:
```python
class WebSocketManager:
    def __init__(self):
        # In-memory: user_id → WebSocket connection
        self.connections = {}
        # Redis backup (for failover)
        self.redis = Redis()
    
    def on_connect(self, user_id, ws):
        self.connections[user_id] = ws
        # Register in Redis (for cross-server lookup)
        self.redis.hset("ws:connections", user_id, server_id)
        # Set heartbeat (disconnect if no ping in 30s)
        self.start_heartbeat(user_id)
    
    def on_disconnect(self, user_id):
        del self.connections[user_id]
        self.redis.hdel("ws:connections", user_id)
    
    def send_message(self, user_id, message):
        ws = self.connections.get(user_id)
        if ws:
            ws.send(json.dumps(message))
        else:
            # User not on this server, check Redis
            server_id = self.redis.hget("ws:connections", user_id)
            if server_id:
                # Forward to correct server
                forward_to_server(server_id, user_id, message)
            else:
                # User offline, queue message
                queue_offline_message(user_id, message)
```

**Scaling WebSocket servers**:
```
Load balancer (sticky sessions by user_id)
   ↓
├── WebSocket Server 1 (handles 100K connections)
├── WebSocket Server 2 (handles 100K connections)
├── ... (scale horizontally)
└── WebSocket Server N (handles 100K connections)

Total: 2B users / 100K per server = 20,000 servers
Actual: ~50,000 servers (geographic distribution + redundancy)
```

---

## Message Ordering: FIFO Guarantee

### The Problem

**Without ordering**:
```
User A sends:
  Message 1: "What time?" (sent 10:00:00)
  Message 2: "For the meeting?" (sent 10:00:01)

User B receives:
  Message 2: "For the meeting?" (received 10:00:02)
  Message 1: "What time?" (received 10:00:03)

Result: Confusing conversation
```

### Solution: Sequencer + Kafka Ordering

**1. Unique Message IDs** (Snowflake/Sequencer):
```
Message ID format (64-bit):
[Timestamp: 41 bits][Datacenter: 5 bits][Server: 5 bits][Sequence: 13 bits]

Benefits:
- ✅ Globally unique
- ✅ Time-ordered (timestamp embedded)
- ✅ Distributed generation (no central bottleneck)
```

**2. Kafka FIFO per Partition**:
```python
# Publish to Kafka
def send_group_message(group_id, message):
    kafka.produce(
        topic="group_messages",
        key=group_id,  # Same key → same partition → FIFO
        value=message
    )

# Kafka guarantees:
# - Messages with same key go to same partition
# - Within partition: strict order preserved
# - Consumer reads in order

Result: All messages for Group/A processed in order
```

**3. Client-Side Ordering**:
```python
# Client maintains sequence per conversation
def receive_message(message):
    expected_seq = get_last_seq(message.group_id) + 1
    
    if message.sequence == expected_seq:
        # In order, display immediately
        display_message(message)
        update_last_seq(message.group_id, message.sequence)
    else:
        # Out of order, buffer until gap filled
        buffer_message(message)
        request_missing_messages(message.group_id, expected_seq)
```

---

## Non-Functional Requirements

### Latency: Geo-Distributed Architecture

**Global deployment**:
```
User in India → Nearest data center (Mumbai)
User in USA → Nearest data center (Virginia)
User in Europe → Nearest data center (Frankfurt)

Benefits:
- ✅ Low latency (< 50ms to nearest DC)
- ✅ Data sovereignty compliance
- ✅ Disaster recovery (multi-region)
```

**CDN for media**:
```
Photos/Videos uploaded to regional CDN
├── Edge cache (1000+ PoPs worldwide)
├── Origin storage (S3/Blob)
└── Encryption (end-to-end, only sender/receiver can decrypt)

Latency: <100ms for media access (vs 500ms from origin)
```

---

### Consistency: Message Delivery Guarantees

**Three levels**:

| Level | Guarantee | Implementation | Use Case |
|-------|-----------|----------------|----------|
| **At-most-once** | Sent once, may be lost | Fire and forget | Real-time games |
| **At-least-once** | Delivered, may duplicate | Retry until ACK | WhatsApp messages |
| **Exactly-once** | Delivered once, no duplicates | Idempotency key | Financial transactions |

**WhatsApp choice**: At-least-once
```
Why not exactly-once?
- ❌ Higher latency (2-phase commit)
- ❌ More complex
- ✅ At-least-once sufficient (client deduplicates by message_id)

Implementation:
1. Message sent to server
2. Server stores in DB (persistent)
3. Send to recipient
4. If no ACK after 5s, retry
5. Recipient deduplicates by message_id
```

---

### Availability: Multi-Region Replication

**Architecture**:
```
Region 1 (Primary): US-East
├── Active WebSocket servers
├── Kafka cluster (3 brokers)
└── MySQL primary (writes)

Region 2 (Secondary): EU-West
├── Active WebSocket servers
├── Kafka cluster (3 brokers)
└── MySQL replica (read-only)

Region 3 (Tertiary): Asia-Pacific
├── Active WebSocket servers
├── Kafka cluster (3 brokers)
└── MySQL replica (read-only)

Replication:
- Async replication (low latency, eventual consistency)
- Cross-region latency: 50-200ms
- If Region 1 fails → Promote Region 2 to primary (RTO: 5 minutes)
```

---

### Security: End-to-End Encryption

**Signal Protocol**:
```
1. User A and User B exchange public keys (one-time setup)

2. User A sends message:
   - Encrypt with User B's public key
   - Only User B's private key can decrypt

3. Server sees:
   - Encrypted blob (unreadable)
   - Metadata (sender, recipient, timestamp)
   - Cannot read message content

4. User B receives:
   - Decrypt with own private key
   - Display plaintext

Key features:
- ✅ Perfect forward secrecy (keys rotate)
- ✅ Server cannot read messages
- ✅ Even WhatsApp employees cannot decrypt
```

**Group encryption**:
```
Each group has shared secret key
- Generated by group creator
- Distributed to all members via pairwise encrypted channels
- New member joins → Rotate key, redistribute

Encryption:
1. Encrypt message with group key
2. Encrypt group key with each member's public key
3. Send: {encrypted_message, {encrypted_key_for_user_b, encrypted_key_for_user_c, ...}}
```

---

### Scalability: Horizontal Scaling

**Stateless services**:
```
All services scale horizontally:
├── WebSocket servers (add more for more users)
├── Message service (process more messages)
├── Group service (handle more groups)
└── Media service (store more photos/videos)

No shared state (stateless) → Easy scaling
```

**Database sharding**:
```
Users table: Shard by user_id % 1000 (1000 shards)
Messages table: Shard by conversation_id % 10000 (10,000 shards)
Groups table: Shard by group_id % 1000 (1000 shards)

Benefits:
- ✅ Distribute load across many databases
- ✅ Each shard handles smaller dataset
- ✅ Scale to billions of users/messages
```

---

## Non-Functional Requirements Table

![image](https://github.com/user-attachments/assets/df5a0ba0-3b05-4701-ae1c-a3466b585c1c)

| Requirement | Approaches |
|-------------|------------|
| **Minimizing Latency** | • Geographically distributed cache and servers<br>• CDNs for media files |
| **Consistency** | • Unique message IDs via Sequencer<br>• FIFO queue (Kafka partitioning) |
| **Availability** | • Multiple WebSocket servers<br>• Replication of messages and user data<br>• Disaster recovery protocols |
| **Security** | • End-to-end encryption (Signal protocol)<br>• Encrypted storage |
| **Scalability** | • Performance tuning of servers<br>• Horizontal scaling of all services<br>• Database sharding |

---

## ⚠️ Common Pitfalls

1. **No message ordering**
   - **Problem**: Messages arrive out of order, confusing users
   - **Solution**: Kafka partitioning + Sequencer + client-side buffering

2. **Single WebSocket server**
   - **Problem**: Can't handle 2B users
   - **Solution**: 50K+ WebSocket servers with Redis coordination

3. **Synchronous group message delivery**
   - **Problem**: Sending to 256 members sequentially = high latency
   - **Solution**: Kafka async processing, parallel delivery

4. **No offline message queue**
   - **Problem**: Messages lost if recipient offline
   - **Solution**: Persistent queue (MySQL), deliver on reconnect

5. **No encryption**
   - **Problem**: Privacy concerns, government surveillance
   - **Solution**: End-to-end encryption (only sender/receiver can read)

---

## 📝 Quick Reference

**Scale**: 2B users, 100B messages/day, 256 members/group  
**Delivery**: <200ms via WebSocket, Kafka for async group fan-out  
**Ordering**: Sequencer IDs + Kafka FIFO per partition + client buffering  
**Encryption**: Signal protocol (end-to-end, server cannot decrypt)  
**Availability**: Multi-region (US, EU, APAC), async replication  
**Storage**: Sharded MySQL (messages), Redis (cache), S3/CDN (media)  
**Key insight**: Kafka event-driven architecture + WebSocket real-time + end-to-end encryption = secure scalable messaging

---

<!-- TOC --><a name="16-typeahead"></a>
# 16. Typeahead Suggestion System - Autocomplete

> **📌 Quick Summary**: Real-time query suggestions as user types (Google Search, YouTube, Twitter)  
> **Scale**: Billions of queries/day, <100ms suggestion latency | **Complexity**: ⭐⭐⭐⭐☆

![image](https://github.com/user-attachments/assets/87be1f0b-97f7-47d2-8cff-8fce02a0e11c)

### System Requirements

**Functional**:
- ✅ Suggest queries as user types (after 2-3 characters)
- ✅ Rank suggestions by popularity
- ✅ Personalized suggestions based on user history
- ✅ Handle typos and spelling corrections
- ✅ Support trending topics

**Non-Functional**:
- ⚡ **Latency**: <100ms for suggestions
- 📈 **Scalability**: Billions of queries per day
- 🔄 **Freshness**: Trending topics appear within 1 hour
- 💾 **Storage**: 10M+ unique queries
- 🎯 **Accuracy**: Top 5 suggestions relevant 90%+ of time

---

## 🎯 Core Data Structure: Trie (Prefix Tree)

### Why Trie?

**Problem**: Need to find all queries starting with "mac" in <100ms

**Naive approach** (database LIKE query):
```sql
SELECT query, frequency FROM queries 
WHERE query LIKE 'mac%' 
ORDER BY frequency DESC 
LIMIT 10;

Issues:
- ❌ Full table scan (slow for millions of queries)
- ❌ Can't use index efficiently (prefix wildcard)
- ❌ Latency: 500ms+ for large dataset
```

**Trie approach**:
```
Time complexity: O(k) where k = length of prefix
Space complexity: O(n) where n = total characters across all queries

Example trie:
           root
          /    \
        m       p
       /         \
      a           y
     / \           \
    c   n          t
   / \   \          \
  h   e   g         h
  |   |   |         |
 ine ros  o        on
 (5K)(2K)(10K)    (50K)

Queries stored:
- "machine" → frequency: 5K searches/day
- "macneros" → frequency: 2K
- "mango" → frequency: 10K
- "python" → frequency: 50K

Search "mac" → traverse m→a→c → return ["machine": 5K, "macneros": 2K]
Latency: 3 character lookups = <1ms
```

---

## System Architecture

### Components

| Component | Responsibility | Technology | Scale |
|-----------|----------------|------------|-------|
| **Collection Service** | Log user queries | Kafka, Flume | Billions/day |
| **HDFS** | Store raw query logs | Hadoop HDFS | Petabytes |
| **Aggregator** | Compute query frequencies | MapReduce, Spark | Batch processing |
| **Cassandra** | Store aggregated frequencies | Cassandra | 100M+ queries |
| **Trie Builder** | Build/update tries | Custom service | Hourly updates |
| **Trie Database** | Store serialized tries | MongoDB | 10GB per trie |
| **ZooKeeper** | Coordinate trie updates | ZooKeeper | Metadata |
| **Suggestion Service** | Serve suggestions | Custom API | 100K QPS |
| **Redis Cache** | Cache hot suggestions | Redis | Sub-ms latency |

---

## Data Flow Pipeline

### 1. Collection Phase

```
User searches "machine learning"
   ↓
1. Collection Service logs:
   {
     "query": "machine learning",
     "user_id": "user123",
     "timestamp": 1699999999,
     "language": "en",
     "region": "US"
   }
   ↓
2. Publish to Kafka topic: raw_queries
   ↓
3. Kafka consumer writes to HDFS
   - Partitioned by date: /queries/2024/11/10/
   - Format: Parquet (compressed, columnar)
```

---

### 2. Aggregation Phase

```
HDFS (/queries/2024/11/10/)
   ↓
MapReduce/Spark job (runs hourly)
   ↓
Map phase:
  Input: "machine learning", "machine vision", "python tutorial"
  Output: {("machine learning", 1), ("machine vision", 1), ("python tutorial", 1)}
   ↓
Reduce phase:
  Group by query, sum frequencies:
  {
    "machine learning": 5000,
    "machine vision": 2000,
    "python tutorial": 10000
  }
   ↓
Write to Cassandra
  Table: query_frequencies
  Schema: {query (primary key), frequency, timestamp}
```

---

### 3. Trie Building Phase

```
Trie Builder Service (cron job, runs hourly)
   ↓
1. Read from Cassandra:
   SELECT query, frequency FROM query_frequencies
   WHERE timestamp > last_update_time
   
2. Load existing trie from MongoDB (if exists)
   
3. Update trie:
   For each new query:
     - Insert into trie
     - Update frequency at leaf node
     - Propagate frequency update to parent nodes
   
4. Serialize trie:
   - Convert tree to JSON/Protobuf
   - Compress (gzip)
   - Size: ~10GB per trie (English queries)
   
5. Store in MongoDB:
   - Collection: tries
   - Document: {
       "language": "en",
       "region": "US",
       "trie_data": "<serialized>",
       "version": 12345,
       "created_at": 1699999999
     }
   
6. Notify ZooKeeper:
   - Update metadata: /tries/en-US/version → 12345
   - Suggestion servers detect new version
   - Reload trie (hot swap, no downtime)
```

---

### 4. Suggestion Serving Phase

```
User types "mac" in search box
   ↓
1. Client sends request: GET /suggest?q=mac&limit=10
   ↓
2. Suggestion Service:
   a. Check Redis cache:
      Key: suggest:mac
      Hit (95% case): Return cached suggestions (<5ms)
   
   b. Cache miss (5% case):
      - Load trie from memory (trie loaded on startup)
      - Traverse: root → m → a → c
      - Get children: ["machine", "macbook", "macneros", "macros", ...]
      - Sort by frequency
      - Take top 10
      - Cache result in Redis (TTL: 1 hour)
      - Return to client (<50ms)
```

---

## Optimization Techniques

### 1. Reducing Trie Depth

**Problem**: Deep trie = slow traversal

**Solution**: Limit max depth
```
Instead of storing full query:
"how to learn machine learning in 2024" (depth: 46)

Store truncated:
"how to learn machine learning in" (depth: 32)

Benefits:
- ✅ Shallower tree (faster traversal)
- ✅ Less memory
- ⚠️ Slight loss in specificity (acceptable trade-off)
```

---

### 2. Offline Trie Updates

**Problem**: Updating trie in real-time = high CPU

**Solution**: Batch updates (hourly)
```
Real-time path (on user's critical path):
- User types → Fetch suggestions from trie
- Latency requirement: <100ms

Background path (not on critical path):
- Update trie every hour
- CPU-intensive task run during low traffic hours

Result: Low latency for users, trie stays reasonably fresh
```

---

### 3. Geographic Distribution

**Problem**: Single trie server = high latency for global users

**Solution**: Regional tries
```
US users → US data center (Virginia) → en-US trie
EU users → EU data center (Frankfurt) → en-GB trie
Asia users → Asia data center (Singapore) → en-IN trie

Benefits:
- ✅ Low latency (<50ms to nearest DC)
- ✅ Localized suggestions (US: "football" = NFL, EU: "football" = soccer)
- ✅ Data sovereignty compliance
```

---

### 4. Caching Layer (Redis)

**Three-tier caching**:

```
Tier 1: Client-side cache (browser)
  - Cache: Top 100 popular queries
  - TTL: 24 hours
  - Hit ratio: 30% (common queries)
  - Latency: <1ms

Tier 2: Redis cache (server-side)
  - Cache: Hot prefixes (last 1M unique queries)
  - TTL: 1 hour
  - Hit ratio: 60% (frequently searched)
  - Latency: <5ms

Tier 3: Trie (in-memory)
  - Full trie loaded in RAM
  - Updated hourly
  - Hit ratio: 10% (long-tail queries)
  - Latency: <50ms

Total cache hit ratio: 90% (<5ms), 10% miss (<50ms)
```

---

### 5. Trie Partitioning

**Problem**: Single trie too large (10GB+ for billions of queries)

**Solution**: Partition by prefix
```
Partition 1: Queries starting with a-e
Partition 2: Queries starting with f-j
Partition 3: Queries starting with k-o
Partition 4: Queries starting with p-t
Partition 5: Queries starting with u-z

Benefits:
- ✅ Parallel serving (scale horizontally)
- ✅ Smaller tries per partition (faster loading)
- ✅ Fault isolation (one partition fails, others work)

Routing:
- User types "mac" → Route to Partition 3 (k-o)
- Load balancer handles routing
```

---

## Non-Functional Requirements

| Requirement | Approaches |
|-------------|------------|
| **Low Latency (<100ms)** | • Reduce trie depth (truncate long queries)<br>• Update tries offline (not real-time)<br>• Partition tries (parallel serving)<br>• Multi-tier caching (client, Redis, trie)<br>• Geographic distribution |
| **Fault Tolerance** | • Replicate tries (3 copies per trie)<br>• Replicate NoSQL databases (Cassandra RF=3) |
| **Scalability** | • Auto-scaling of suggestion servers<br>• Increase trie partitions<br>• Horizontal scaling of all services |

---

## ⚖️ Trade-offs Analysis

### Trie vs Database

| Approach | Latency | Memory | Update Cost | Best For |
|----------|---------|--------|-------------|----------|
| **Trie** | <50ms | High (10GB+) | Medium (rebuild) | Real-time typeahead |
| **Database (SQL)** | 500ms+ | Low (indexed) | Low (single UPDATE) | Non-real-time search |
| **Elasticsearch** | 100-200ms | Medium | Low (incremental) | Fuzzy search, typos |

**Production approach**: Hybrid
- Trie for exact prefix match (fast path)
- Elasticsearch for fuzzy/typo correction (fallback)

---

## ⚠️ Common Pitfalls

1. **Real-time trie updates**
   - **Problem**: High CPU, blocking user queries
   - **Solution**: Offline updates (hourly batch)

2. **No caching**
   - **Problem**: Every keystroke hits trie
   - **Solution**: Multi-tier cache (client, Redis, trie)

3. **Single global trie**
   - **Problem**: High latency for distant users
   - **Solution**: Regional tries (US, EU, Asia)

4. **Storing full query text in trie nodes**
   - **Problem**: Memory explosion
   - **Solution**: Store only characters, reconstruct query on retrieval

5. **No partitioning**
   - **Problem**: Single trie too large to fit in memory
   - **Solution**: Partition by prefix (a-e, f-j, k-o, p-t, u-z)

---

## 📝 Quick Reference

**Scale**: Billions of queries/day, 10M+ unique queries  
**Data structure**: Trie (prefix tree) with frequency annotations  
**Latency**: <100ms (50ms trie + 5ms cache + 1ms network)  
**Pipeline**: Collection (Kafka) → Aggregation (MapReduce) → Trie building (hourly) → Serving (Redis cache)  
**Cache**: 90% hit ratio (client 30% + Redis 60%)  
**Storage**: MongoDB (tries), Cassandra (frequencies), HDFS (raw logs)  
**Key insight**: Offline trie building + multi-tier caching + geographic distribution = <100ms typeahead at global scale

---

<!-- TOC --><a name="17-google-docs"></a>
# 17. Google Docs - Collaborative Document Editing

> **📌 Quick Summary**: Real-time collaborative editing with conflict resolution and versioning  
> **Scale**: 2B+ documents, millions of concurrent editors, <50ms sync | **Complexity**: ⭐⭐⭐⭐⭐

![image](https://github.com/user-attachments/assets/ac643c4f-36f7-4b4f-a4c3--5d8b76d825ca)

### System Requirements

**Functional**:
- ✅ Real-time collaborative editing (10+ users simultaneously)
- ✅ Conflict resolution (concurrent edits)
- ✅ Version history (restore any past version)
- ✅ Comments and suggestions
- ✅ Offline editing with sync

**Non-Functional**:
- ⚡ **Sync latency**: <50ms for edits to propagate
- 🔄 **Consistency**: Strong consistency (all users see same state)
- �� **Scalability**: Support millions of concurrent editors
- 💾 **Durability**: Never lose user's work
- 🎯 **Availability**: 99.99% uptime

---

## 🎯 The Core Challenge: Conflict Resolution

### The Problem

**Scenario**: Two users edit same document simultaneously

```
Initial state: "Hello World"

User A (position 6):                User B (position 6):
"Hello World"                       "Hello World"
Insert "Beautiful " at pos 6        Insert "Amazing " at pos 6
→ "Hello Beautiful World"           → "Hello Amazing World"

Both send edits to server... which is correct?
```

**Naive approach**: Last write wins
- ❌ User A's edit lost
- ❌ Unpredictable behavior
- ❌ Frustrating user experience

**Solution**: Operational Transformation (OT) or CRDTs

---

## Operational Transformation (OT)

### How OT Works

**Key idea**: Transform conflicting operations so they can be applied in any order

**Example**:
```
Initial: "Hello World" (11 chars)

User A operation: Insert("Beautiful ", pos=6)
User B operation: Insert("Amazing ", pos=6)

Step 1: Server receives both operations
   
Step 2: Assign order (timestamps, or server order)
   - Operation A arrived first → Apply directly
   - Operation B arrived second → Must transform

Step 3: Apply A
   "Hello World" → "Hello Beautiful World" (19 chars)

Step 4: Transform B based on A
   - B wanted to insert at pos 6
   - But A already inserted 10 chars at pos 6
   - Transform: pos 6 → pos 16 (6 + 10)
   - New operation: Insert("Amazing ", pos=16)

Step 5: Apply transformed B
   "Hello Beautiful World" → "Hello Beautiful Amazing World"

Result: Both edits preserved, consistent state
```

### OT Transformation Rules

**Insert vs Insert**:
```
Op1: Insert("A", pos=5)
Op2: Insert("B", pos=5)

If Op1 applied first:
  Transform Op2: pos 5 → pos 6
  Result: "...A B..."

If Op2 applied first:
  Transform Op1: pos 5 → pos 6
  Result: "...B A..."

Depends on: Timestamp or server order
```

**Insert vs Delete**:
```
Op1: Insert("A", pos=5)
Op2: Delete(pos=5, len=1)

Transform Op2 against Op1:
  - Op1 inserted at pos 5, shifts everything right
  - Op2 wanted to delete pos 5
  - Transform: pos 5 → pos 6
  - Result: Delete different character
```

**Delete vs Delete**:
```
Op1: Delete(pos=5, len=3)
Op2: Delete(pos=7, len=2)

Transform Op2 against Op1:
  - Op1 deleted 3 chars starting at 5 (positions 5,6,7)
  - Op2 wanted to delete positions 7,8
  - Position 7 already deleted → Transform: Delete(pos=5, len=2)
```

---

## Alternative: CRDTs (Conflict-free Replicated Data Types)

### How CRDTs Work

**Key idea**: Data structure designed to merge automatically without conflicts

**CRDT for text editing** (e.g., RGA - Replicated Growable Array):
```
Instead of: "Hello World" (string)

Store as: Linked list with unique IDs
[
  {id: "user_a_1", char: "H", prev: null},
  {id: "user_a_2", char: "e", prev: "user_a_1"},
  {id: "user_a_3", char: "l", prev: "user_a_2"},
  {id: "user_a_4", char: "l", prev: "user_a_3"},
  {id: "user_a_5", char: "o", prev: "user_a_4"},
  {id: "user_a_6", char: " ", prev: "user_a_5"},
  {id: "user_a_7", char: "W", prev: "user_a_6"},
  ...
]

Insert operation:
- User A inserts "Beautiful " after char id "user_a_5"
- Creates: {id: "user_a_100", char: "B", prev: "user_a_5"}
- User B inserts "Amazing " after char id "user_a_5"
- Creates: {id: "user_b_200", char: "A", prev: "user_a_5"}

Merge rule (deterministic):
- Both point to same prev ("user_a_5")
- Resolve by: Compare IDs lexicographically
- "user_a_100" < "user_b_200" → A comes first
- Result: "Hello Beautiful Amazing World"

Benefits:
- ✅ No central server needed for conflict resolution
- ✅ Offline editing works seamlessly
- ✅ Commutative (operations can apply in any order)
```

---

## Google Docs Architecture

### Real-Time Collaboration Pipeline

```
User A types "Hello"
   ↓
1. Client-side buffering:
   - Collect keystrokes (buffer for 50ms)
   - Create operation: Insert("H", pos=0), Insert("e", pos=1), ...
   ↓
2. Send via WebSocket to server:
   - Low latency (<20ms)
   - Persistent connection
   ↓
3. Server (Collab Engine):
   - Assign operation ID (server timestamp)
   - Apply OT/CRDT transformation
   - Write to time-series database (operation log)
   - Persist operation: {doc_id, op_id, user_id, op_type, params, timestamp}
   ↓
4. Broadcast to other clients (User B, C, D):
   - Via WebSockets
   - Each client applies operation locally
   - Latency: <50ms (server → client)
   ↓
5. Client-side rendering:
   - Apply operation to local document state
   - Update UI (show "Hello")
   ↓
6. Acknowledgment:
   - Client sends ACK to server
   - Server marks operation as delivered
```

---

## Consistency Mechanisms

### 1. Gossip Protocol (Within Data Center)

**Purpose**: Replicate document state across servers in same data center

```
Server A receives edit
   ↓
Gossip to neighbors:
├── Server B (same DC)
├── Server C (same DC)
└── Server D (same DC)

Benefits:
- ✅ Fast replication (<10ms)
- ✅ No single point of failure
- ✅ Eventually consistent

Gossip algorithm:
1. Server A has new operation
2. Randomly select 3 neighbors
3. Send operation to neighbors
4. Neighbors propagate to their neighbors
5. After log(N) rounds, all servers have operation
```

---

### 2. Time-Series Database (Operation Log)

**Purpose**: Maintain order of all operations for conflict resolution and version history

```
Operations table (Cassandra/Bigtable):
Partition key: doc_id
Sort key: timestamp

Example:
doc_id | timestamp          | user_id | operation
-------|-------------------|---------|-------------------
doc123 | 2024-11-10 10:00:01 | user_a  | Insert("H", 0)
doc123 | 2024-11-10 10:00:02 | user_a  | Insert("e", 1)
doc123 | 2024-11-10 10:00:03 | user_b  | Insert("i", 1)
doc123 | 2024-11-10 10:00:04 | user_a  | Delete(2, 1)

Query:
SELECT * FROM operations 
WHERE doc_id = 'doc123' 
AND timestamp > '2024-11-10 09:00:00'
ORDER BY timestamp ASC

Result: Replay all operations to reconstruct document
```

---

### 3. Cross-Data Center Replication

**Purpose**: Disaster recovery and global availability

```
US Data Center (Primary)
   ↓
Async replication (every 1 second)
   ↓
┌────────────────┬─────────────────┐
│ EU Data Center │ Asia Data Center│
│ (Secondary)    │ (Secondary)     │
└────────────────┴─────────────────┘

Replication strategy:
- Async replication (low latency for users)
- Operation log replicated cross-region
- If US fails → Promote EU to primary (RTO: 5 min)

Data consistency:
- Within DC: Strong consistency (Gossip protocol)
- Cross-DC: Eventual consistency (acceptable trade-off)
```

---

## Non-Functional Requirements

| Requirement | Approaches |
|-------------|------------|
| **Consistency** | • Gossip protocol (within DC replication)<br>• OT/CRDTs (conflict resolution)<br>• Time-series DB (operation ordering)<br>• Cross-DC replication |
| **Latency (<50ms)** | • WebSockets (persistent connection)<br>• Async replication<br>• Optimal DC location (nearest to user)<br>• CDN for media (images/videos)<br>• Redis for CRDTs |
| **Availability (99.99%)** | • Component replication (no SPOF)<br>• Multiple WebSocket servers<br>• Component isolation<br>• Disaster recovery (multi-region) |
| **Scalability** | • Different data stores (time-series, blob, Redis)<br>• Horizontal sharding of RDBMS<br>• CDN for large files |

---

## Why Strong Consistency?

### Comparison: Eventual vs Strong Consistency

**Eventual Consistency** (e.g., Amazon Dynamo):
```
User A edits: "Hello World" → "Hello Beautiful World"
User B edits: "Hello World" → "Hello Amazing World"

Eventual consistency:
- Both edits stored as separate versions
- System eventually reconciles (auto or manual)

Issues for Google Docs:
- ❌ Document suddenly changes (jarring UX)
- ❌ Manual reconciliation tedious
- ❌ Defeats purpose of collaboration
```

**Strong Consistency** (Google Docs):
```
User A edits: "Hello World" → "Hello Beautiful World"
User B edits: "Hello World" → "Hello Amazing World"

Strong consistency:
- Server assigns order (A first, B second)
- OT transforms B based on A
- Result: "Hello Beautiful Amazing World"
- All users see same state immediately

Benefits:
- ✅ Predictable behavior
- ✅ No jarring updates
- ✅ Automatic conflict resolution
```

---

## ⚠️ Common Pitfalls

1. **Last write wins**
   - **Problem**: Concurrent edits lost
   - **Solution**: OT or CRDTs

2. **No operation log**
   - **Problem**: Can't restore past versions
   - **Solution**: Time-series database for all operations

3. **Synchronous cross-DC replication**
   - **Problem**: High latency (200ms+ to distant DC)
   - **Solution**: Async replication, eventual consistency acceptable

4. **No buffering on client**
   - **Problem**: Send every keystroke = network overhead
   - **Solution**: Buffer 50ms, batch operations

5. **Not using WebSockets**
   - **Problem**: HTTP polling = high latency, inefficient
   - **Solution**: WebSockets for bidirectional real-time updates

---

## 📝 Quick Reference

**Scale**: 2B documents, millions of concurrent editors  
**Sync latency**: <50ms (WebSocket + OT/CRDT)  
**Conflict resolution**: Operational Transformation (deterministic ordering)  
**Consistency**: Strong within DC (Gossip), eventual cross-DC  
**Storage**: Time-series DB (operations), Redis (CRDTs), Blob (media)  
**Availability**: Multi-region (US, EU, Asia), Gossip protocol (no SPOF)  
**Version history**: Replay operations from time-series log  
**Key insight**: OT/CRDTs + time-series operation log + WebSockets + Gossip protocol = conflict-free real-time collaboration

---

# 🎉 Phase 4 Complete!

All 11 real-world system designs have been comprehensively refactored with:
- ✅ Scale metrics and requirements
- ✅ Component breakdown tables
- ✅ Architecture diagrams analysis
- ✅ Step-by-step workflows
- ✅ Trade-off comparisons
- ✅ Technology choices with justifications
- ✅ Common pitfalls with solutions
- ✅ Quick reference summaries

**Next**: Phase 5 (Final enhancements - Pattern Index, Decision Trees, Comparison Matrix)

