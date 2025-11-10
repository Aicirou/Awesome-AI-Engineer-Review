# 🎯 Modern System Design - Interview Prep Guide

> **Complete reference for system design interviews** - Covering distributed systems concepts, architectural patterns, real-world implementations, and decision frameworks.

---

## 📚 Table of Contents

### Part I: Core Concepts (1-6)
- **[1. Concepts](#1-concepts)**
  - [1.1 RPC (Remote Procedure Call)](#11-rpc-remote-procedure-call)
  - [1.2 Fault Tolerance](#12-fault-tolerance)
  - [1.3 Load Balancer](#13-load-balancer)
  - [1.4 Databases](#14-databases)
    - [1.4.1 SQL vs NoSQL](#141-sql-vs-nosql)
    - [1.4.2 Data Replication & Consistency](#142-data-replication--consistency)
    - [1.4.3 Sharding](#143-sharding)
    - [1.4.4 Database Optimization](#144-database-optimization)
  - [1.5 Storage](#15-storage)
    - [1.5.1 Object Storage](#151-object-storage)
    - [1.5.2 Merkle Tree](#152-merkle-tree)
- **[2. Content Delivery Network (CDN)](#2-content-delivery-network-cdn)**
- **[3. Cache](#3-cache)**
  - [3.1 Cache Strategies](#31-cache-strategies)
  - [3.2 Memcached vs Redis](#32-memcached-vs-redis)
- **[4. Rate Limiter](#4-rate-limiter)**
- **[5. Distributed Search](#5-distributed-search)**
  - [5.1 Inverted Index](#51-inverted-index)
  - [5.2 MapReduce](#52-mapreduce)
- **[6. Distributed Task Scheduler](#6-distributed-task-scheduler)**

### Part II: Real-World Systems (7-17)
- **[7. YouTube](#7-youtube)**
- **[8. Quora](#8-quora)**
- **[9. Google Maps](#9-google-maps)**
- **[10. Yelp](#10-yelp)**
- **[11. Uber](#11-uber)**
- **[12. Twitter](#12-twitter)**
- **[13. Newsfeed System](#13-newsfeed-system)**
- **[14. Instagram](#14-instagram)**
- **[15. WhatsApp](#15-whatsapp)**
- **[16. Typeahead Suggestion System](#16-typeahead-suggestion-system)**
- **[17. Google Docs](#17-google-docs)**

### Part III: Advanced Design Tools
- **[Pattern Index](#-comprehensive-pattern-index)** - 40+ design patterns with decision frameworks
- **[Decision Trees](#-decision-trees-for-system-design)** - Database, Cache, Rate Limiter, Fan-Out strategies
- **[Technology Comparison Matrix](#-technology-comparison-matrix)** - 25+ systems compared across 8 dimensions

---

## 🎓 How to Use This Guide

### For Interview Preparation
1. **Quick Review**: Start with 📌 Quick Summary blocks in each section
2. **Deep Dive**: Read full sections with real-world examples
3. **Decision Making**: Use comparison tables and trade-off sections
4. **Pattern Recognition**: Reference Pattern Index for common solutions
5. **Practice**: Use Decision Trees to simulate interview scenarios

### Key Features
- ✅ **Complete Coverage**: All major system design topics
- ✅ **Real-World Examples**: Companies and scale metrics
- ✅ **Visual Aids**: 33 architectural diagrams with detailed descriptions
- ✅ **Decision Frameworks**: When to use each technology
- ✅ **Trade-off Analysis**: Pros/cons for every approach
- ✅ **Interview-Optimized**: Structured for quick reference and recall

---

## 📋 SDE-II Interview Cheat Sheet

> **Last-minute review** - Memorize these before your interview!

### Capacity Numbers (Memorize These!)

| Metric | Small | Medium | Large | Massive |
|--------|-------|--------|-------|---------|
| **QPS** | 1K | 10K | 100K | 1M+ |
| **DAU** | 1M | 10M | 100M | 1B+ |
| **Data** | 100GB | 1TB | 10TB | 1PB+ |
| **Latency Target** | <1s | <500ms | <100ms | <50ms |

### Power of 2 Table (Quick Reference)

| Power | Exact Value | Approx | Name |
|-------|-------------|--------|------|
| 2^10 | 1,024 | ~1K | KB |
| 2^20 | 1,048,576 | ~1M | MB |
| 2^30 | 1,073,741,824 | ~1B | GB |
| 2^40 | ~1.1 trillion | ~1T | TB |

### Latency Numbers (2025 Updated)

| Operation | Latency | Example |
|-----------|---------|---------|
| L1 cache | 0.5 ns | CPU register |
| L2 cache | 7 ns | On-chip cache |
| RAM | 100 ns | Main memory |
| SSD read | 150 μs | Local disk |
| Network (same datacenter) | 500 μs | Server-to-server |
| HDD seek | 10 ms | Spinning disk |
| Network (cross-region) | 150 ms | US → Europe |

### Technology Selection Quick Guide

**Database:**
- SQL: Need ACID transactions (banking, e-commerce checkout)
- NoSQL: High scale (>100K QPS) + simple queries (social media, logs)

**Cache:**
- Redis: Need data structures (sorted sets, lists), persistence
- Memcached: Simple key-value, pure in-memory

**Load Balancer:**
- Round Robin: Stateless services, equal server capacity
- Least Connections: Long-lived connections (WebSocket, DB)

**Rate Limiter:**
- Token Bucket: Allow bursts (APIs with occasional spikes)
- Leaky Bucket: Smooth traffic (video streaming, payment processing)

**Message Queue:**
- Use if: Async processing, decoupling services, traffic smoothing
- Skip if: Synchronous user-facing response needed

### Common System Design Patterns

| Pattern | Use Case | Example |
|---------|----------|---------|
| **Consistent Hashing** | Distributed cache/storage | Memcached, Cassandra |
| **Bloom Filter** | Fast existence check | URL deduplication, weak password check |
| **Circuit Breaker** | Prevent cascading failures | Service mesh, API Gateway |
| **CQRS** | Read/write at different scales | Twitter (write 10K TPS, read 300K TPS) |
| **Event Sourcing** | Audit trail, time travel | Banking transactions, order history |
| **Saga** | Distributed transactions | E-commerce checkout (payment + inventory + shipping) |

### CAP Theorem Quick Decision

**Need strong consistency?** (Banking, inventory)
- Choose CP: MongoDB, HBase, Redis Sentinel
- Sacrifice: Availability during network partition

**Need high availability?** (Social media, analytics)
- Choose AP: Cassandra, DynamoDB, CouchDB
- Sacrifice: Temporary inconsistency (eventual consistency)

### Interview Red Flags to Avoid ❌

1. **"Use microservices"** → Start monolith, split when needed
2. **"Use NoSQL for everything"** → SQL is default, NoSQL when >100K QPS
3. **"Use Kubernetes"** → Overkill for <10K users, consider Heroku/EC2
4. **"Blockchain for audit trail"** → Merkle trees provide integrity without overhead
5. **"Real-time ML inference"** → Pre-compute recommendations offline
6. **"Cache everything"** → Cache hot data (80/20 rule), measure hit ratio
7. **"Shard database immediately"** → Try read replicas + vertical scaling first

### Interview Answer Framework (RADIO)

**R**equirements (5 min):
- Functional: Core features (post, like, comment)
- Non-functional: Scale (DAU, QPS), latency (<100ms), availability (99.9%)

**A**PI Design (5 min):
- REST endpoints: POST /api/v1/posts, GET /api/v1/feed
- Request/response format

**D**ata Model (10 min):
- SQL schema or NoSQL structure
- Indexes on query columns
- Sharding strategy

**I**nfrastructure (15 min):
- Draw boxes: LB → App Servers → DB → Cache → CDN
- Explain data flow

**O**ptimizations (10 min):
- Bottlenecks: Database at 10K QPS? → Add read replicas
- Caching: What to cache? → Hot user data, feed top 20 posts
- CDN: Static assets, images, videos

### Final Checklist Before Interview ✅

- [ ] Practiced 5+ system design questions with timer (45 min each)
- [ ] Can draw load balancer, database, cache, CDN in every diagram
- [ ] Know when to use SQL vs NoSQL (default SQL, NoSQL >100K QPS)
- [ ] Can calculate QPS, storage, bandwidth (back-of-envelope)
- [ ] Memorized latency numbers (RAM 100ns, SSD 150μs, Network 500μs)
- [ ] Practiced explaining trade-offs (consistency vs availability)
- [ ] Know 3 real-world examples (Instagram shards PostgreSQL, Netflix uses Cassandra)
- [ ] Can discuss monitoring (metrics, logs, traces)
- [ ] Ready to acknowledge assumptions ("Assuming 80/20 read/write ratio...")
- [ ] Prepared follow-up answers (What if traffic 10x? What if database fails?)

---

# 1. Concepts

> **📌 Section Overview**: Fundamental building blocks of modern distributed systems - from communication protocols to data storage and fault tolerance.

### 📊 Distributed Systems Concepts Overview

![Modern System Design Concepts Overview](https://github.com/user-attachments/assets/2295663b-18fb-4a79-9258-b31b9e0d3638)

**🖼️ Diagram Description:**
This comprehensive architecture diagram illustrates the fundamental concepts that form the backbone of modern distributed systems:

**Core Components:**
- **Communication Layer**: RPC (Remote Procedure Call) protocols enabling inter-service communication
- **Resilience Layer**: Fault tolerance mechanisms including circuit breakers, retries, and failover strategies
- **Distribution Layer**: Load balancers distributing traffic across multiple servers
- **Data Layer**: Database systems (SQL/NoSQL) with replication and sharding strategies
- **Storage Layer**: Object storage systems and Merkle trees for data integrity
- **Delivery Layer**: CDN infrastructure for global content distribution
- **Performance Layer**: Caching strategies (Redis, Memcached) for low-latency access
- **Protection Layer**: Rate limiters preventing system overload
- **Search Layer**: Distributed search with inverted indexes and MapReduce
- **Orchestration Layer**: Task schedulers coordinating distributed workloads

**Key Relationships:**
- All components interconnect through load balancers ensuring high availability
- Data flows from storage → cache → CDN for optimized delivery
- Fault tolerance mechanisms protect every layer
- Scaling happens horizontally across all tiers

**Interview Relevance:**
- Shows how concepts layer together in production systems
- Demonstrates separation of concerns (SoC) principle
- Illustrates CAP theorem trade-offs across different layers
- Highlights where bottlenecks typically occur (database, cache, network)

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

![Rate Limiter Algorithm Comparison](https://github.com/user-attachments/assets/be78a775-8195-4dfb-8802-3c194be21692)

**🖼️ Rate Limiter Algorithms - Visual Comparison:**

This diagram illustrates the four main rate limiting algorithms with their operational mechanics:

**1. Token Bucket Algorithm** (Top Left):
- **Mechanism**: Bucket fills with tokens at fixed rate; requests consume tokens
- **Visualization**: Tokens flow in continuously (5 tokens/sec), requests drain bucket
- **Burst Handling**: Allows bursts up to bucket capacity (10 tokens max)
- **Use Case**: API gateways needing flexible burst tolerance (AWS API Gateway, Kong)
- **Pros**: Smooth rate limiting, handles bursty traffic gracefully
- **Cons**: Can deplete bucket during sustained high traffic

**2. Leaky Bucket Algorithm** (Top Right):
- **Mechanism**: Requests queue in bucket, processed at fixed rate (FIFO)
- **Visualization**: Requests enter bucket, leak out at constant rate
- **Burst Handling**: Queues excess requests, rejects when bucket full
- **Use Case**: Traffic shaping, network packet scheduling (Nginx rate limiting)
- **Pros**: Smooths out traffic spikes, predictable output rate
- **Cons**: Old requests processed first (may timeout), fixed processing rate

**3. Fixed Window Counter** (Bottom Left):
- **Mechanism**: Count requests in fixed time windows (e.g., per minute)
- **Visualization**: Time divided into discrete windows, counter resets at boundaries
- **Burst Handling**: Can allow 2x rate at window boundaries (edge case)
- **Use Case**: Simple implementations, quota systems (Twitter API: 15 requests/15min)
- **Pros**: Memory efficient O(1), easy to implement
- **Cons**: Traffic spike at window boundaries, uneven rate limiting

**4. Sliding Window Log** (Bottom Right):
- **Mechanism**: Track timestamp of each request, count in rolling time window
- **Visualization**: Continuous sliding window maintaining request timestamps
- **Burst Handling**: Most accurate, no boundary issues
- **Use Case**: Strict rate enforcement, financial systems (Stripe: 100 req/sec)
- **Pros**: Most accurate, no edge cases, fair distribution
- **Cons**: Higher memory O(N), more complex implementation

**Interview Decision Framework:**
- **Bursty APIs**: Token Bucket (AWS, Google Cloud)
- **Traffic Shaping**: Leaky Bucket (Nginx, HAProxy)
- **Simple Quotas**: Fixed Window (Twitter, GitHub)
- **Strict Billing**: Sliding Window Log (Stripe, payment processors)
- **Balanced Choice**: Sliding Window Counter (combines accuracy + efficiency)

**Real-World Hybrid Approaches:**
- **Cloudflare**: Token Bucket (1st tier) + Sliding Window (2nd tier)
- **Redis Rate Limiting**: Fixed Window (fast path) + Sliding Log (strict mode)
- **Kong API Gateway**: Token Bucket with configurable refill strategies

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

**🔍 Image Description - Inverted Index Structure & Query Flow**: This diagram illustrates how inverted indexes enable fast full-text search:
- **Forward index (top)**: Traditional document-to-terms mapping showing Document 1 → ["the", "quick", "brown", "fox"], Document 2 → ["the", "lazy", "dog"], Document 3 → ["dogs", "are", "loyal"]. Arrow indicating this approach requires scanning all documents for each query.
- **Inverted index (bottom)**: Flipped term-to-documents mapping showing efficient lookup structure:
  - Term "quick" → [Doc1] with metadata (position: 1, frequency: 1)
  - Term "brown" → [Doc1] with position: 2
  - Term "dog" → [Doc2, Doc3] with positions (after stemming "dogs" → "dog")
  - HashMap visualization with O(1) term lookup time
- **Query execution flow**: Arrows showing how a search query "brown dog" is processed:
  1. **Tokenization**: "brown dog" → ["brown", "dog"] tokens
  2. **Index lookup**: Fetch posting lists for each term in parallel
  3. **Boolean operations**: Merge posting lists (AND/OR/NOT operations)
  4. **Ranking**: Apply TF-IDF or BM25 scoring to sort results by relevance
  5. **Result set**: Return sorted list [Doc1, Doc2] with relevance scores
- **Posting list details**: Each entry shows document ID, term frequency (TF), term positions (for phrase queries), and document length normalization factor.
- **Optimization annotations**:
  - **Skip pointers**: Arrows showing jump optimization in long posting lists (skip over irrelevant sections)
  - **Compression**: Delta encoding for document IDs (store differences: 1, 5, 3 instead of 1, 6, 9)
  - **Bloom filters**: Quick negative lookup before accessing posting lists
- **Distributed architecture**: Shows how index is sharded across multiple nodes:
  - **Document partitioning**: Each shard contains full index for subset of documents
  - **Term partitioning**: Alternative approach with each shard handling subset of terms
  - Comparison table showing trade-offs (document partitioning: balanced load vs term partitioning: fewer shards per query)
- **Modern implementations (2024-2025)**: Elasticsearch/OpenSearch using Lucene inverted indexes with vector embeddings for semantic search (hybrid keyword + neural retrieval), Algolia with typo-tolerance (edit distance), and real-time indexing (<1s latency from write to searchable). Also shows integration with transformer-based reranking models (BERT, T5) for relevance improvements.

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

**🔍 Image Description - MapReduce Inverted Index Building Process**: This diagram shows the distributed indexing workflow using MapReduce paradigm:
- **Input layer (top)**: Large document corpus split across multiple nodes (Doc1, Doc2, Doc3...DocN) stored in distributed file system (HDFS/S3), each document ~1MB-10GB size.
- **Map phase (parallel processing)**:
  - Each mapper processes a subset of documents independently
  - Tokenization: "The quick brown fox" → ["quick", "brown", "fox"]
  - Emit intermediate key-value pairs: (term, doc_id) tuples
  - Example mapper output: ("quick", Doc1), ("brown", Doc1), ("fox", Doc1), ("lazy", Doc2), ("dog", Doc2)
  - Shows N mapper nodes processing in parallel, each handling 1000-10000 documents
- **Shuffle/Sort phase (middle layer)**:
  - Group all values by key (term) across all mappers
  - Arrows showing data redistribution/partitioning by term hash
  - Network shuffle illustrated with colored data streams converging
  - Sorting within each group to prepare for reduce phase
  - Example grouped data: "brown" → [Doc1, Doc5, Doc12], "dog" → [Doc2, Doc8, Doc15]
- **Reduce phase (aggregation)**:
  - Each reducer processes one term across all documents
  - Build posting list for each term with document IDs and metadata
  - Calculate term frequency (TF), document frequency (DF), and TF-IDF scores
  - Output: Final inverted index entry: "brown" → [(Doc1, TF:1, pos:2), (Doc5, TF:2, pos:0,7), (Doc12, TF:1, pos:5)]
  - M reducer nodes, each handling subset of terms
- **Output layer (bottom)**: Final inverted index stored in distributed storage, sharded by term or document. Shows index segments ready for search queries.
- **Performance metrics**: Annotations showing typical throughput: 1TB corpus indexed in 1-2 hours using 100-node cluster, with 500GB/hour processing rate.
- **Optimization highlights**:
  - **Combiner optimization**: Local aggregation at mapper to reduce shuffle data (50-70% reduction)
  - **Partitioning strategy**: Hash-based term partitioning ensures even distribution across reducers
  - **Compression**: Gzip intermediate data during shuffle saves network bandwidth
- **Modern alternatives (2024-2025)**: Apache Spark RDD operations replacing MapReduce (10x faster with in-memory processing), Apache Flink for real-time stream indexing, and cloud-native solutions like AWS Glue/EMR with serverless auto-scaling.

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


# 5. Distributed Task Scheduler

> **📌 Quick Summary**: Reliable execution of periodic and delayed tasks across distributed systems  
> **Use Cases**: Cron jobs, background processing, workflow orchestration | **Complexity**: ⭐⭐⭐⭐⭐

![Distributed Task Scheduler Architecture](https://github.com/user-attachments/assets/5de17b3e-0048-4481-8507-1391d2e2e749)

**🖼️ Distributed Task Scheduler - Complete Architecture:**

This diagram illustrates a production-grade distributed task scheduling system with fault tolerance and scalability:

**Core Components Flow:**

1. **Task Submission Layer** (Left):
   - **API Gateway**: Receives task creation requests from clients
   - **Task Validator**: Checks task parameters, schedule format, priority
   - **Task Store (Primary DB)**: Persists task definitions (PostgreSQL/MySQL with ACID guarantees)
   
2. **Coordination Layer** (Center):
   - **ZooKeeper Cluster**: Distributed coordination and leader election
     - Maintains worker registry (which workers are alive)
     - Stores task assignments (which worker handles which task)
     - Provides distributed locks for exclusive task execution
   - **Leader Election**: Ensures single scheduler master at any time
   
3. **Scheduling Engine** (Center-Right):
   - **Priority Queues**: Tasks organized by execution time (heap data structure)
   - **Task Dispatcher**: Assigns tasks to available workers based on:
     - Worker capacity (current load)
     - Task affinity (pin tasks to specific workers)
     - Geographic proximity (reduce network latency)
   - **Delayed Queue**: Holds future tasks, promotes to ready queue when time arrives
   
4. **Execution Layer** (Right):
   - **Worker Pool**: Multiple worker nodes executing tasks in parallel
   - **Task Executor**: Runs actual task logic (sandboxed/containerized)
   - **Health Monitor**: Heartbeats to ZooKeeper every 30s
   - **Result Reporter**: Sends execution status back to coordinator
   
5. **Reliability Mechanisms**:
   - **Watchdog Service**: Detects worker failures, reassigns orphaned tasks
   - **Retry Handler**: Implements exponential backoff for failed tasks
   - **Dead Letter Queue**: Stores tasks that failed after max retries
   - **Audit Log**: Records all task state transitions for debugging

**Data Flow (Task Lifecycle):**
1. Client submits task → API Gateway validates → Store in DB
2. Scheduler reads tasks → Checks execution time → Adds to priority queue
3. Dispatcher assigns task → Worker receives via message queue
4. Worker executes → Reports status → Updates task state
5. On failure → Retry handler kicks in → Requeue or move to DLQ

**Fault Tolerance Strategy:**
- **Worker Failure**: ZooKeeper detects missing heartbeat → Watchdog reassigns task to healthy worker
- **Scheduler Failure**: New leader elected via ZooKeeper → Reads task state from DB → Resumes scheduling
- **Network Partition**: Tasks remain in persistent queue, retry after partition heals
- **Database Failure**: Replicas provide read access, writes queued until primary recovers

**Interview Key Points:**
- **At-Least-Once Execution**: Task may run multiple times on failure (requires idempotency)
- **Exactly-Once Semantics**: Use distributed transactions (2PC) or idempotency keys
- **Scalability**: Add workers horizontally, partition task queue by hash(task_id)
- **Real-World Systems**: Celery (Python), Sidekiq (Ruby), AWS Step Functions, Airflow

**Scale Metrics:**
- **Airflow (Uber)**: 500K+ tasks/day across 100+ workers
- **Celery (Instagram)**: Millions of tasks/hour for async processing
- **AWS Step Functions**: Handles billions of state transitions/month

---

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


# 6. Sharded Counters

> **📌 Quick Summary**: Distributed counter system to handle high-frequency increments  
> **Use Cases**: View counts, likes, real-time analytics | **Complexity**: ⭐⭐⭐⭐☆

![Sharded Counters Architecture](https://github.com/user-attachments/assets/0d3f9fff-271a-4693-a6d2-138add59b885)

**🖼️ Sharded Counters - High-Throughput Write Distribution:**

This diagram demonstrates how sharded counters solve the hot key problem in high-traffic scenarios:

**Problem Scenario (Top - Before Sharding):**
- **Single Counter**: `video:abc123:views = 10,245,332`
- **Write Bottleneck**: All increment operations hit one Redis key/database row
- **Contention**: Lock conflicts when multiple servers increment simultaneously
- **Performance Degradation**: 
  - Without sharding: ~1,000 writes/sec (limited by single key locks)
  - Network latency amplified: Every increment waits for previous to complete
  - Database hot spot: CPU spikes on single partition

**Solution Architecture (Bottom - With Sharding):**

1. **Shard Distribution Strategy**:
   - Counter split into N shards (typically 10-100 based on write volume)
   - Each increment randomly selects a shard: `hash(request_id) % num_shards`
   - Shards stored as separate keys: `video:abc123:views:shard_0`, `video:abc123:views:shard_1`, etc.

2. **Write Flow** (Illustrated Left to Right):
   ```
   Client Request → Load Balancer → Application Server
   → Shard Selector (hash function) → Redis/DB Shard
   → Increment shard_X counter
   ```

3. **Read Aggregation Flow**:
   ```
   Read Request → Fetch all shards in parallel
   → Sum(shard_0 + shard_1 + ... + shard_N)
   → Cache aggregated result (1min TTL)
   → Return total count
   ```

4. **Shard Management**:
   - **Shard 0-9**: Each maintains independent counter
   - **Write Distribution**: Random selection ensures even load (law of large numbers)
   - **Read Optimization**: Aggregate values cached to avoid repeated summation
   - **Consistency Model**: Eventually consistent (acceptable for view counts)

**Performance Comparison:**

| Metric | Single Counter | Sharded (10 shards) | Sharded (100 shards) |
|--------|---------------|---------------------|---------------------|
| Write Throughput | 1K writes/sec | 10K writes/sec | 100K writes/sec |
| Write Latency | 50ms (p99) | 5ms (p99) | 1ms (p99) |
| Read Latency | 1ms | 5ms (10 parallel reads) | 20ms (100 parallel reads) |
| Contention | High | Low | Very Low |

**Real-World Implementation:**

**YouTube View Counter:**
- 100 shards per video
- Increments distributed across shards
- Aggregated count computed every 5 minutes
- Cached in CDN for 1 minute
- Handles 40K+ view increments/sec for viral videos

**Redis Implementation:**
```python
# Increment (Fast Write)
shard_id = hash(video_id + timestamp) % NUM_SHARDS
redis.incr(f"video:{video_id}:views:shard_{shard_id}")

# Read (Parallel Aggregation)
pipeline = redis.pipeline()
for i in range(NUM_SHARDS):
    pipeline.get(f"video:{video_id}:views:shard_{i}")
counts = pipeline.execute()
total_views = sum(int(c or 0) for c in counts)
```

**Trade-Offs:**
- ✅ **Pros**: 
  - Linear scalability of writes (add more shards)
  - Reduced lock contention and hot key issues
  - Simple implementation (just modulo hashing)
  
- ❌ **Cons**:
  - Reads more expensive (must aggregate N shards)
  - Eventually consistent (brief delay in total count)
  - Over-sharding increases read latency (find balance)

**Interview Decision Points:**
- **How many shards?** Start with 10, increase based on write load (monitor p99 latency)
- **When to aggregate?** Cache aggregated value, refresh periodically (trade freshness for performance)
- **Consistency requirements?** If strict consistency needed, use distributed locks (slower but accurate)
- **Read vs Write ratio?** High read: fewer shards + longer cache. High write: more shards + shorter cache.

**Similar Use Cases:**
- **Twitter**: Tweet like counts (50+ shards per tweet)
- **Facebook**: Post engagement metrics (distributed across datacenters)
- **Reddit**: Upvote/downvote counters (sharded + eventual consistency)
- **Quora**: Answer view tracking (time-series sharding)

---

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


# 7. YouTube - Video Streaming Platform

> **📌 Quick Summary**: Large-scale video sharing platform with encoding, storage, and CDN delivery  
> **Scale**: 2B+ users, 500hrs uploaded/min, 1B+ hours watched daily | **Complexity**: ⭐⭐⭐⭐⭐

![YouTube High-Level Architecture](https://github.com/user-attachments/assets/cdd2be4a-a2b6-4cf8-8f17-e753afd0ad9d)

**🖼️ YouTube High-Level Upload and Streaming Flow:**

This diagram illustrates YouTube's simplified upload-to-delivery pipeline:

**Upload Pipeline (Left to Right):**
1. **User Upload** → Web servers receive video files (up to 256GB per video)
2. **Metadata Storage** → MySQL stores video metadata (title, description, uploader ID, timestamp)
3. **Temporary Storage** → Video stored temporarily while awaiting encoding
4. **Encoding Queue** → Tasks queued for video processing (2.1 in diagram)
5. **Encoder + Transcoder** (2.2 in diagram):
   - **Compression**: Reduce file size using H.264/H.265 (VP9/AV1 for efficiency)
   - **Multi-Resolution Transcoding**: Generate 2160p/1440p/1080p/720p/480p/360p/240p/144p
   - **Thumbnail Extraction**: Create 3-4 thumbnail options at different timestamps
   - **Audio Extraction**: Separate audio tracks for adaptive streaming
6. **Blob Storage** → Processed videos stored in distributed storage (similar to GFS/S3)
7. **CDN Distribution** → Popular videos pushed to CDN edge locations

**Streaming Pipeline (Right to Left):**
1. **User Request** → Client requests video via player
2. **CDN Delivery** → If cached at CDN edge (low latency ~1-5ms)
3. **Origin Fetch** → If not cached, fetch from blob storage (higher latency ~50-200ms)
4. **Adaptive Bitrate** → Client selects resolution based on network speed

**Key Design Decisions:**
- **Asynchronous Processing**: Upload completes immediately, encoding happens in background
- **CDN Strategy**: Only push popular videos to CDN (80/20 rule - 20% videos = 80% views)
- **Multi-Resolution Storage**: Store all resolutions to avoid real-time transcoding
- **Blob Storage Choice**: Optimized for large objects, sequential reads, high throughput

**Interview Talking Points:**
- Why async encoding? User experience (don't block upload), resource optimization (batch processing)
- CDN vs Origin serving: Cost trade-off (CDN expensive but fast, origin cheaper but slower)
- Storage explosion: 1 video → 8 resolutions × 1.5 codec overhead = 12x storage cost
- Handling viral videos: Predictive CDN push using ML models (trending detection)

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

![YouTube Detailed Component Architecture](https://github.com/user-attachments/assets/34180cb8-fefd-40b0-8d0a-48944237cd43)

**🖼️ YouTube Detailed Multi-Tier Storage and Component Architecture:**

This diagram reveals YouTube's production-grade architecture with specialized component responsibilities:

**Frontend Layer:**
- **Load Balancers**: Distribute traffic across regions using GeoDNS (route users to nearest datacenter)
- **Web Servers**: Handle HTTP requests, serve HTML/JS/CSS for player interface
- **Application Servers**: Business logic layer (authentication, authorization, recommendation engine)

**Data Storage Tier (Multi-Database Strategy):**
1. **MySQL (User + Metadata Storage)**:
   - **User Data**: Profiles, subscriptions, playlists, watch history
   - **Video Metadata**: Title, description, tags, upload timestamp, channel ID
   - **Relational Data**: Comments, likes (foreign keys to user_id, video_id)
   - **Sharding Strategy**: Shard by user_id (user data) and video_id (video metadata)
   
2. **Bigtable (Thumbnail Storage)**:
   - **Why Bigtable?** Optimized for high throughput key-value storage (millions of QPS)
   - **Data**: Thumbnails (each video has 3-4 thumbnails, each <10MB)
   - **Access Pattern**: Read-heavy, rarely updated after upload
   - **Scale**: Billions of thumbnails, petabytes of data

3. **Upload Storage (Temporary Buffer)**:
   - **Purpose**: Hold raw uploaded videos before encoding completes
   - **Technology**: Fast SSD storage for quick writes
   - **Retention**: Delete after encoding completes (24-48 hours)
   - **Scale**: Handles 500+ hours of video uploaded per minute

**Video Processing Pipeline:**
- **Encoders**: Worker pool consuming tasks from encoding queue
  - CPU-intensive: Use GPU clusters for faster encoding (NVIDIA Tesla for H.264)
  - Parallel processing: Split long videos into chunks, encode in parallel, stitch together
  - Priority queue: Verified channels get faster processing
  
- **Blob Storage (Video Files)**:
  - **Technology**: GFS (Google File System) or S3-like distributed object storage
  - **Data**: All video resolutions (144p through 2160p/4K)
  - **Redundancy**: 3x replication across datacenters
  - **Access Pattern**: Write-once, read-many (WORM)
  - **Cost Optimization**: Cold videos moved to cheaper archival storage (Nearline/Coldline)

**Content Delivery Strategy (Three-Tier Serving):**

1. **CDN (Edge Locations)**:
   - **Purpose**: Serve most popular content with lowest latency
   - **Cache Hit Rate**: ~90% for viral videos
   - **Technology**: Google Global Cache, Akamai, Cloudflare
   - **TTL**: Popular videos cached for days, trending content for hours
   
2. **Colocation Sites**:
   - **Purpose**: Regional serving tier between CDN and origin
   - **Use Case**: Moderately popular content (10K-1M views)
   - **Location**: Internet Service Provider (ISP) datacenters (negotiated deals)
   - **Cost**: Cheaper than full CDN, closer than origin datacenter
   - **Example**: YouTube caches at Comcast, Verizon, AT&T facilities
   
3. **Origin Servers (Blob Storage)**:
   - **Purpose**: Long-tail content (older videos, niche content)
   - **Cache Miss Rate**: ~10% of requests fall through to origin
   - **Latency**: 200-500ms (acceptable for less popular content)

**Key Architectural Insights:**

**Cost Optimization:**
- **CDN ($$$)**: Only 20% of videos (viral content)
- **Colocation ($$)**: 30% of videos (moderately popular)
- **Origin ($)**: 50% of videos (long-tail content)
- **Total Savings**: 60% cost reduction vs serving everything from CDN

**Scalability Numbers:**
- **500+ hours** of video uploaded per minute
- **1 billion+ hours** watched daily
- **Encoding Throughput**: Process 8 resolutions per video in <10 minutes (parallel GPU encoding)
- **Storage Growth**: +100 petabytes per year

**Interview Trade-Off Discussions:**

| Decision | Why This Choice? | Trade-Off |
|----------|-----------------|------------|
| **MySQL for Users** | Strong consistency, ACID transactions | Limited horizontal scaling (use sharding) |
| **Bigtable for Thumbnails** | High throughput, low latency key-value | No complex queries (must know exact key) |
| **Multi-Tier CDN** | Cost-performance balance | Complexity in cache invalidation |
| **Async Encoding** | Better UX (fast upload completion) | Delay before video available (1-10 min) |
| **Multiple Resolutions** | Adaptive bitrate streaming | 12x storage overhead (amortized over views) |

**Real-World Numbers (2024):**
- **Daily Views**: 5+ billion
- **Peak Concurrent Users**: 30+ million
- **Average Video Size (1080p)**: 1GB per 10 minutes
- **Encoding Cost**: $0.01-0.05 per minute of video (GPU cluster amortized)
- **Storage Cost**: $0.02/GB/month (decreases to $0.001/GB for cold storage)

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

![YouTube Storage Strategy - Cost vs Performance Trade-offs](https://github.com/user-attachments/assets/bfdff910-3eec-4158-a205-24eea8b51b96)

**🖼️ YouTube Multi-Tier Storage Strategy:**

This diagram illustrates YouTube's intelligent three-tier serving infrastructure balancing cost, performance, and scale:

**Tier 1: CDN (Content Delivery Network)**
- **Purpose**: Lowest latency for most popular videos (hot content)
- **Coverage**: Top 20% of videos generating 80% of traffic (Pareto principle)
- **Cache Hit Rate**: 85-95% for viral/trending content
- **Latency**: 1-10ms (edge locations near users)
- **Cost**: Highest ($$$) - $0.08-0.15 per GB served
- **Use Case**: Newly uploaded videos from popular channels, trending content, recently watched

**Tier 2: Colocation Centers**
- **Purpose**: Regional caching for moderately popular videos (warm content)
- **Partnership Model**: YouTube installs cache servers inside ISP datacenters (Comcast, Verizon, AT&T)
- **Coverage**: 30-40% of videos with 10K-1M views
- **Latency**: 10-50ms (within ISP network, no internet backbone traversal)
- **Cost**: Medium ($$) - $0.02-0.05 per GB (negotiated wholesale rates with ISPs)
- **Benefit**: Reduces ISP bandwidth costs (win-win), improves user experience

**Tier 3: Origin Servers (Blob Storage)**
- **Purpose**: Long-tail content, older videos, niche content (cold content)
- **Coverage**: 50-60% of videos with <10K views
- **Latency**: 50-300ms (datacenter to user, multiple hops)
- **Cost**: Lowest ($) - $0.01-0.02 per GB from origin storage
- **Storage Tier**: Hot data on SSD, cold data on HDD, archival on tape/Nearline
- **Access Pattern**: Infrequent views, acceptable higher latency

**Decision Flow (Video Serving Logic):**
```
1. User requests video → GeoDNS routes to nearest edge
2. Check CDN cache → Hit? Serve immediately (1-5ms)
3. Cache miss → Check colocation center → Hit? Serve (10-50ms)
4. Colocation miss → Fetch from origin → Serve (50-300ms) + cache at colocaton
5. Popular videos promoted to CDN after threshold views
```

**Cost Optimization Calculations:**
- **Scenario**: 1 billion video views per day, average 5MB per view = 5 petabytes/day
- **All-CDN**: 5PB × $0.12/GB = $600K/day = $220M/year
- **Hybrid (20% CDN, 30% Colocation, 50% Origin)**:
  - CDN: 1PB × $0.12 = $120K/day
  - Colocation: 1.5PB × $0.03 = $45K/day
  - Origin: 2.5PB × $0.015 = $37.5K/day
  - **Total**: $202K/day = $74M/year
  - **Savings**: $146M/year (66% cost reduction)

**Consistency vs Availability Trade-Offs:**
- **View Counters**: Eventually consistent (acceptable delay of 5-10 minutes)
- **Video Availability**: Strongly consistent (video must be watchable immediately after encoding)
- **Recommendation Feed**: Eventually consistent (personalization can lag by hours)
- **User Data** (subscriptions, playlists): Strongly consistent (MySQL with ACID)

**Database Choice Rationale:**
- **MySQL for User/Metadata**: Structured data, relational queries (joins), ACID transactions needed
- **Bigtable for Thumbnails**: High-throughput key-value access, no complex queries, petabyte-scale
- **Why NOT NoSQL for Users?** Need strong consistency (subscriptions, billing), complex queries (recommendations)

**Interview Trade-Off Questions:**
1. **Q**: Why not serve everything from CDN? **A**: Cost-prohibitive ($220M vs $74M), 50% of videos watched <10 times
2. **Q**: Why use Bigtable for thumbnails vs MySQL? **A**: Bigtable handles 10M+ reads/sec, thumbnails don't need relations
3. **Q**: Consistency model for view counts? **A**: Eventually consistent (sharded counters, acceptable 5min delay)
4. **Q**: How handle CDN cache invalidation? **A**: Short TTL (1 hour) for trending content, versioned URLs for updates

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


# 8. Quora - Q&A Platform

> **📌 Quick Summary**: Knowledge-sharing platform with ML-powered recommendations and ranking  
> **Scale**: 300M+ monthly users, 400M+ questions, real-time content | **Complexity**: ⭐⭐⭐⭐☆

![Quora System Architecture](https://github.com/user-attachments/assets/c5b8ad91-5008-46e4-b699-7fef5c446e8b)

**🖼️ Quora Polyglot Persistence Architecture:**

Quora's architecture demonstrates **polyglot persistence** - using multiple specialized databases for different data types:

**Storage Layer Breakdown:**
1. **MySQL** (Relational Data): Questions, answers, comments, users - needs strong consistency and ACID transactions
2. **HBase** (Analytics): View counts, rankings, ML features - high-throughput writes, batch processing
3. **Blob Storage**: Images/videos in posts - served via CDN
4. **Memcached**: Query result caching - reduces MySQL load
5. **Redis**: Real-time counters - atomic increments for view counts

**Key Insight**: Using Redis for counters (10K+ increments/sec) frees MySQL for critical transactional data. HBase handles analytics workloads (feature extraction, ranking computation) that would overwhelm relational databases.

---

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

![Quora ML Compute Servers Architecture](https://github.com/user-attachments/assets/f0db48d7-31a8-4d82-8053-92c3a8f7d62b)

**🖼️ Quora Recommendation Engine:** Compute servers with high RAM + CPU run ML models for answer ranking and personalization. Features extracted from HBase (view patterns, upvote history, topic preferences) feed into ranking algorithms. Batch processing computes recommendations offline, online serving layer (Redis-cached) delivers sub-50ms. Separates compute-heavy ML workloads from transactional databases.

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

![Quora Kafka-Based Async Task Processing](https://github.com/user-attachments/assets/0695a93b-ee0b-4399-b1c9-79613dacc3fa)

**🖼️ Quora Async Job Distribution:** Updated design offloads non-urgent tasks from synchronous API flow. Kafka disseminates jobs to specialized queues: view counter updates (sharded counters pattern), notification system (push notifications to followers), analytics pipelines (BigQuery aggregations), content highlighting (ML-based topic extraction). Each queue processed by dedicated worker pools via cron jobs. Reduces API response latency by 60% (sub-100ms), improves scalability by decoupling write-heavy operations.

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

![Quora Long Polling for Real-Time Updates](https://github.com/user-attachments/assets/23702cfb-22d2-4d48-a705-bca788384b80)

**🖼️ Real-Time Features via Long Polling:** For comments, upvotes, downvotes requiring frequent updates, Quora uses long polling instead of traditional polling. Client requests updates, server holds connection open for up to 60 seconds until new data arrives (immediate response if available). Reduces unnecessary requests by 95% vs polling every 5 seconds (from 12 requests/min to 1 request/min when idle). More efficient than WebSockets for infrequent updates, simpler infrastructure (HTTP-based, works through proxies).

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


# 9. Google Maps - Navigation & Location Services

> **📌 Quick Summary**: Real-time routing, traffic prediction, and location search at global scale  
> **Scale**: 1B+ users, 220+ countries, 25M+ updates/day | **Complexity**: ⭐⭐⭐⭐⭐

![Google Maps Segment-Based Routing Architecture](https://github.com/user-attachments/assets/8cd295ae-e8eb-4784-abe6-93632629208e)

**🖼️ Google Maps Segment Architecture:** Divides world into segments (e.g., 10km × 10km), each containing road network graph. Segment adder assigns unique IDs via ID generator, server allocator distributes segments across servers, key-value store maintains segment-to-server mappings. For routing queries, graph processing service identifies source/destination segments, connects to relevant servers, computes shortest path using A* or contraction hierarchies. Multi-segment routes computed by stitching paths across segment boundaries.

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

![Google Maps Segment Addition Workflow](https://github.com/user-attachments/assets/37d09a35-828b-43f8-b0f6-cd3eca4aaa6f)

**🖼️ Adding New Road Segments:** Segment adder receives new segment with lat/long boundary coordinates + road network graph → Unique ID generator assigns segment ID (distributed ID generation via Snowflake) → Server allocator assigns segment to available server (load balancing based on current segment count per server) → Segment graph hosted on assigned server → Mapping stored in key-value store (segment_id → server_id) + boundary coordinates stored separately (segment_id → lat/long bounds). Enables efficient routing query processing by quick segment lookup.

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

![Google Maps Real-Time Traffic Analytics Pipeline](https://github.com/user-attachments/assets/d043a28a-df21-4468-9d19-4e5ba7de6d37)

**🖼️ Traffic Data Processing:** Pub-sub system collects location streams (device, timestamp, lat/long) from all map servers (millions of active users) → Apache Spark reads data from pub-sub, applies ML models for traffic prediction, clustering (identify events/gatherings), anomaly detection (accidents, road closures), road discovery (new roads from GPS trails) → Analytics improve ETA accuracy by 30%, enable real-time rerouting. Data pipeline handles 10M+ location updates per second, processes in sub-minute latency.

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


# 10. Yelp - Location-Based Business Search

> **📌 Quick Summary**: Place discovery using dynamic QuadTree segments with proximity search  
> **Scale**: 200M+ reviews, 100M+ places, <100ms search latency | **Complexity**: ⭐⭐⭐⭐☆

![Yelp QuadTree Dynamic Segmentation](https://github.com/user-attachments/assets/7a121032-5b3b-4d47-8f78-7940cb912c9d)

**🖼️ Yelp QuadTree Spatial Indexing:** Solves uneven place distribution by dynamic segment splitting. When place count exceeds threshold (e.g., 500), segment divides into 4 quadrants recursively. Tree structure: internal nodes represent segments, leaf nodes store actual places. Doubly-linked list connects neighboring segments for efficient boundary queries (find restaurants within 5km crosses multiple leaf nodes). Search starts at root, traverses to target leaf, fetches neighboring segments if needed. Handles dense urban areas (small segments) and sparse rural areas (large segments) efficiently.

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


# 11. Uber - Ride-Hailing Platform

> **📌 Quick Summary**: Real-time driver-rider matching with optimized routing and payments  
> **Scale**: 150M+ users, 7M+ drivers, 25M+ trips/day | **Complexity**: ⭐⭐⭐⭐⭐

![Uber Hybrid QuadTree + Hash Table Architecture](https://github.com/user-attachments/assets/dd019f80-ee5f-4bf5-b3ce-f0a8dc9248a5)

**🖼️ Uber Driver-Rider Matching with Hybrid Index:** Combines QuadTree (spatial indexing) with hash table (fast updates). QuadTree updated every 15 seconds (batch process), hash table updated every 4 seconds (real-time driver locations). For matching: QuadTree finds drivers in rider's segment, hash table provides latest GPS coordinates, ranking algorithm considers distance + ETA + driver rating. Precomputed routing uses contraction hierarchies (partitioned graph with parallel processing across segments). Redis stores driver states (available/busy), Cassandra persists trip history.

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

![Apache Kafka Payment Processing Flow](https://github.com/user-attachments/assets/ef333239-b3a0-4777-9b4d-f934da8f6220)

**🖼️ Kafka Payment Pipeline:** Order creator \u2192 Kafka (order topic) \u2192 Order processor validates \u2192 Kafka (intent topic) \u2192 PSP transaction \u2192 Kafka (result topic) \u2192 Order writer persists. Asynchronous, fault-tolerant, replay-able for failed payments.

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

![Apache Kafka Payment Architecture (Detailed)](https://github.com/user-attachments/assets/ef333239-b3a0-4777-9b4d-f934da8f6220)

**🖼️ Detailed Kafka Payment Flow:** Trip completion triggers order creator \u2192 Money movement info published to Kafka (amount, rider ID, driver ID, payment method) \u2192 Order processor validates business rules (sufficient balance, valid payment method) \u2192 Intent sent to Kafka \u2192 PSP integration (Stripe/Braintree) processes charge \u2192 Result (success/failure) written to Kafka \u2192 Order writer updates database (transaction history, wallet balance). **Three-topic pattern:** orders \u2192 intents \u2192 results enables audit trail, retry logic, exactly-once semantics.

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


# 12. Twitter - Real-Time Microblogging Platform

> **📌 Quick Summary**: Distributed social network with real-time timeline, search, and analytics  
> **Scale**: 450M+ users, 500M+ tweets/day, 100ms search, 15s indexing latency | **Complexity**: ⭐⭐⭐⭐⭐

![Twitter Manhattan + Lucene Dual-Index Architecture](https://github.com/user-attachments/assets/af48a651-4620-43b9-b9b0-677139170393)

**🖼️ Twitter Storage Strategy:** Manhattan (distributed KV store) handles tweets, accounts, DMs with millions of QPS across multiple clusters (read-only vs heavy read-write separation). Uses RocksDB storage engine per node. Apache Lucene provides dual-index search: real-time index (recent tweets, past week) stored in RAM for <100ms latency, full index (historical tweets) uses batch processing with lower priority. Indexing latency ~15 seconds from tweet creation. Pelikan cache (Twemcache/Redis replacement) provides high-throughput low-latency caching layer. Multi-tier storage: Hot data (Manhattan) → Warm cache (Pelikan) → Cold analytics (BigQuery via Kafka).

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

![Twitter Complete System Architecture](https://github.com/user-attachments/assets/3b6dc27e-79d9-4c33-9645-2a922b35ba75)

**🖼️ Twitter End-to-End Data Flow:** Load balancer routes requests → **Tweet Service**: Handles /postTweet API, stores text/metadata in Manhattan/MySQL, attachments in Blobstore, streams events to Kafka → Cloud Pub/Sub → BigQuery (analytics) + Bigtable (serving). **Timeline Service**: Handles /viewHome_timeline, checks CDN first, fetches from Manhattan + sharded counters (engagement metrics), returns Top-K tweets. **Search Service**: /searchTweet queries Apache Lucene real-time index (RAM, recent tweets <15s latency) + full index (batch-processed historical), ranks by relevance/recency/engagement. ZooKeeper manages configuration, Zipkin traces request flows.

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


# 13. Newsfeed System - Social Media Feed

> **📌 Quick Summary**: Personalized content ranking and delivery for social networks  
> **Scale**: Billions of posts, millions of concurrent users, <200ms feed load | **Complexity**: ⭐⭐⭐⭐☆

![Newsfeed Hybrid Fan-Out Architecture](https://github.com/user-attachments/assets/ec84d0b0-7edd-4ffb-a4fb-a8b4e296f0ac)

**🖼️ Newsfeed Fan-Out Strategy:** Hybrid approach combining fan-out-on-write (celebrities) and fan-out-on-read (regular users). When user posts: if followers <1000, write to each follower's newsfeed cache (fan-out-on-write); if followers >100K (celebrity), store in user's own outbox (fan-out-on-read). Timeline generation: fetch pre-computed feed from cache (fan-out-on-write entries) + fetch celebrity posts on-demand (fan-out-on-read) + merge and rank. Trade-off: Fan-out-on-write = fast reads but slow writes (1M followers = 1M writes), Fan-out-on-read = fast writes but slower reads (must query multiple celebrity outboxes). Ranking service applies ML model to merged feed for personalization.

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


# 14. Instagram - Photo/Video Sharing

> **📌 Quick Summary**: Media-heavy social platform with feed, stories, and explore  
> **Scale**: 2B+ users, 95M+ photos/day, 100M+ stories/day | **Complexity**: ⭐⭐⭐⭐☆

![Instagram Push-Pull Hybrid CDN Architecture](https://github.com/user-attachments/assets/c295add8-8097-4665-b3cf-b73c53e1e156)

**🖼️ Instagram Feed Generation Strategy:** Hybrid push-pull model based on follower count. **Push-based users** (<1000 followers): Fan-out-on-write, pre-compute followers' feeds, fast read times. **Pull-based users** (celebrities >100K followers): Fan-out-on-read, followers fetch on-demand, avoids write amplification. CDN caching: Popular celebrity content cached at edge (3-tier: Edge CDN → Regional cache → Origin blob storage), regular users served from application servers with Redis cache. Load balancer routes: Read requests → Nearest CDN first, write requests → Application servers → Database shards. Handles millions of concurrent users with low latency (<50ms p99).

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

![Instagram Feed Generation Detailed Flow](https://github.com/user-attachments/assets/324bb9d4-9f9e-46d0-92ff-40701bbffc3e)

**🖼️ Instagram Push vs Pull Strategy Visualization:** Diagram illustrates decision tree: followers <1000 → fan-out-on-write (pre-compute feeds, write to follower caches), followers >100K → fan-out-on-read (celebrity posts fetched on-demand). Shows data flow: Post creation → Database write → Fan-out service checks follower count → Conditional routing (push workers write N feeds vs pull index stores celebrity post reference) → Timeline generation merges both sources → Ranking algorithm (ML model) applies engagement prediction → Serve to user. CDN integration shown for media (images/videos) separate from feed text.

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


# 15. WhatsApp - Messaging Platform

> **📌 Quick Summary**: End-to-end encrypted messaging with groups, media, and real-time delivery  
> **Scale**: 2B+ users, 100B+ messages/day, <200ms delivery | **Complexity**: ⭐⭐⭐⭐⭐

![WhatsApp Group Messaging via Kafka Topics](https://github.com/user-attachments/assets/13e8b5c6-48fc-4dd6-a69e-d907d38bbf28)

**🖼️ WhatsApp Group Message Flow:** User A sends message to Group/A → WebSocket server forwards to message service → Kafka topic created per group (group = Kafka topic, senders = producers, receivers = consumers) → Group service fetches group members from MySQL cluster (geographically distributed replicas + Redis cache for low latency) → Group message handler retrieves member list → Delivers message to each member's WebSocket connection. **Key features:** End-to-end encryption (Signal Protocol), FIFO ordering (Kafka guarantees), at-least-once delivery (retry on failure), offline message queue (messages persist in Kafka until user comes online). **Scalability:** Kafka partitions by group_id, horizontal scaling of message handlers, Redis cache reduces MySQL load.

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


# 16. Typeahead Suggestion System - Autocomplete

> **📌 Quick Summary**: Real-time query suggestions as user types (Google Search, YouTube, Twitter)  
> **Scale**: Billions of queries/day, <100ms suggestion latency | **Complexity**: ⭐⭐⭐⭐☆

![Typeahead Trie with Offline Updates](https://github.com/user-attachments/assets/87be1f0b-97f7-47d2-8cff-8fce02a0e11c)

**🖼️ Typeahead Suggestion System Architecture:** Collection service gathers raw search queries → Aggregator (MapReduce) computes prefix frequencies from HDFS logs → Cassandra stores aggregated data (prefix → frequency mappings) → Trie builder creates/updates trie structures offline (not real-time to avoid user-facing latency) → Tries stored in MongoDB (NoSQL document store, easy serialization), distributed via ZooKeeper to shards → Redis caches hot prefixes for <1ms lookup. **Trie optimization:** Reduced depth (group rare terms), top-k stored at each node (precomputed suggestions), geographically partitioned tries (US vs EU different trending queries). **Latency:** <10ms end-to-end (Redis cache hit), <50ms (MongoDB trie lookup).

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


# 17. Google Docs - Collaborative Document Editing

> **📌 Quick Summary**: Real-time collaborative editing with conflict resolution and versioning  
> **Scale**: 2B+ documents, millions of concurrent editors, <50ms sync | **Complexity**: ⭐⭐⭐⭐⭐

![Google Docs Operational Transformation (OT)](https://github.com/user-attachments/assets/ac643c4f-36f7-4b4f-a4c3--5d8b76d825ca)

**🖼️ Google Docs Real-Time Collaboration:** WebSocket connections enable bidirectional communication between clients and servers. **Conflict resolution:** Operational Transformation (OT) and CRDTs (Conflict-free Replicated Data Types) transform concurrent edits to maintain consistency (e.g., User A inserts at position 5 while User B deletes at position 3 → OT adjusts A's position to 4). **Time-series database** preserves operation order across distributed servers. **Replication:** Gossip protocol replicates operations within same datacenter (<5ms), async replication across datacenters (50-200ms). **Storage:** CRDTs stored in Redis (in-memory, fast conflict merging), periodic snapshots to RDBMS (PostgreSQL/MySQL) and blob storage (document versions). **Availability:** Multiple WebSocket servers per region, component isolation prevents cascading failures, CDN serves static assets (images/videos embedded in docs).

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


---

# 📚 Phase 5 Enhancements: Pattern Index, Decision Trees & Comparison Matrix

## 🎯 Pattern Index - Cross-Reference of Design Patterns

### Architectural Patterns

| Pattern | Systems Using It | Use Case | Implementation Details |
|---------|------------------|----------|------------------------|
| **Layered Architecture** | YouTube, Instagram, WhatsApp, Google Docs | Separation of concerns | Presentation → Business Logic → Data Access → Database |
| **Microservices** | Uber, Twitter, Quora | Independent scaling, deployment | Service mesh, API gateway, service discovery (Consul/Eureka) |
| **Event-Driven** | Instagram (Stories), Twitter (Timeline), Newsfeed | Asynchronous processing | Kafka, RabbitMQ, AWS SNS/SQS |
| **CQRS (Command Query Responsibility Segregation)** | Twitter (read/write separation), Google Docs | Optimize read vs write paths | Separate models: write (Manhattan) vs read (Lucene/Memcached) |
| **Saga Pattern** | Uber (payments), E-commerce | Distributed transactions | Choreography (events) or Orchestration (central coordinator) |
| **Circuit Breaker** | All distributed systems | Fault tolerance | Hystrix, Resilience4j, implement timeouts + fallbacks |
| **Bulkhead** | Rate Limiter, API Gateway | Resource isolation | Thread pools, connection pools per client tier |
| **Strangler Fig** | Twitter (Manhattan replaced Cassandra) | Legacy system migration | Incrementally route traffic from old to new system |

### Data Patterns

| Pattern | Systems Using It | Use Case | Key Characteristics |
|---------|------------------|----------|---------------------|
| **Sharding** | YouTube, Twitter, Instagram, Uber | Horizontal database scaling | Partition by user_id, consistent hashing, range-based |
| **Replication** | All systems | High availability, read scaling | Master-slave (async), master-master (sync), 3x typical |
| **Caching (Multi-tier)** | YouTube (CDN/RAM/SSD), Instagram (L1/L2/L3) | Latency reduction | CDN (90% hit) → App cache (8%) → Database (2%) |
| **Read-Through Cache** | Quora, Twitter | Simplify cache logic | Cache sits between app and database, auto-populates |
| **Write-Through Cache** | Instagram (Stories), WhatsApp | Consistency | Write to cache and database synchronously |
| **Write-Behind Cache** | Google Analytics, Logging | High write throughput | Buffer writes in cache, batch flush to database |
| **Eventual Consistency** | Newsfeed, Instagram likes | Availability over consistency | Accept stale reads for 100-1000ms |
| **Strong Consistency** | Google Docs (OT), Banking | Correctness critical | 2PC, Paxos, Raft consensus algorithms |
| **Denormalization** | NoSQL (MongoDB, Cassandra) | Read performance | Embed related data in single document, accept duplication |
| **Materialized Views** | Analytics dashboards, Leaderboards | Pre-computed aggregates | Redis sorted sets, pre-join tables |

### Communication Patterns

| Pattern | Systems Using It | Use Case | Protocols/Technologies |
|---------|------------------|----------|------------------------|
| **Request-Response (Synchronous)** | Most APIs | Simple queries | HTTP/REST, gRPC |
| **Pub-Sub** | Twitter (notifications), Instagram (feed updates) | 1-to-many messaging | Redis Pub/Sub, Kafka topics, AWS SNS |
| **Message Queue** | Uber (trip processing), YouTube (video encoding) | Async task processing | RabbitMQ, AWS SQS, Google Pub/Sub |
| **Long Polling** | Quora (notifications), Facebook (older implementation) | Near real-time updates | HTTP keep-alive, 30-60s timeout |
| **WebSockets** | WhatsApp, Google Docs, Typeahead | Bi-directional real-time | Persistent TCP connection, <50ms latency |
| **Server-Sent Events (SSE)** | Stock tickers, Live sports scores | Server-to-client streaming | HTTP-based, uni-directional |
| **gRPC Streaming** | YouTube (video upload), Google Maps (GPS tracking) | Efficient binary streaming | HTTP/2, Protobuf, bi-directional |

### Scalability Patterns

| Pattern | Systems Using It | Use Case | Scale Achieved |
|---------|------------------|----------|----------------|
| **Horizontal Scaling (Stateless Services)** | All systems | Add more servers | Linear scalability up to 1000s of nodes |
| **Auto-Scaling** | Cloud systems (AWS ECS, K8s) | Dynamic capacity | Scale on CPU/memory/queue depth metrics |
| **Database Federation** | Large enterprises | Partition by function | Users DB, Products DB, Orders DB separate |
| **Read Replicas** | YouTube, Twitter | Read scaling | 10-100x read capacity vs single master |
| **Load Balancing** | All systems | Traffic distribution | Layer 4 (TCP) + Layer 7 (HTTP), consistent hashing |
| **CDN (Content Delivery Network)** | YouTube, Instagram | Global content distribution | 50-200 PoPs, 90%+ cache hit ratio |
| **Geographically Distributed** | Google Maps, Uber | Low latency worldwide | Data centers in US-East, US-West, EU, Asia regions |
| **Fan-Out on Write** | Twitter (celebrities <10K followers), Newsfeed | Pre-compute timelines | Write to N follower caches proactively |
| **Fan-Out on Read** | Twitter (mega-influencers >1M followers) | Avoid write amplification | Compute timeline on-demand per read request |

---

## 🌳 Decision Trees for Technology Selection

### 1. Database Selection Decision Tree

```
START: Choosing a Database
│
├─ Need ACID transactions + complex joins?
│  ├─ YES → Need distributed + high availability?
│  │        ├─ YES → **CockroachDB** / **YugabyteDB** / **Google Spanner**
│  │        │         (Distributed SQL, strong consistency, 99.99% uptime)
│  │        └─ NO → **PostgreSQL** / **MySQL**
│  │                  (Traditional RDBMS, <100K QPS per node)
│  │
│  └─ NO → What's your primary access pattern?
│           │
│           ├─ Key-value lookups (user profiles, sessions)
│           │  → **Redis** (in-memory, <1ms latency)
│           │  → **DynamoDB** / **Cassandra** (persistent, 10ms P99)
│           │
│           ├─ Document storage (flexible schema, JSON)
│           │  → **MongoDB** (rich queries, <10ms reads)
│           │  → **Couchbase** (built-in cache, mobile sync)
│           │
│           ├─ Wide-column (time-series, analytics)
│           │  → **Cassandra** (write-heavy, petabyte-scale)
│           │  → **HBase** / **BigTable** (strong consistency)
│           │
│           ├─ Graph relationships (social, recommendations)
│           │  → **Neo4j** (ACID, Cypher query language)
│           │  → **DGraph** (GraphQL-native, distributed)
│           │
│           └─ Full-text search
│              → **Elasticsearch** / **OpenSearch** (inverted index, faceted search)
│              → **Algolia** (managed, typo-tolerance, <10ms)
```

### 2. Cache Strategy Selection

```
START: Choosing a Caching Strategy
│
├─ Cache location?
│  ├─ Client-side (browser, mobile app)
│  │  → **Local Storage** / **IndexedDB** / **SQLite**
│  │     Use: Static assets, user preferences
│  │     TTL: Hours to days
│  │
│  ├─ CDN (edge, global)
│  │  → **Cloudflare** / **Fastly** / **Akamai**
│  │     Use: Images, videos, static HTML/CSS/JS
│  │     TTL: 1-30 days, manual purge on updates
│  │
│  ├─ Application-level (in-process)
│  │  → **Caffeine** (Java) / **GoCache** (Go) / **LRU Cache** (Python)
│  │     Use: Hot data, <1MB, single-server
│  │     TTL: Seconds to minutes
│  │
│  └─ Distributed (shared across servers)
│     ├─ Need persistence + advanced data structures?
│     │  ├─ YES → **Redis** (sorted sets, pub/sub, Lua scripts)
│     │  └─ NO → **Memcached** (pure cache, 1M+ ops/sec)
│     │
│     └─ Need multi-region replication?
│        ├─ YES → **Redis Enterprise** / **Amazon ElastiCache Global Datastore**
│        └─ NO → **Redis** / **Memcached** (single region)
│
├─ Cache population strategy?
│  ├─ **Cache-Aside** (Lazy Loading)
│  │  → App checks cache first, fetches from DB on miss, then stores in cache
│  │  → Use: Read-heavy, tolerate stale data (e.g., user profiles, product catalogs)
│  │  → **Example**: Twitter user profile lookup
│  │
│  ├─ **Read-Through**
│  │  → Cache automatically loads from DB on miss (cache library handles it)
│  │  → Use: Simplify code, consistent pattern
│  │  → **Example**: Hibernate second-level cache
│  │
│  ├─ **Write-Through**
│  │  → Write to cache and DB synchronously
│  │  → Use: Strong consistency needed (e.g., inventory, financial balances)
│  │  → **Trade-off**: Higher write latency (+5-10ms)
│  │
│  ├─ **Write-Behind** (Write-Back)
│  │  → Write to cache immediately, async flush to DB
│  │  → Use: High write throughput (e.g., analytics, click tracking)
│  │  → **Risk**: Data loss if cache crashes before flush
│  │
│  └─ **Refresh-Ahead**
│     → Proactively refresh cache before expiry
│     → Use: Predictable access patterns (e.g., homepage data, trending topics)
│     → **Example**: Pre-warm cache during off-peak hours
│
└─ Eviction policy?
   ├─ **LRU** (Least Recently Used) → Default, works for 80% of cases
   ├─ **LFU** (Least Frequently Used) → Long-term popularity (e.g., Wikipedia articles)
   ├─ **FIFO** → Simple, predictable, good for queues
   └─ **TTL-based** → Time-sensitive data (e.g., JWT tokens, OTP codes)
```

### 3. Rate Limiting Algorithm Selection

```
START: Choosing a Rate Limiting Algorithm
│
├─ Burst tolerance required?
│  │
│  ├─ YES (allow temporary spikes)
│  │  ├─ Need precise rate control over time?
│  │  │  ├─ YES → **Token Bucket** ⭐ [RECOMMENDED]
│  │  │  │        • Capacity: Max burst size
│  │  │  │        • Refill rate: Sustained rate
│  │  │  │        • **Use**: AWS API Gateway, Stripe, most APIs
│  │  │  │        • **Example**: 100 req/sec sustained, burst up to 1000
│  │  │  │
│  │  │  └─ NO (approximate okay) → **Fixed Window Counter**
│  │  │           • Simple: Count requests per time window (e.g., per minute)
│  │  │           • **Trade-off**: Boundary burst (200 req at 12:00:59 + 200 at 12:01:00 = 400 in 2 sec)
│  │  │           • **Use**: Simple cases, low traffic
│  │  │
│  └─ NO (strict rate enforcement)
│     → **Leaky Bucket**
│        • Processes requests at fixed rate, queues excess
│        • **Use**: Message brokers, batch processing (Kafka, RabbitMQ)
│        • **Trade-off**: Higher memory (O(N) queue), added latency
│
├─ Need accuracy + memory efficiency?
│  └─ YES → **Sliding Window Counter** (Hybrid) ⭐ [RECOMMENDED]
│            • Combines fixed window (memory O(1)) + sliding accuracy (99%)
│            • **Use**: High-traffic APIs, distributed systems
│            • **Example**: Cloudflare, Kong API Gateway
│
├─ Need perfect accuracy?
│  └─ YES → **Sliding Window Log**
│            • Store timestamp of each request
│            • **Trade-off**: High memory (O(N)), expensive (scan + delete old entries)
│            • **Use**: Financial systems, compliance (audit trail)
│
└─ Distributed rate limiting (multiple servers)?
   ├─ **Centralized Counter** (Redis)
   │  • Single source of truth
   │  • **Trade-off**: Redis is SPOF, network latency +1-5ms
   │  • **Use**: Most production systems (Netflix, Twitter)
   │
   ├─ **Local + Sync** (Gossip Protocol)
   │  • Each node tracks locally, periodically syncs
   │  • **Trade-off**: Eventual consistency, may exceed limit temporarily
   │  • **Use**: Relaxed requirements, very high throughput
   │
   └─ **Rate Limit Headers** (Response to client)
      • X-RateLimit-Limit: 1000
      • X-RateLimit-Remaining: 234
      • X-RateLimit-Reset: 1699999999 (Unix timestamp)
      • **Use**: All public APIs for client-side backoff
```

### 4. Fan-Out Strategy Selection (Social Networks)

```
START: Newsfeed Fan-Out Strategy
│
├─ User's follower count?
│  │
│  ├─ < 1,000 followers → **Fan-Out on Write (Push Model)** ⭐
│  │  • Write post → Immediately insert into all followers' newsfeeds (cache/DB)
│  │  • **Read time**: O(1) - just fetch pre-computed feed
│  │  • **Write time**: O(N) where N = follower count
│  │  • **Storage**: O(M × N) where M = posts, N = followers
│  │  • **Example**: Regular Instagram user posts a photo
│  │  • **Pros**: Instant feed updates, fast reads (<10ms)
│  │  • **Cons**: Write amplification for popular users
│  │
│  ├─ 1,000 - 100,000 followers → **Hybrid Model** ⭐ [MOST COMMON]
│  │  • **Push to active followers** (<10K active in last 30 days)
│  │  • **Pull for inactive followers** (compute on-demand if they login)
│  │  • **Example**: Medium-sized influencer (YouTuber with 50K subs)
│  │  • **Optimization**: Pre-compute for VIP followers (verified, frequently engage)
│  │  • **Trade-off**: Complexity in tracking active vs inactive
│  │
│  └─ > 100,000 followers (Celebrities, Mega-Influencers) → **Fan-Out on Read (Pull Model)**
│     • Write post → Store once in user's timeline table
│     • Read feed → Fetch posts from followees on-demand, merge, rank
│     • **Read time**: O(K × log K) where K = followees count
│     • **Write time**: O(1) - just insert one record
│     • **Example**: Taylor Swift tweets → 100M followers, don't fan-out
│     • **Pros**: No write amplification, O(1) storage
│     • **Cons**: Slower reads (100-500ms), requires caching of celebrity posts
│
├─ Hybrid Implementation Details (Twitter's approach)
│  │
│  ├─ **In-Memory Fan-Out**
│  │  • Push to Redis sorted sets (feed cache)
│  │  • Key: user:{follower_id}:feed
│  │  • Value: [{post_id, timestamp, score}] (sorted by time)
│  │  • Limit: Keep last 800 posts per user (~1KB total)
│  │
│  ├─ **Pull from Celebrity Cache**
│  │  • Celebrities' recent posts (last 24 hours) in dedicated Redis cache
│  │  • Key: celeb_posts:global (sorted set)
│  │  • All users fetch from this shared cache on read
│  │  • **Hit ratio**: 95%+ (most users follow ≥1 celebrity)
│  │
│  └─ **Merge Algorithm (Feed Rendering)**
│     1. Fetch pre-computed feed from Redis (pushed posts)
│     2. Fetch celebrity posts from shared cache (pulled posts)
│     3. Merge two lists by timestamp
│     4. Apply ML ranking (engagement prediction, recency, relevance)
│     5. Return top 20-50 posts
│     6. **Total latency**: 50-150ms
│
└─ When to Recompute?
   ├─ **Incremental Updates** → New post triggers small update
   ├─ **Full Recomputation** → User follows/unfollows someone, refresh entire feed
   ├─ **Scheduled Refresh** → Daily at 3 AM for inactive users (catch up on missed posts)
   └─ **On-Demand** → User explicitly pulls to refresh (swipe down)
```

---

## 📊 Technology Comparison Matrix

### 1. Relational vs NoSQL Databases

| Dimension | PostgreSQL | MySQL | CockroachDB | MongoDB | Cassandra | Redis |
|-----------|------------|-------|-------------|---------|-----------|-------|
| **Type** | RDBMS | RDBMS | Distributed SQL | Document | Wide-Column | Key-Value |
| **ACID** | ✅ Full | ✅ Full | ✅ Full | ⚠️ Single-doc | ❌ Eventual | ⚠️ Single-key |
| **Consistency** | Strong | Strong | Strong | Eventual | Tunable | Strong |
| **Scalability** | Vertical | Vertical | Horizontal | Horizontal | Horizontal | Vertical |
| **Max Throughput** | 50K QPS | 100K QPS | 50K QPS | 100K QPS | 1M writes/s | 1M ops/s |
| **Latency (P99)** | 5-10ms | 5-10ms | 10-50ms | 5-10ms | 5-10ms | <1ms |
| **Joins** | ✅ Excellent | ✅ Good | ✅ Excellent | ❌ Limited | ❌ No | ❌ No |
| **Schema** | Fixed | Fixed | Fixed | Flexible | Flexible | Schema-less |
| **Replication** | Async/Sync | Async/Sync | Multi-region | Replica sets | Multi-DC | Master-Replica |
| **Best For** | Complex queries | Web apps | Global apps | Rapid dev | Time-series | Caching |
| **Worst For** | High writes | Sharding | Low latency | Analytics | Joins | Large values |
| **Cost** | Open-source | Open-source | $$$ (license) | Open-source | Open-source | Open-source |
| **Max Data Size** | 32TB/table | 64TB/table | Petabytes | Petabytes | Exabytes | 512MB/key |
| **Who Uses It** | Instagram, Uber | Facebook, Twitter | CockroachLabs | eBay, EA | Netflix, Apple | Twitter, GitHub |

### 2. Caching Solutions Comparison

| Feature | Memcached | Redis | Caffeine (Java) | Varnish | CDN (Cloudflare) |
|---------|-----------|-------|-----------------|---------|------------------|
| **Primary Use** | Distributed cache | Distributed cache + DB | In-process cache | HTTP cache | Global edge cache |
| **Data Structures** | String only | String, List, Set, Sorted Set, Hash, HyperLogLog, Bitmap | ConcurrentHashMap | Objects | Static files |
| **Persistence** | ❌ No | ✅ RDB + AOF | ❌ No | ✅ Disk | ✅ SSD |
| **Replication** | ❌ No | ✅ Master-Replica | ❌ No | ✅ Clustered | ✅ Global PoPs |
| **Max Throughput** | 1M+ ops/s | 500K ops/s | 10M+ ops/s | 50K req/s | 100M+ req/s |
| **Latency** | <1ms | <1ms | <0.01ms | 1-5ms | 10-50ms |
| **Memory Limit** | 64GB/node | 512GB/node | Heap size | Unlimited | Unlimited |
| **Eviction** | LRU | LRU, LFU, TTL | LRU, TinyLFU | TTL | TTL |
| **Clustering** | ❌ Client-side | ✅ Redis Cluster | ❌ No | ✅ Yes | ✅ Global |
| **Pub/Sub** | ❌ No | ✅ Yes | ❌ No | ❌ No | ❌ No |
| **Scripting** | ❌ No | ✅ Lua | ❌ No | ✅ VCL | ✅ Workers (JS) |
| **Best For** | Simple KV, high throughput | Complex data, persistence | Local cache, low latency | HTTP proxy | Static assets, global |
| **Cost** | Open-source | Open-source | Open-source | Open-source | $$$ (usage-based) |
| **Who Uses It** | Facebook, Pinterest | Twitter, GitHub, StackOverflow | Google, LinkedIn | Vimeo, BBC | 20% of web |

### 3. Message Queues & Streaming Platforms

| Feature | RabbitMQ | Apache Kafka | AWS SQS | Redis Streams | Google Pub/Sub |
|---------|----------|--------------|---------|---------------|----------------|
| **Type** | Message Broker | Distributed Log | Queue Service | Stream | Managed Pub/Sub |
| **Ordering** | ✅ Per queue | ✅ Per partition | ❌ Best-effort | ✅ Per stream | ⚠️ Per key |
| **Throughput** | 50K msg/s | 1M+ msg/s | 300K msg/s | 100K msg/s | 100K msg/s |
| **Latency** | 1-5ms | 5-10ms | 10-100ms | <1ms | 10-100ms |
| **Retention** | Until consumed | Days to years | 14 days max | Limited by memory | 7 days default |
| **Persistence** | ✅ Disk | ✅ Disk | ✅ S3 | ⚠️ Optional | ✅ Cloud Storage |
| **Replayability** | ❌ No | ✅ Yes (offset) | ⚠️ Limited | ✅ Yes | ✅ Yes (snapshots) |
| **Max Message Size** | 128MB | 1MB default | 256KB | 512MB | 10MB |
| **Delivery Semantics** | At-least-once | Exactly-once | At-least-once | At-least-once | At-least-once |
| **Ordering Guarantee** | FIFO queues | Per partition | FIFO queues only | Yes | With ordering key |
| **Clustering** | ✅ Yes | ✅ Yes | ✅ Managed | ✅ Cluster mode | ✅ Managed |
| **Protocol** | AMQP | Custom (binary) | HTTP/SQS API | RESP | HTTP/gRPC |
| **Best For** | Task queues, RPC | Event sourcing, logs | Decoupling services | Real-time analytics | Serverless, multi-cloud |
| **Cost** | Self-hosted | Self-hosted | $0.40/1M requests | Open-source | $0.60/1M messages |
| **Who Uses It** | Robinhood, Reddit | Uber, Netflix, LinkedIn | Airbnb, Capital One | Twitter (internal) | Spotify, PayPal |

### 4. API Protocols Comparison

| Feature | REST (HTTP/JSON) | gRPC (HTTP/2/Protobuf) | GraphQL | WebSocket | SOAP |
|---------|------------------|------------------------|---------|-----------|------|
| **Transport** | HTTP/1.1, HTTP/2 | HTTP/2 | HTTP | TCP | HTTP |
| **Data Format** | JSON, XML | Protobuf (binary) | JSON | Binary/Text | XML |
| **Latency** | 10-50ms | 1-5ms | 10-50ms | <10ms | 50-200ms |
| **Throughput** | 10K req/s | 100K req/s | 10K req/s | 50K msg/s | 1K req/s |
| **Type Safety** | ❌ No (runtime) | ✅ Yes (Protobuf schema) | ⚠️ Schema validation | ❌ No | ✅ WSDL |
| **Browser Support** | ✅ Native | ❌ Requires proxy | ✅ Native | ✅ Native | ✅ With libraries |
| **Streaming** | ❌ No (SSE possible) | ✅ Bi-directional | ⚠️ Subscriptions | ✅ Bi-directional | ❌ No |
| **Caching** | ✅ HTTP caching | ❌ Limited | ⚠️ Complex | ❌ No | ⚠️ Limited |
| **Code Generation** | ⚠️ Optional (OpenAPI) | ✅ Built-in | ✅ Built-in | ❌ Manual | ✅ From WSDL |
| **Learning Curve** | Low | Medium | Medium | Low | High |
| **Best For** | Public APIs, CRUD | Internal services, microservices | Flexible queries, mobile | Real-time, chat | Enterprise, legacy |
| **Worst For** | High performance | Public-facing | Simple CRUD | HTTP compatibility | Modern web |
| **Who Uses It** | Stripe, Twilio, most public APIs | Google, Netflix, Uber | GitHub, Shopify, Facebook | WhatsApp, Discord | Banks, SAP |

### 5. Search & Analytics Engines

| Feature | Elasticsearch | Apache Solr | Algolia | MeiliSearch | Typesense |
|---------|---------------|-------------|---------|-------------|-----------|
| **Based On** | Apache Lucene | Apache Lucene | Custom (C++) | Rust | C++ |
| **Deployment** | Self-hosted | Self-hosted | Managed SaaS | Self-hosted | Self-hosted |
| **Indexing Speed** | 10K docs/s | 10K docs/s | 100K docs/s | 50K docs/s | 50K docs/s |
| **Query Latency** | 10-50ms | 10-50ms | <10ms | <10ms | <10ms |
| **Typo Tolerance** | ⚠️ Fuzzy queries | ⚠️ Fuzzy queries | ✅ Built-in | ✅ Built-in | ✅ Built-in |
| **Faceted Search** | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| **Vector Search** | ✅ Yes (HNSW) | ⚠️ Plugin | ❌ No | ✅ Yes | ✅ Yes |
| **Real-time** | ✅ 1s refresh | ✅ 1s refresh | ✅ Instant | ✅ Instant | ✅ Instant |
| **Scalability** | Petabytes | Petabytes | Managed | 10s of GB | 100s of GB |
| **Language Support** | 30+ | 30+ | 40+ | 20+ | 20+ |
| **Best For** | Logs, analytics | Enterprise search | E-commerce, apps | Small-medium datasets | Fast autocomplete |
| **Cost** | Open-source | Open-source | $$$ (query-based) | Open-source | Open-source |
| **Who Uses It** | Uber, LinkedIn, Netflix | Apple, Netflix, Instagram | Stripe, Twitch, Lacoste | Internal tools | Airbnb, Wikipedia |

---

## 📝 Quick Decision Framework Cheatsheet

### When to Use Which Database?

1. **Need complex joins + transactions** → PostgreSQL / MySQL
2. **High write throughput (logs, events)** → Cassandra / ClickHouse
3. **Fast key-value lookups** → Redis / DynamoDB
4. **Flexible schema (rapid iteration)** → MongoDB / Firestore
5. **Global distribution + strong consistency** → CockroachDB / Spanner
6. **Full-text search** → Elasticsearch / MeiliSearch
7. **Graph relationships** → Neo4j / ArangoDB
8. **Time-series data** → InfluxDB / TimescaleDB

### When to Use Which Cache?

1. **Simple string caching, highest throughput** → Memcached
2. **Complex data structures, persistence** → Redis
3. **In-process, lowest latency** → Caffeine / Local cache
4. **HTTP responses, reverse proxy** → Varnish / NGINX
5. **Global static assets** → CDN (Cloudflare / Fastly)
6. **Database query cache** → Redis / Memcached
7. **Session storage** → Redis (persistence + expiry)

### When to Use Which Message Queue?

1. **Event sourcing, replay, high throughput** → Kafka
2. **Task queue, RPC, dead-letter queues** → RabbitMQ
3. **Serverless, AWS-native** → SQS + SNS
4. **Real-time analytics** → Redis Streams / Pulsar
5. **Simple pub-sub** → Redis Pub/Sub / Google Pub/Sub
6. **Guaranteed ordering** → Kafka (per partition) / SQS FIFO

### When to Use Which API Protocol?

1. **Public-facing REST API** → REST (JSON)
2. **Internal microservices** → gRPC (Protobuf)
3. **Mobile apps (minimize bandwidth)** → GraphQL
4. **Real-time bidirectional** → WebSocket
5. **Enterprise integration** → SOAP (legacy) / REST (modern)

---

## 🎓 Common Anti-Patterns to Avoid

1. **Database as Message Queue** → Use actual message queue (Kafka, RabbitMQ)
2. **SELECT * in loops** (N+1 query problem) → Use joins or batch queries
3. **Not using connection pooling** → Causes connection exhaustion
4. **Caching without TTL** → Memory leaks, stale data
5. **Ignoring database indexes** → Slow queries (seconds vs milliseconds)
6. **Synchronous processing of async tasks** → Use message queues
7. **Single point of failure** → Replicate critical components
8. **No monitoring/alerting** → Blind to production issues
9. **Premature optimization** → "Make it work, make it right, make it fast"
10. **Not considering consistency model** → Leads to data corruption bugs

---

**End of Phase 5 Enhancements** 🎉

---

# 🎯 SDE-II Interview Essentials

## 📐 Back-of-the-Envelope Calculations

> **Why this matters**: Interviewers expect you to estimate capacity, storage, and bandwidth in EVERY system design interview.

### Power of 2 Table (Memorize This!)

| Power | Exact Value | Approximate | Short Name |
|-------|-------------|-------------|------------|
| 2^10 | 1,024 | ~1 thousand | 1 KB |
| 2^20 | 1,048,576 | ~1 million | 1 MB |
| 2^30 | 1,073,741,824 | ~1 billion | 1 GB |
| 2^40 | 1,099,511,627,776 | ~1 trillion | 1 TB |
| 2^50 | ~1.1 × 10^15 | ~1 quadrillion | 1 PB |

### Latency Numbers Every SDE Should Know (2025 Updated)

| Operation | Latency | Relative Time |
|-----------|---------|---------------|
| **L1 cache reference** | 0.5 ns | 1 sec |
| **L2 cache reference** | 7 ns | 14 sec |
| **RAM reference** | 100 ns | 3 min |
| **SSD random read** | 150 μs (150,000 ns) | 1.7 days |
| **HDD seek** | 10 ms (10,000,000 ns) | 4 months |
| **Network within datacenter** | 500 μs | 5.8 days |
| **Network cross-country (US)** | 50 ms | 1.6 years |
| **Network trans-Atlantic** | 150 ms | 4.8 years |

**Key Takeaways:**
- Memory is ~100,000x faster than disk
- SSD is ~100x faster than HDD
- Within-datacenter network is 100x faster than cross-country
- **Design implication**: Cache in RAM, avoid network calls when possible

### Typical System Capacity Numbers

| Metric | Small System | Medium System | Large System |
|--------|--------------|---------------|--------------|
| **Users** | 10K-100K | 1M-10M | 100M+ |
| **QPS (read)** | 100-1K | 10K-100K | 1M+ |
| **QPS (write)** | 10-100 | 1K-10K | 100K+ |
| **Storage** | 10GB-100GB | 1TB-10TB | 100TB-PB |
| **Bandwidth** | 1 Mbps | 100 Mbps | 10+ Gbps |
| **Servers** | 1-10 | 10-100 | 1000+ |

### Estimation Framework (Use This Every Time!)

```
📝 Example: Design Twitter

1. Assumptions (Always state these first!):
   - 300M DAU (Daily Active Users)
   - Each user tweets 2 times/day → 600M tweets/day
   - Read:Write ratio = 100:1 (viewing >> posting)
   - Average tweet size = 280 chars = 280 bytes (text only)
   - 10% tweets have images (200KB each), 1% have videos (2MB each)

2. Traffic Estimates:
   - Write QPS = 600M / 86400 seconds ≈ 7K tweets/sec
   - Peak Write QPS = 7K × 3 (assume 3x peak) = 21K/sec
   - Read QPS = 7K × 100 = 700K reads/sec
   - Peak Read QPS = 700K × 2 = 1.4M reads/sec

3. Storage Estimates (5 years):
   - Text: 600M tweets/day × 280 bytes × 365 × 5 = ~306TB
   - Images: 60M/day × 200KB × 365 × 5 = ~22PB
   - Videos: 6M/day × 2MB × 365 × 5 = ~22PB
   - Total: ~44PB (mostly media!)

4. Bandwidth Estimates:
   - Write bandwidth = 7K tweets/sec × 280 bytes = 2 MB/s (text)
   - With media: ~500 MB/s incoming
   - Read bandwidth = 700K reads/sec × 280 bytes = 196 MB/s (text)
   - With media: ~50 GB/s outgoing (this is why CDN is critical!)

5. Memory Estimates (for cache):
   - 80/20 rule: 20% of tweets generate 80% of traffic
   - Cache 20% of daily tweets = 120M tweets
   - Size = 120M × 280 bytes = 33.6 GB (text only)
   - With thumbnails: ~2TB (fits in RAM across servers!)
```

### Quick Calculation Tricks

**Tip 1: Simplify** 
- 365 days/year ≈ 400 (easier mental math)
- 86400 seconds/day ≈ 100K (simplifies division)

**Tip 2: Powers of 10**
- 1K = 10^3, 1M = 10^6, 1B = 10^9

**Tip 3: Percentage to Fraction**
- 20% = 1/5, 33% = 1/3, 25% = 1/4

### Common Estimation Mistakes

❌ **Don't:**
- Spend >5 minutes on calculations (rough estimates OK!)
- Get exact numbers (approximations are fine)
- Forget to state assumptions
- Ignore peak vs average (peak can be 2-10x)

✅ **Do:**
- Round aggressively (7,234 → 7K)
- State assumptions clearly
- Think in orders of magnitude
- Consider read/write ratio

---

## 🎭 Interview Framework: The RADIO Method

> **Use this structure in EVERY system design interview** - covers all bases interviewers expect.

### R - Requirements (5 min)

**Functional Requirements** (What the system does):
```
Example: URL Shortener
✅ Given long URL, return short URL
✅ Short URL redirects to original
✅ Custom short URLs (optional)
✅ Expiration time (optional)
❌ Analytics (out of scope for now)
❌ User accounts (simplify for interview)
```

**Non-Functional Requirements** (How well it performs):
```
✅ High availability (99.9% uptime)
✅ Low latency (<100ms redirects)
✅ Scalable (handle millions of URLs)
✅ Read-heavy (100:1 read:write ratio)
⚠️ Eventual consistency OK (not financial data)
```

**Scale Estimates**:
```
- 100M URLs shortened/month → ~40 writes/sec
- 100:1 ratio → 4K redirects/sec
- Storage: 100M × 500 bytes = 50GB/month
- 5 year retention → 3TB
```

### A - API Design (5 min)

**Design the interface FIRST** (shows you understand the problem):
```
POST /api/v1/shorten
Request: { "long_url": "https://...", "expiry": "2025-12-31" }
Response: { "short_url": "https://tiny.url/a3x9f", "expiry": 1735689600 }

GET /api/v1/{short_code}
Response: 302 Redirect to original URL
```

**Pro tip**: Mention versioning (/v1/), RESTful conventions, status codes

### D - Data Model (10 min)

**Design your schema** (SQL vs NoSQL decision):
```sql
-- URL Shortener Example (SQL works fine here)

TABLE urls (
  id BIGINT PRIMARY KEY,
  short_code VARCHAR(10) UNIQUE NOT NULL,  -- indexed!
  long_url TEXT NOT NULL,
  user_id BIGINT,  -- if we add users later
  created_at TIMESTAMP,
  expiry_at TIMESTAMP,
  click_count INT DEFAULT 0
);

INDEX idx_short_code ON urls(short_code);  -- CRITICAL for lookups!
```

**Key decisions to mention:**
- Why this database? (SQL for simple schema, strong consistency)
- What indexes? (short_code lookups are most common)
- Sharding strategy? (range-based on ID or hash-based on short_code)

### I - Infrastructure/High-Level Design (15 min)

**Draw the boxes** (Load balancer → Servers → Database → Cache):

```
                 [Load Balancer]
                        |
        +---------------+----------------+
        |               |                |
   [App Server]   [App Server]    [App Server]
        |               |                |
        +-------+-------+--------+-------+
                |                |
           [Redis Cache]    [Primary DB]
                                  |
                          [Read Replicas]
```

**Discuss:**
- **Load balancer**: Round-robin or least connections
- **App servers**: Stateless, horizontally scalable
- **Cache**: Redis for short_code → long_url lookups (99% hit rate)
- **Database**: MySQL primary + read replicas (for analytics)
- **CDN**: Not needed (redirects, not content delivery)

### O - Optimizations & Deep Dives (15 min)

**Interviewer will probe specific areas**:

**1. Short Code Generation**:
```
Option A: Base62 encoding of auto-increment ID
- Pros: Simple, no collisions, predictable length
- Cons: Predictable (security issue?), single-point (ID generator)

Option B: Hash (MD5) and take first 6-8 characters  
- Pros: Distributed generation
- Cons: Collision handling needed, longer codes

Option C: Pre-generate pool of codes
- Pros: Super fast writes, distributed
- Cons: Coordination overhead, code "wastage"

✅ Recommended: Base62 with distributed ID generator (Snowflake pattern)
```

**2. Cache Strategy**:
```
- Cache hot URLs (write-through cache)
- TTL = 1 hour (URLs don't change often)
- Cache hit ratio target: 95%+
- Cache size: ~100MB for 1M hot URLs
```

**3. Database Scaling**:
```
- Vertical scaling: 64GB RAM, 16 cores (handles ~10K QPS)
- Read replicas: For analytics queries (don't slow down writes)
- Sharding: If >100M URLs/month, shard by hash(short_code) % N
```

**4. Handling Deleted/Expired URLs**:
```
- Soft delete (mark as deleted, don't remove row)
- Background job cleans up daily (DELETE WHERE expiry_at < NOW())
- Return 404 if expired/deleted
```

---

## 🔥 CAP Theorem - Practical Examples

> **Theory everyone knows, but can you apply it?** Interviewers want real-world trade-offs.

### CAP Theorem Refresher

**You can only choose 2 of 3:**
- **C**onsistency: All nodes see the same data at the same time
- **A**vailability: Every request receives a response (success or failure)
- **P**artition Tolerance: System continues operating despite network failures

**Reality**: Network partitions WILL happen, so you're really choosing between **CP** or **AP**.

### Real-World System Examples

#### CP Systems (Consistency + Partition Tolerance)

**MongoDB (CP mode with replica sets)**:
```
Scenario: Bank account balance
- User A transfers $100 to User B
- Network partition splits replica set
- PRIMARY becomes isolated from SECONDARIES
- MongoDB BLOCKS writes until PRIMARY can reach majority
- Result: System unavailable BUT data stays consistent

Why CP: Can't have duplicate money (consistency > availability)
```

**HBase, Redis (CP with Sentinel/Cluster)**:
- Master-slave with quorum-based writes
- If master isolated, new master elected
- Brief unavailability (<30s) during election

#### AP Systems (Availability + Partition Tolerance)

**Cassandra, DynamoDB (AP mode)**:
```
Scenario: Social media likes count
- User likes a post
- Network partition occurs
- Write succeeds on 1 replica (quorum = 1)
- Other replicas get update later (eventually consistent)
- Brief period where like counts differ across nodes

Why AP: Slight inconsistency OK (availability > consistency)
```

**Cassandra Tuning** (adjustable via quorum):
```python
# Write to 3 replicas, wait for 2 ACKs
WRITE_CONSISTENCY = QUORUM  # More consistency, less availability

# Write to 3 replicas, wait for 1 ACK
WRITE_CONSISTENCY = ONE  # More availability, less consistency
```

### CA Systems (Consistency + Availability) - Myth!

**Traditional RDBMS (Single Node)**:
- MySQL, PostgreSQL on single server
- No partition tolerance (if network fails, system fails)
- Not distributed → Not CAP-relevant!

**Real talk**: Once you distribute data, partitions happen. CA doesn't exist at scale.

### Interview Question: "Would you use CP or AP for...?"

| Use Case | Choice | Reasoning |
|----------|--------|-----------|
| **Banking transactions** | CP | Money must be correct, brief downtime acceptable |
| **Social media likes** | AP | Slight delay in like count OK, must stay available |
| **E-commerce inventory** | CP | Can't oversell items (consistency critical) |
| **E-commerce product catalog** | AP | Showing old price briefly OK vs site down |
| **Ride-sharing driver location** | AP | Slightly stale location OK vs no drivers shown |
| **Healthcare patient records** | CP | Wrong diagnosis data is unacceptable |
| **Netflix recommendations** | AP | Slightly stale recs OK vs no video playback |
| **Uber payment processing** | CP | Payment integrity over availability |
| **Twitter timeline** | AP | Old tweets showing briefly OK vs Twitter down |

### PACELC - Beyond CAP

**More realistic**:
- **P**artition → choose **A**vailability or **C**onsistency
- **E**lse (no partition) → choose **L**atency or **C**onsistency

**Examples**:
- **Cassandra**: PA/EL (availability + low latency)
- **MongoDB**: PC/EC (consistency always, even at cost of latency)
- **DynamoDB**: PA/EL (can be tuned to PC/EC with quorum)

---

## 🏗️ Monolith vs Microservices Decision

> **Don't just say "microservices are better"** - That's a red flag! There's nuance.

### Decision Matrix

| Factor | Monolith | Microservices |
|--------|----------|---------------|
| **Team size** | <10 engineers | 10+ engineers |
| **Deployment frequency** | Weekly/Monthly | Daily/Hourly |
| **System complexity** | Simple CRUD | Complex domains |
| **Scale requirements** | <10K RPS | >100K RPS |
| **Tech diversity** | Single stack preferred | Polyglot needed |
| **Development speed** | Faster (initially) | Slower (initially), faster (long-term) |

### When Monolith is Better

**Startups, MVPs, Small Teams:**
```
✅ Faster to market (no service boundaries to design)
✅ Easier debugging (single codebase, stack trace)
✅ Simpler deployment (one binary, one database)
✅ Lower operational complexity (no service mesh, discovery)
✅ Easier testing (no network calls to mock)
```

**Example**: **Instagram** was a monolith until 100M+ users!

### When Microservices Make Sense

**Large teams, high scale:**
```
✅ Independent scaling (auth service different load than video processing)
✅ Independent deployment (ship features without full system release)
✅ Team autonomy (each team owns a service)
✅ Fault isolation (payment service down ≠ whole site down)
✅ Technology flexibility (Python for ML, Go for performance)
```

**Example**: **Netflix** - 700+ microservices, billions of requests/day

### The Migration Path (Strangler Fig Pattern)

**Don't rewrite everything overnight!**

```
Step 1: Identify bounded contexts
   Example: User Auth, Payment, Inventory, Recommendations

Step 2: Extract one service at a time (high value first)
   Start with: Authentication (used everywhere, security-critical)

Step 3: API Gateway pattern
   [Client] → [API Gateway] → [Monolith + New Service]
   Gateway routes /auth/* to new service, rest to monolith

Step 4: Gradually move more services out
   Takes 1-3 years for large companies!

Step 5: Eventually decommission monolith
   Or keep it! Monolith for core CRUD, services for special needs.
```

### Microservices Challenges (Mention These!)

**1. Distributed System Complexity**:
- Network calls can fail (retry logic, circuit breakers)
- Distributed tracing needed (Zipkin, Jaeger)
- Service discovery (Consul, etcd)

**2. Data Consistency**:
- No ACID across services
- Need Saga pattern or event sourcing
- Eventual consistency trade-offs

**3. Operational Overhead**:
- Deploy 50 services vs 1 binary
- Monitor 50 services (Prometheus, Grafana)
- Debug cross-service issues

**4. Testing Complexity**:
- Integration tests require multiple services running
- Contract testing (Pact) needed
- End-to-end tests brittle

### Hybrid Approach (Best of Both)

**"Monolith-first, extract services later"**:
```
- Start with modular monolith (clean boundaries WITHIN codebase)
- Use interfaces/modules to separate concerns
- When a module needs independent scaling → extract to service
- Keep most code in monolith, extract ~5-10 critical services
```

**Example**: **Shopify** - Monolithic Rails app + specialized services (payment, shipping)

---

## 🔐 Auth at Scale (Interviewers LOVE This Topic)

### Authentication vs Authorization

| Authentication | Authorization |
|----------------|---------------|
| **Who are you?** | **What can you do?** |
| Login with password | Check if user can access resource |
| Returns user identity | Returns permissions |
| Happens once (login) | Happens every request |

### Session-Based Auth (Traditional)

```
1. User logs in with username/password
2. Server creates session, stores in Redis/database
3. Server returns session_id in cookie
4. Subsequent requests include cookie
5. Server looks up session_id to get user info
```

**Pros**: Can revoke sessions immediately (logout)
**Cons**: Server-side storage, not scalable across microservices

### Token-Based Auth (JWT - Modern Standard)

```
1. User logs in
2. Server generates JWT (signed token containing user_id, role, expiry)
3. Client stores JWT (localStorage or cookie)
4. Client sends JWT in Authorization header: "Bearer <token>"
5. Server validates signature (no database lookup needed!)
```

**JWT Structure**:
```
header.payload.signature
eyJhbGc...  .  eyJ1c2VyX2lkIjo...  .  SflKxwRJSMeKKF2QT...
(Base64)      (Base64 user data)      (HMAC signature)
```

**Pros**: Stateless (no server-side storage), scales horizontally
**Cons**: Can't revoke (until expiry), token size (200-1000 bytes)

### OAuth 2.0 (Third-Party Login)

**"Login with Google/Facebook/GitHub"**:
```
1. User clicks "Login with Google"
2. Redirect to Google's auth page
3. User authorizes your app
4. Google redirects back with authorization_code
5. Your server exchanges code for access_token
6. Use access_token to fetch user profile from Google API
7. Create session/JWT for user in YOUR system
```

**Why use OAuth?**:
- Don't store passwords (security)
- Leverage existing accounts (reduce friction)
- Access user data from provider (email, profile pic)

### API Keys for Service-to-Service

```
Example: Mobile app → Your API → Stripe API

Your API key for Stripe:
- Long-lived token (doesn't expire)
- Stored as environment variable
- Rotated quarterly
- Never exposed to client
```

### Role-Based Access Control (RBAC)

```python
# Simple RBAC example
roles = {
  "admin": ["read", "write", "delete", "manage_users"],
  "editor": ["read", "write"],
  "viewer": ["read"]
}

def check_permission(user, action, resource):
  user_permissions = roles[user.role]
  return action in user_permissions
```

**Interview tip**: Mention RBAC vs ABAC (Attribute-Based) for complex permissions.

### Scaling Auth

**Problem**: Auth service becomes bottleneck (every request hits it)

**Solution 1: JWT** (no auth service call needed)
```
API Gateway validates JWT signature → forwards to backend
```

**Solution 2: Cache user permissions in Redis**
```
Cache key: "user:123:permissions" → ["read", "write"]
TTL: 5 minutes
```

**Solution 3: Service mesh (Istio) with mTLS**
```
Services communicate with mutual TLS certificates
No per-request auth check needed
```

---

## 📊 Monitoring & Observability (Often Overlooked!)

### The Three Pillars

**1. Metrics** (What's happening?):
```
- Request rate (QPS)
- Error rate (5xx errors)
- Latency (p50, p95, p99)
- Resource usage (CPU, memory, disk)

Tools: Prometheus, Grafana, Datadog
```

**2. Logs** (Why did it happen?):
```
- Structured logs (JSON format)
- Include: timestamp, request_id, user_id, error message
- Centralized logging (ELK stack, Loki)

Example:
{
  "timestamp": "2025-11-10T10:30:00Z",
  "level": "ERROR",
  "request_id": "abc-123",
  "user_id": 456,
  "message": "Database connection timeout",
  "query": "SELECT * FROM users WHERE id=456"
}
```

**3. Traces** (Where's the bottleneck?):
```
- Track request through multiple services
- Shows: which service, how long, dependencies

Example trace:
API Gateway (10ms) → Auth Service (50ms) → User Service (200ms) → Database (180ms)
Total: 260ms (database is the bottleneck!)

Tools: Jaeger, Zipkin, OpenTelemetry
```

### Key Metrics to Monitor

```
Golden Signals (Google SRE):
1. Latency: How long requests take
2. Traffic: How many requests
3. Errors: Rate of failed requests  
4. Saturation: How "full" is the system (CPU, memory)

RED Metrics (for services):
1. Rate: Requests per second
2. Errors: Error rate
3. Duration: Latency distribution

USE Metrics (for resources):
1. Utilization: % time resource is busy
2. Saturation: Queue depth
3. Errors: Error count
```

### Alert Thresholds (Don't Alert on Everything!)

```
❌ Bad: Alert on p50 latency > 100ms
   (Too noisy, p50 fluctuates normally)

✅ Good: Alert on p99 latency > 1000ms for 5 minutes
   (Real user impact, not transient spikes)

❌ Bad: Alert on any error
   (Errors happen, some are expected)

✅ Good: Alert on error rate > 1% for 10 minutes
   (Significant degradation, not isolated incidents)
```

### Interview Answer Template

**When asked "How would you monitor this system?"**:

```
1. Metrics (Prometheus + Grafana):
   - QPS dashboard (reads vs writes)
   - Error rate (5xx errors, timeouts)
   - Latency (p50, p95, p99)
   - Resource usage (CPU, memory per pod)

2. Logs (ELK stack):
   - Structured JSON logs
   - Include request_id for tracing
   - Centralized (all services → Elasticsearch)
   - Retention: 30 days

3. Traces (Jaeger):
   - Track requests across microservices
   - Identify slow dependencies
   - Correlation with request_id

4. Alerts (PagerDuty):
   - p99 latency > 1s for 5 min
   - Error rate > 1% for 10 min
   - CPU > 80% for 15 min
   - Database connection pool exhaustion

5. Dashboards:
   - Real-time: Current QPS, errors, latency
   - Historical: Trends over 7/30 days
   - Per-service: Drill down into specific component
```

---

## 🎤 Common Interview Questions & Frameworks

### Q1: "Design a URL Shortener"

**Framework**: RADIO method (Requirements → API → Data → Infrastructure → Optimizations)

**Time**: 45 minutes total
- Requirements (5 min): Functional (shorten, redirect), Non-functional (low latency, scalable)
- API (5 min): POST /shorten, GET /{code}
- Data Model (10 min): urls table, short_code index
- Infrastructure (15 min): LB → App servers → Redis → MySQL
- Optimizations (10 min): Base62 encoding, cache strategy, analytics

**Gotchas to mention**:
- Short code collision handling
- Expired URL cleanup
- Custom short codes (unique constraint)
- Rate limiting (prevent abuse)

### Q2: "Design Instagram"

**Key features**: Upload photos, follow users, newsfeed, likes/comments

**Critical decisions**:
- Blob storage for images (S3, GCS)
- Hybrid fan-out (push for regular users, pull for celebrities)
- CDN for image delivery
- Database sharding (by user_id)
- Cache hot feed content (Redis)

**Scale numbers**:
- 1B users, 500M DAU
- 100M photos uploaded/day
- Each photo: 2MB × 3 sizes = 6MB
- Storage: 600M × 6MB = 3.6PB/day (need S3 deep archive!)

### Q3: "Design a Rate Limiter"

**Algorithms**: Token bucket, leaky bucket, fixed/sliding window

**Implementation options**:
- API Gateway (centralized)
- Redis (distributed counter)
- In-memory (per-server, approximate)

**Key decisions**:
- Rate limit scope (per-user vs per-IP vs per-API-key)
- Time window (per second, minute, hour)
- Exceeded response (429 status, Retry-After header)
- Distributed coordination (Redis vs local)

### Q4: "Design a Chat System (WhatsApp/Slack)"

**Core features**: 1-to-1 messaging, group chat, online status, read receipts

**Architecture**:
- WebSocket servers (persistent connections)
- Message queue (Kafka for delivery)
- Cassandra (message storage, user_id + timestamp clustering)
- Redis (online status, pub/sub for real-time)

**Challenges**:
- Message ordering (use sequence numbers)
- Offline message delivery (queue until user online)
- Group chat (fan-out to N users)
- End-to-end encryption (Signal Protocol)

### Q5: "Design a Notification System"

**Channels**: Push notifications, SMS, email, in-app

**Components**:
- Event producers (user actions trigger notifications)
- Notification service (formats messages per channel)
- Third-party APIs (FCM for push, Twilio for SMS, SendGrid for email)
- Retry logic (exponential backoff)

**Optimizations**:
- Batching (combine multiple notifications)
- User preferences (opt-out, quiet hours)
- Rate limiting (don't spam users)
- Priority queue (urgent vs regular)

---

## 🧠 Mental Models & Heuristics

### "How big should my cache be?"

**Rule of thumb**: Cache 20% of your data (covers 80% of requests)
```
If daily requests = 1B
And unique data items = 100M
Cache size = 100M × 20% = 20M items
Assuming 1KB per item = 20GB

Practical: 64GB RAM can cache 64M items (more than enough!)
```

### "Do I need a message queue?"

**Yes, if**:
- Async processing (email sending, video encoding)
- Decoupling services (producer/consumer independent)
- Traffic spikes (queue smooths load)
- Retries needed (failed jobs requeued)

**No, if**:
- Synchronous response required (user waiting)
- Simple request-response (REST API)
- Low volume (<100 requests/min)

### "When to shard my database?"

**Shard when**:
- Single database > 1TB (hard to backup/restore)
- QPS > 10K (single DB bottleneck)
- Hot partitions (some users generate 10x traffic)

**Before sharding, try**:
- Read replicas (scale reads)
- Caching (reduce DB hits by 80%+)
- Vertical scaling (bigger machine)
- Indexes (slow queries often just need indexes)

### "SQL or NoSQL?"

**Quick decision tree**:
```
Do you need ACID transactions? (bank transfers, inventory)
  └─ Yes → SQL (PostgreSQL)
  └─ No  → Continue

Do you need complex joins? (many-to-many relationships)
  └─ Yes → SQL
  └─ No  → Continue

Is your data highly structured? (fixed schema)
  └─ Yes → SQL
  └─ No  → Continue

Do you need >100K QPS or >10TB data?
  └─ Yes → NoSQL (Cassandra, DynamoDB)
  └─ No  → SQL is fine!

Default: Start with PostgreSQL, migrate to NoSQL when needed.
```

---

## 🚨 Red Flags to Avoid

**These responses will hurt you**:

❌ "We'll use microservices because they're modern"
   ✅ "We'll start with a monolith for speed, extract services as scale demands"

❌ "NoSQL is faster than SQL"
   ✅ "NoSQL trades consistency for horizontal scalability; SQL is fine until 100K+ QPS"

❌ "We'll use Kubernetes"
   ✅ "For 10K users, Heroku/EC2 is simpler. Kubernetes adds value at 100K+ RPS"

❌ "Blockchain for data integrity"
   ✅ "Merkle trees provide integrity without blockchain overhead"

❌ "Machine learning will recommend content"
   ✅ "Start with collaborative filtering (simpler), add ML when data >1M users"

❌ "I'll add caching everywhere"
   ✅ "Cache hot data (80/20 rule), measure hit ratio, avoid premature caching"

❌ "MongoDB because it's flexible"
   ✅ "MongoDB if schema changes frequently AND don't need ACID. PostgreSQL JSONB is a hybrid option."

---

## 🎯 Final Checklist for Interviews

**Before you finish your design, verify**:

☑️ **Stated assumptions clearly** (DAU, read/write ratio, storage per user)
☑️ **Calculated rough numbers** (QPS, storage, bandwidth)
☑️ **Defined APIs** (endpoints, request/response format)
☑️ **Chose appropriate database(s)** (SQL vs NoSQL with reasoning)
☑️ **Included caching layer** (what to cache, eviction policy)
☑️ **Discussed scaling strategy** (horizontal scaling, sharding, replication)
☑️ **Mentioned monitoring** (metrics, logs, alerts)
☑️ **Identified bottlenecks** (database, network, CPU, memory)
☑️ **Proposed optimizations** (CDN, load balancing, async processing)
☑️ **Acknowledged trade-offs** (consistency vs availability, cost vs performance)

---

**End of SDE-II Interview Essentials** 🎓

---

# 💻 Code-Level System Design Patterns

> **Interviewers often ask: "How would you implement this?"** - Show you can code, not just draw boxes.

## Circuit Breaker Pattern

**Problem**: Calling a failing service repeatedly wastes resources and delays responses.

**Solution**: Detect failures, "open" the circuit, fail fast until service recovers.

```python
import time
from enum import Enum

class CircuitState(Enum):
    CLOSED = "closed"  # Normal operation
    OPEN = "open"      # Failing, reject requests
    HALF_OPEN = "half_open"  # Testing recovery

class CircuitBreaker:
    def __init__(self, failure_threshold=5, timeout=60, success_threshold=2):
        self.failure_threshold = failure_threshold  # Open after N failures
        self.timeout = timeout  # Seconds to wait before trying again
        self.success_threshold = success_threshold  # Close after N successes
        
        self.failure_count = 0
        self.success_count = 0
        self.last_failure_time = None
        self.state = CircuitState.CLOSED
    
    def call(self, func, *args, **kwargs):
        """Execute function through circuit breaker"""
        if self.state == CircuitState.OPEN:
            if time.time() - self.last_failure_time > self.timeout:
                # Try again after timeout
                self.state = CircuitState.HALF_OPEN
                self.success_count = 0
            else:
                raise Exception("Circuit breaker is OPEN - failing fast")
        
        try:
            result = func(*args, **kwargs)
            self._on_success()
            return result
        except Exception as e:
            self._on_failure()
            raise e
    
    def _on_success(self):
        self.failure_count = 0
        if self.state == CircuitState.HALF_OPEN:
            self.success_count += 1
            if self.success_count >= self.success_threshold:
                self.state = CircuitState.CLOSED  # Service recovered!
    
    def _on_failure(self):
        self.failure_count += 1
        self.last_failure_time = time.time()
        if self.failure_count >= self.failure_threshold:
            self.state = CircuitState.OPEN  # Too many failures, open circuit

# Usage example
import requests

payment_circuit = CircuitBreaker(failure_threshold=3, timeout=30)

def charge_customer(customer_id, amount):
    """This might fail if payment service is down"""
    response = requests.post(
        "https://payment-api.com/charge",
        json={"customer_id": customer_id, "amount": amount}
    )
    response.raise_for_status()  # Raises exception on 4xx/5xx
    return response.json()

# Protected call
try:
    result = payment_circuit.call(charge_customer, customer_id=123, amount=99.99)
    print(f"Payment successful: {result}")
except Exception as e:
    print(f"Payment failed (circuit breaker): {e}")
    # Return cached response, show error to user, or use fallback
```

**Interview talking points**:
- State transitions: CLOSED → OPEN (failures) → HALF_OPEN (timeout) → CLOSED (recovery)
- Prevents cascading failures (one service down doesn't bring down others)
- Used by Netflix Hystrix, Resilience4j, AWS service clients

---

## Rate Limiter (Token Bucket Implementation)

```python
import time
import threading

class TokenBucket:
    def __init__(self, capacity, refill_rate):
        """
        capacity: Maximum number of tokens (burst size)
        refill_rate: Tokens added per second
        """
        self.capacity = capacity
        self.refill_rate = refill_rate
        self.tokens = capacity
        self.last_refill = time.time()
        self.lock = threading.Lock()
    
    def _refill(self):
        """Add tokens based on time elapsed"""
        now = time.time()
        elapsed = now - self.last_refill
        tokens_to_add = elapsed * self.refill_rate
        self.tokens = min(self.capacity, self.tokens + tokens_to_add)
        self.last_refill = now
    
    def consume(self, tokens=1):
        """Try to consume tokens; return True if allowed, False if rate limited"""
        with self.lock:
            self._refill()
            if self.tokens >= tokens:
                self.tokens -= tokens
                return True
            return False

# Distributed version with Redis
import redis

class DistributedTokenBucket:
    def __init__(self, redis_client, key, capacity, refill_rate):
        self.redis = redis_client
        self.key = key
        self.capacity = capacity
        self.refill_rate = refill_rate
    
    def consume(self, tokens=1):
        """Lua script ensures atomic operation in Redis"""
        lua_script = """
        local key = KEYS[1]
        local capacity = tonumber(ARGV[1])
        local refill_rate = tonumber(ARGV[2])
        local tokens_requested = tonumber(ARGV[3])
        local now = tonumber(ARGV[4])
        
        -- Get current state
        local bucket = redis.call('HMGET', key, 'tokens', 'last_refill')
        local tokens = tonumber(bucket[1]) or capacity
        local last_refill = tonumber(bucket[2]) or now
        
        -- Refill tokens
        local elapsed = now - last_refill
        tokens = math.min(capacity, tokens + (elapsed * refill_rate))
        
        -- Try to consume
        if tokens >= tokens_requested then
            tokens = tokens - tokens_requested
            redis.call('HMSET', key, 'tokens', tokens, 'last_refill', now)
            redis.call('EXPIRE', key, 3600)  -- Auto-cleanup after 1 hour
            return 1  -- Allowed
        else
            return 0  -- Rate limited
        end
        """
        
        result = self.redis.eval(
            lua_script,
            1,  # Number of keys
            self.key,
            self.capacity,
            self.refill_rate,
            tokens,
            time.time()
        )
        return result == 1

# Usage example
r = redis.Redis(host='localhost', port=6379)
limiter = DistributedTokenBucket(
    redis_client=r,
    key="user:123:api_calls",
    capacity=100,  # Burst up to 100 requests
    refill_rate=10  # 10 requests per second sustained
)

def api_call(user_id):
    if not limiter.consume(tokens=1):
        return {"error": "Rate limit exceeded", "status": 429}
    # Process request...
    return {"data": "success"}
```

**Interview talking points**:
- Allows bursts (good for APIs with occasional spikes)
- Distributed version uses Redis + Lua for atomic operations
- Alternative: Sliding window (more accurate but more complex)

---

## Consistent Hashing with Virtual Nodes

```python
import hashlib
from bisect import bisect_right

class ConsistentHashRing:
    def __init__(self, num_virtual_nodes=150):
        self.num_virtual_nodes = num_virtual_nodes
        self.ring = {}  # hash -> node_id
        self.sorted_keys = []  # Sorted list of hashes for binary search
    
    def _hash(self, key):
        """Hash function (MD5 for simplicity; use xxHash in production)"""
        return int(hashlib.md5(key.encode()).hexdigest(), 16)
    
    def add_node(self, node_id):
        """Add a physical node with virtual nodes"""
        for i in range(self.num_virtual_nodes):
            virtual_key = f"{node_id}:{i}"
            hash_value = self._hash(virtual_key)
            self.ring[hash_value] = node_id
        
        self.sorted_keys = sorted(self.ring.keys())
        print(f"Added node {node_id} with {self.num_virtual_nodes} virtual nodes")
    
    def remove_node(self, node_id):
        """Remove a physical node"""
        for i in range(self.num_virtual_nodes):
            virtual_key = f"{node_id}:{i}"
            hash_value = self._hash(virtual_key)
            del self.ring[hash_value]
        
        self.sorted_keys = sorted(self.ring.keys())
        print(f"Removed node {node_id}")
    
    def get_node(self, key):
        """Find which node this key belongs to"""
        if not self.ring:
            return None
        
        hash_value = self._hash(key)
        
        # Binary search for the next node clockwise on the ring
        idx = bisect_right(self.sorted_keys, hash_value)
        
        # Wrap around if we're past the end
        if idx == len(self.sorted_keys):
            idx = 0
        
        return self.ring[self.sorted_keys[idx]]

# Usage example
hash_ring = ConsistentHashRing(num_virtual_nodes=150)

# Add servers
hash_ring.add_node("server1")
hash_ring.add_node("server2")
hash_ring.add_node("server3")

# Route requests
keys = ["user:1234", "user:5678", "session:abc123", "cache:xyz"]
for key in keys:
    node = hash_ring.get_node(key)
    print(f"{key} → {node}")

# Simulate server failure
print("\nServer2 failed, removing...")
hash_ring.remove_node("server2")

print("\nAfter removal:")
for key in keys:
    node = hash_ring.get_node(key)
    print(f"{key} → {node}")

# Output shows minimal key remapping!
```

**Interview talking points**:
- Virtual nodes solve uneven load distribution
- Adding/removing nodes only affects neighbors (not all keys)
- Used in Cassandra, DynamoDB, Redis Cluster

---

## LRU Cache (Interview Classic!)

```python
class Node:
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = {}  # key -> Node
        
        # Doubly linked list with dummy head/tail
        self.head = Node(0, 0)  # Most recently used
        self.tail = Node(0, 0)  # Least recently used
        self.head.next = self.tail
        self.tail.prev = self.head
    
    def _remove(self, node):
        """Remove node from linked list"""
        prev_node = node.prev
        next_node = node.next
        prev_node.next = next_node
        next_node.prev = prev_node
    
    def _add_to_head(self, node):
        """Add node right after head (most recently used)"""
        node.next = self.head.next
        node.prev = self.head
        self.head.next.prev = node
        self.head.next = node
    
    def get(self, key):
        """Get value and mark as recently used"""
        if key in self.cache:
            node = self.cache[key]
            self._remove(node)
            self._add_to_head(node)
            return node.value
        return -1  # Cache miss
    
    def put(self, key, value):
        """Add/update value"""
        if key in self.cache:
            # Update existing
            node = self.cache[key]
            node.value = value
            self._remove(node)
            self._add_to_head(node)
        else:
            # Add new
            if len(self.cache) >= self.capacity:
                # Evict LRU (tail.prev)
                lru_node = self.tail.prev
                self._remove(lru_node)
                del self.cache[lru_node.key]
            
            new_node = Node(key, value)
            self.cache[key] = new_node
            self._add_to_head(new_node)

# Usage
cache = LRUCache(capacity=3)
cache.put(1, "A")
cache.put(2, "B")
cache.put(3, "C")
print(cache.get(1))  # "A" (1 is now most recent)
cache.put(4, "D")  # Evicts key 2 (LRU)
print(cache.get(2))  # -1 (cache miss)
```

**Interview talking points**:
- O(1) get and put operations (hash map + doubly linked list)
- Evicts least recently used when capacity reached
- Production versions add TTL, write-through/write-behind policies

---

## Bloom Filter (Space-Efficient Set Membership)

```python
import mmh3  # MurmurHash3 (fast, non-cryptographic)
from bitarray import bitarray

class BloomFilter:
    def __init__(self, size, num_hash_functions):
        """
        size: Number of bits in the filter
        num_hash_functions: Number of hash functions (k)
        
        Rule of thumb: 
        - m (size) = -n * ln(p) / (ln(2)^2)
        - k (num_hash_functions) = (m/n) * ln(2)
        where n = expected items, p = false positive rate
        
        Example: 1M items, 1% false positive rate
        - m ≈ 9,585,059 bits (1.14 MB)
        - k ≈ 7 hash functions
        """
        self.size = size
        self.num_hash_functions = num_hash_functions
        self.bit_array = bitarray(size)
        self.bit_array.setall(0)
    
    def add(self, item):
        """Add item to the filter"""
        for i in range(self.num_hash_functions):
            # Generate multiple hash values from a single hash function
            hash_value = mmh3.hash(item, i) % self.size
            self.bit_array[hash_value] = 1
    
    def contains(self, item):
        """Check if item *might* be in the set"""
        for i in range(self.num_hash_functions):
            hash_value = mmh3.hash(item, i) % self.size
            if self.bit_array[hash_value] == 0:
                return False  # Definitely NOT in set
        return True  # Probably in set (might be false positive)

# Usage example: Checking if username is taken
usernames_bf = BloomFilter(size=10_000_000, num_hash_functions=7)

# Add existing usernames
existing_users = ["alice", "bob", "charlie", "david"]
for user in existing_users:
    usernames_bf.add(user)

# Check availability (fast pre-check before database query)
def is_username_taken(username):
    if usernames_bf.contains(username):
        # Might be taken, check database to confirm
        # return db.query("SELECT 1 FROM users WHERE username = ?", username)
        return "Possibly taken (check DB)"
    else:
        # Definitely available (skip DB query!)
        return "Available!"

print(is_username_taken("alice"))  # "Possibly taken"
print(is_username_taken("zebra"))  # "Available!" (skips DB!)

# False positive example (rare, ~1% with our settings)
print(is_username_taken("xyz123"))  # Might say "Possibly taken" even if not in DB
```

**Interview talking points**:
- **Space-efficient**: 1M items in ~1MB (vs 1M × 50 bytes = 50MB in hash set)
- **No false negatives**: If it says "not present," it's definitely not present
- **Has false positives**: ~1% with typical settings (tunable)
- **Use cases**: 
  - Weak password check (is password in breached password list?)
  - Crawler deduplication (have we seen this URL?)
  - Database query optimization (skip query if key definitely doesn't exist)

---

## Retry with Exponential Backoff

```python
import time
import random

def exponential_backoff_retry(func, max_retries=5, base_delay=1, max_delay=60, jitter=True):
    """
    Retry function with exponential backoff
    
    Delays: 1s, 2s, 4s, 8s, 16s, ... (capped at max_delay)
    Jitter: Adds randomness to avoid thundering herd
    """
    for attempt in range(max_retries):
        try:
            return func()
        except Exception as e:
            if attempt == max_retries - 1:
                # Last attempt failed, give up
                raise e
            
            # Calculate delay: 2^attempt * base_delay
            delay = min(base_delay * (2 ** attempt), max_delay)
            
            # Add jitter (random ±25%)
            if jitter:
                delay *= (0.75 + 0.5 * random.random())
            
            print(f"Attempt {attempt + 1} failed: {e}. Retrying in {delay:.2f}s...")
            time.sleep(delay)

# Usage example
import requests

def fetch_user_data(user_id):
    response = requests.get(f"https://api.example.com/users/{user_id}", timeout=5)
    response.raise_for_status()  # Raises exception on 4xx/5xx
    return response.json()

try:
    user_data = exponential_backoff_retry(
        lambda: fetch_user_data(user_id=123),
        max_retries=5,
        base_delay=1,
        max_delay=30
    )
    print(f"User data: {user_data}")
except Exception as e:
    print(f"Failed after retries: {e}")
    # Fall back to cached data or show error to user
```

**Interview talking points**:
- Exponential backoff prevents overwhelming failing service
- Jitter prevents "thundering herd" (many clients retrying simultaneously)
- Use for: API calls, database connections, message queue consumers
- Don't retry: 4xx errors (client errors), user-initiated cancellations

---

## Idempotency Key Pattern (Prevent Duplicate Payments!)

```python
import uuid
import redis

class IdempotentPaymentProcessor:
    def __init__(self, redis_client):
        self.redis = redis_client
        self.ttl = 86400  # 24 hours
    
    def process_payment(self, idempotency_key, customer_id, amount):
        """
        Process payment with idempotency guarantee
        If same idempotency_key sent twice, return cached result
        """
        # Check if we've already processed this request
        cached_result = self.redis.get(f"payment:{idempotency_key}")
        if cached_result:
            print(f"Idempotent request detected, returning cached result")
            return eval(cached_result)  # In production, use JSON
        
        # Process payment (this should only happen once!)
        try:
            result = self._charge_customer(customer_id, amount)
            
            # Cache result for 24 hours
            self.redis.setex(
                f"payment:{idempotency_key}",
                self.ttl,
                str(result)
            )
            
            return result
        except Exception as e:
            # Don't cache failures (allow retry)
            raise e
    
    def _charge_customer(self, customer_id, amount):
        """Actual payment processing (hits Stripe API, etc.)"""
        print(f"Charging customer {customer_id} ${amount}...")
        # Simulate payment processing
        time.sleep(1)
        return {
            "payment_id": str(uuid.uuid4()),
            "customer_id": customer_id,
            "amount": amount,
            "status": "success"
        }

# Usage example
r = redis.Redis(host='localhost', port=6379)
payment_processor = IdempotentPaymentProcessor(r)

# Client generates idempotency key (usually UUID)
idempotency_key = str(uuid.uuid4())

# First request: processes payment
result1 = payment_processor.process_payment(
    idempotency_key=idempotency_key,
    customer_id=123,
    amount=99.99
)
print(f"First request: {result1}")

# Duplicate request (network retry, user double-click): returns cached result
result2 = payment_processor.process_payment(
    idempotency_key=idempotency_key,  # Same key!
    customer_id=123,
    amount=99.99
)
print(f"Duplicate request: {result2}")

# Same payment_id proves no double-charge!
assert result1["payment_id"] == result2["payment_id"]
```

**Interview talking points**:
- Critical for payments (prevent double-charging)
- Client generates UUID, sends with request
- Server caches result by idempotency key
- TTL should be longer than retry window (24 hours typical)
- Used by Stripe, PayPal, all payment APIs

---

## Database Connection Pooling

```python
import psycopg2
from psycopg2 import pool
import threading

class DatabasePool:
    def __init__(self, minconn=5, maxconn=20):
        """
        minconn: Minimum number of connections to keep open
        maxconn: Maximum number of connections allowed
        """
        self.pool = psycopg2.pool.ThreadedConnectionPool(
            minconn=minconn,
            maxconn=maxconn,
            host="localhost",
            database="myapp",
            user="postgres",
            password="password"
        )
    
    def execute_query(self, query, params=None):
        """Get connection from pool, execute query, return connection"""
        conn = None
        try:
            # Get connection from pool (blocks if all connections in use)
            conn = self.pool.getconn()
            
            with conn.cursor() as cursor:
                cursor.execute(query, params)
                if cursor.description:  # SELECT query
                    result = cursor.fetchall()
                    return result
                else:  # INSERT/UPDATE/DELETE
                    conn.commit()
                    return cursor.rowcount
        except Exception as e:
            if conn:
                conn.rollback()
            raise e
        finally:
            if conn:
                # Return connection to pool (doesn't close it!)
                self.pool.putconn(conn)
    
    def close_all(self):
        """Close all connections (call on app shutdown)"""
        self.pool.closeall()

# Usage example
db = DatabasePool(minconn=5, maxconn=20)

def fetch_user(user_id):
    result = db.execute_query(
        "SELECT id, name, email FROM users WHERE id = %s",
        (user_id,)
    )
    return result[0] if result else None

# Simulate concurrent requests (connection pool reuses connections)
def simulate_request(request_id):
    user = fetch_user(user_id=123)
    print(f"Request {request_id}: {user}")

threads = [threading.Thread(target=simulate_request, args=(i,)) for i in range(50)]
for t in threads:
    t.start()
for t in threads:
    t.join()

# Without pool: 50 new connections (slow!)
# With pool: Reuse 20 connections (fast!)

db.close_all()
```

**Interview talking points**:
- Creating new DB connections is slow (100-500ms)
- Pool maintains warm connections (reuse instead of recreate)
- Max connections prevents overwhelming database
- Default sizes: minconn=5-10, maxconn=20-50 (depends on load)

---

## Distributed Lock (Prevent Race Conditions)

```python
import redis
import time
import uuid

class RedisDistributedLock:
    def __init__(self, redis_client, lock_key, timeout=10):
        self.redis = redis_client
        self.lock_key = lock_key
        self.timeout = timeout
        self.lock_value = str(uuid.uuid4())  # Unique ID for this lock holder
    
    def acquire(self, blocking=True, timeout=30):
        """
        Try to acquire lock
        blocking: If True, wait until lock available
        timeout: Max time to wait (seconds)
        """
        start_time = time.time()
        
        while True:
            # SET key value NX EX timeout
            # NX: Only set if key doesn't exist
            # EX: Set expiry (prevent deadlock if holder crashes)
            acquired = self.redis.set(
                self.lock_key,
                self.lock_value,
                nx=True,  # Only set if not exists
                ex=self.timeout  # Auto-expire after timeout
            )
            
            if acquired:
                return True
            
            if not blocking:
                return False
            
            if time.time() - start_time > timeout:
                return False  # Timeout waiting for lock
            
            time.sleep(0.1)  # Wait before retrying
    
    def release(self):
        """Release lock (only if we own it)"""
        # Lua script for atomic check-and-delete
        lua_script = """
        if redis.call("GET", KEYS[1]) == ARGV[1] then
            return redis.call("DEL", KEYS[1])
        else
            return 0
        end
        """
        
        released = self.redis.eval(lua_script, 1, self.lock_key, self.lock_value)
        return released == 1
    
    def __enter__(self):
        """Context manager support: with lock:"""
        self.acquire()
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.release()

# Usage example: Prevent double-booking
r = redis.Redis(host='localhost', port=6379)

def book_seat(event_id, seat_id, user_id):
    """Book a seat for an event (must prevent double-booking!)"""
    lock_key = f"lock:event:{event_id}:seat:{seat_id}"
    
    with RedisDistributedLock(r, lock_key, timeout=5):
        # Critical section: only one process can execute this at a time
        
        # Check if seat already booked
        existing_booking = db_query(
            "SELECT user_id FROM bookings WHERE event_id = ? AND seat_id = ?",
            (event_id, seat_id)
        )
        
        if existing_booking:
            return {"error": "Seat already booked"}
        
        # Book the seat
        db_execute(
            "INSERT INTO bookings (event_id, seat_id, user_id) VALUES (?, ?, ?)",
            (event_id, seat_id, user_id)
        )
        
        return {"success": True, "message": f"Seat {seat_id} booked!"}

# Concurrent requests for same seat
import threading

def try_book(user_id):
    result = book_seat(event_id=1, seat_id=42, user_id=user_id)
    print(f"User {user_id}: {result}")

# Only ONE user will successfully book seat 42!
threads = [threading.Thread(target=try_book, args=(i,)) for i in range(10)]
for t in threads:
    t.start()
for t in threads:
    t.join()
```

**Interview talking points**:
- Prevents race conditions in distributed systems
- Auto-expiry prevents deadlock (if holder crashes)
- Lua script ensures atomic release (only lock owner can release)
- Use cases: inventory management, seat booking, ID generation

---

## Saga Pattern (Distributed Transactions)

```python
# Example: Order placement saga
# Order Service → Payment Service → Inventory Service → Shipping Service

class OrderSaga:
    def __init__(self):
        self.steps = []
        self.compensations = []
    
    def add_step(self, action, compensation):
        """
        action: Function to execute
        compensation: Function to undo (rollback) if later step fails
        """
        self.steps.append(action)
        self.compensations.append(compensation)
    
    def execute(self, context):
        """Execute saga steps; rollback on failure"""
        executed_steps = 0
        
        try:
            for i, step in enumerate(self.steps):
                print(f"Executing step {i + 1}...")
                step(context)
                executed_steps += 1
            
            print("Saga completed successfully!")
            return {"status": "success", "order_id": context.get("order_id")}
        
        except Exception as e:
            print(f"Step failed: {e}. Rolling back...")
            
            # Execute compensations in reverse order
            for i in range(executed_steps - 1, -1, -1):
                try:
                    print(f"Compensating step {i + 1}...")
                    self.compensations[i](context)
                except Exception as comp_error:
                    print(f"Compensation failed: {comp_error}")
            
            return {"status": "failed", "error": str(e)}

# Define saga steps
def create_order(context):
    order_id = generate_order_id()
    db_execute("INSERT INTO orders (id, user_id, status) VALUES (?, ?, 'pending')", 
               (order_id, context["user_id"]))
    context["order_id"] = order_id

def cancel_order(context):
    db_execute("UPDATE orders SET status = 'cancelled' WHERE id = ?", 
               (context["order_id"],))

def charge_payment(context):
    result = payment_service.charge(context["user_id"], context["amount"])
    if not result["success"]:
        raise Exception("Payment failed")
    context["payment_id"] = result["payment_id"]

def refund_payment(context):
    payment_service.refund(context["payment_id"])

def reserve_inventory(context):
    result = inventory_service.reserve(context["product_id"], context["quantity"])
    if not result["success"]:
        raise Exception("Out of stock")
    context["reservation_id"] = result["reservation_id"]

def release_inventory(context):
    inventory_service.release(context["reservation_id"])

def create_shipment(context):
    result = shipping_service.create(context["order_id"], context["address"])
    if not result["success"]:
        raise Exception("Shipping unavailable")
    context["shipment_id"] = result["shipment_id"]

def cancel_shipment(context):
    shipping_service.cancel(context["shipment_id"])

# Build saga
order_saga = OrderSaga()
order_saga.add_step(create_order, cancel_order)
order_saga.add_step(charge_payment, refund_payment)
order_saga.add_step(reserve_inventory, release_inventory)
order_saga.add_step(create_shipment, cancel_shipment)

# Execute
context = {
    "user_id": 123,
    "product_id": 456,
    "quantity": 2,
    "amount": 99.99,
    "address": "123 Main St"
}

result = order_saga.execute(context)
print(f"Result: {result}")
```

**Interview talking points**:
- No distributed transactions (ACID) across microservices
- Saga = Sequence of local transactions + compensations
- Choreography (event-driven) vs Orchestration (coordinator service)
- Eventual consistency (order might be "pending" for seconds)

---

**End of Code-Level Patterns** 💻

---

# 🚪 API Gateway Patterns

> **Every microservices interview asks: "How do clients talk to 50+ services?"** - Answer: API Gateway

## What is an API Gateway?

**Single entry point** for all client requests. Routes to appropriate backend services.

```
┌──────────┐
│ Mobile   │ ─┐
│ App      │  │
└──────────┘  │
              │    ┌─────────────────┐         ┌──────────────┐
┌──────────┐  ├───▶│   API Gateway   │────────▶│ Auth Service │
│ Web App  │  │    │                 │         └──────────────┘
└──────────┘  │    │ - Routing       │         ┌──────────────┐
              │    │ - Auth          │────────▶│ User Service │
┌──────────┐  │    │ - Rate Limit    │         └──────────────┘
│ Partner  │  │    │ - Load Balance  │         ┌──────────────┐
│ API      │ ─┘    │ - Logging       │────────▶│ Order Service│
└──────────┘       └─────────────────┘         └──────────────┘
                                                ┌──────────────┐
                                                │ Payment Svc  │
                                                └──────────────┘
```

---

## Core Responsibilities

### 1. Request Routing

```nginx
# NGINX configuration example
server {
    listen 80;
    server_name api.example.com;
    
    # User service
    location /api/v1/users {
        proxy_pass http://user-service:8001;
    }
    
    # Order service
    location /api/v1/orders {
        proxy_pass http://order-service:8002;
    }
    
    # Payment service
    location /api/v1/payments {
        proxy_pass http://payment-service:8003;
    }
}
```

### 2. Authentication & Authorization

```python
# API Gateway validates JWT before forwarding
from fastapi import FastAPI, Header, HTTPException
import jwt
import requests

app = FastAPI()

SECRET_KEY = "your-secret-key"

@app.middleware("http")
async def auth_middleware(request, call_next):
    """Validate JWT token for all requests"""
    auth_header = request.headers.get("Authorization")
    
    if not auth_header or not auth_header.startswith("Bearer "):
        raise HTTPException(status_code=401, detail="Missing or invalid token")
    
    token = auth_header.split(" ")[1]
    
    try:
        # Decode and validate JWT
        payload = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
        
        # Inject user info into request headers (for downstream services)
        request.state.user_id = payload["user_id"]
        request.state.role = payload.get("role", "user")
        
    except jwt.ExpiredSignatureError:
        raise HTTPException(status_code=401, detail="Token expired")
    except jwt.InvalidTokenError:
        raise HTTPException(status_code=401, detail="Invalid token")
    
    response = await call_next(request)
    return response

@app.get("/api/v1/orders")
async def get_orders(request: Request):
    """Forward to order service with user context"""
    user_id = request.state.user_id
    
    # Forward request to order service
    response = requests.get(
        "http://order-service:8002/orders",
        headers={"X-User-ID": str(user_id)}
    )
    
    return response.json()
```

### 3. Rate Limiting (Protect Backend Services)

```python
# API Gateway enforces rate limits
from fastapi import FastAPI, HTTPException
import redis

app = FastAPI()
r = redis.Redis(host='localhost', port=6379)

@app.middleware("http")
async def rate_limit_middleware(request, call_next):
    """Rate limit: 100 requests per minute per IP"""
    client_ip = request.client.host
    key = f"rate_limit:{client_ip}"
    
    # Increment request count
    count = r.incr(key)
    
    if count == 1:
        # First request, set expiry to 60 seconds
        r.expire(key, 60)
    
    if count > 100:
        raise HTTPException(
            status_code=429,
            detail="Rate limit exceeded. Try again in 60 seconds.",
            headers={"Retry-After": "60"}
        )
    
    response = await call_next(request)
    response.headers["X-RateLimit-Remaining"] = str(100 - count)
    return response
```

### 4. Request/Response Transformation

```python
# API Gateway transforms external API to internal format
@app.post("/api/v1/orders")
async def create_order(external_request: dict):
    """
    External API: {"product": "iPhone", "qty": 2}
    Internal API: {"product_id": 123, "quantity": 2, "user_id": 456}
    """
    
    # Transform request
    internal_request = {
        "product_id": PRODUCT_MAPPING.get(external_request["product"]),
        "quantity": external_request["qty"],
        "user_id": request.state.user_id  # From JWT
    }
    
    # Call internal service
    response = requests.post(
        "http://order-service:8002/internal/orders",
        json=internal_request
    )
    
    # Transform response
    internal_response = response.json()
    external_response = {
        "orderId": internal_response["order_id"],
        "status": internal_response["status"],
        "estimatedDelivery": internal_response["estimated_delivery_date"]
    }
    
    return external_response
```

### 5. Protocol Translation

```python
# API Gateway: REST (external) → gRPC (internal)
import grpc
from order_service_pb2 import OrderRequest
from order_service_pb2_grpc import OrderServiceStub

@app.get("/api/v1/orders/{order_id}")
async def get_order(order_id: int):
    """REST API for clients, gRPC to backend"""
    
    # Create gRPC channel
    channel = grpc.insecure_channel('order-service:50051')
    stub = OrderServiceStub(channel)
    
    # Call gRPC service
    request = OrderRequest(order_id=order_id)
    response = stub.GetOrder(request)
    
    # Convert gRPC response to REST JSON
    return {
        "orderId": response.order_id,
        "status": response.status,
        "items": [{"productId": item.product_id, "quantity": item.quantity} 
                  for item in response.items]
    }
```

---

## Backend for Frontend (BFF) Pattern

**Problem**: Mobile needs different data than web (mobile = low bandwidth)

**Solution**: Separate API Gateway per client type

```
┌─────────────┐
│ Mobile BFF  │ ─┐
│             │  │   (Optimized for mobile)
│ - Small     │  │   GET /mobile/feed
│   payloads  │  │   → Returns compressed data
│ - Image     │  │   → 480p images, not 4K
│   resizing  │  │
└─────────────┘  │
                 │         ┌──────────────┐
                 ├────────▶│ User Service │
                 │         └──────────────┘
┌─────────────┐  │         ┌──────────────┐
│   Web BFF   │  ├────────▶│ Feed Service │
│             │  │         └──────────────┘
│ - Rich data │  │         ┌──────────────┐
│ - Multiple  │  ├────────▶│ Media Service│
│   calls     │  │         └──────────────┘
└─────────────┘  │
                 │
┌─────────────┐  │
│ Partner BFF │ ─┘
│             │
│ - Batching  │   (Optimized for B2B partners)
│ - Webhooks  │   POST /partner/bulk-orders
└─────────────┘   → Upload CSV, get webhook callback
```

**Code example**:

```python
# Mobile BFF: Optimized for low bandwidth
@mobile_bff.get("/mobile/feed")
async def get_mobile_feed(user_id: int):
    """Lightweight feed for mobile"""
    posts = await feed_service.get_posts(user_id, limit=20)
    
    # Only return essential fields
    return [
        {
            "id": post.id,
            "text": post.text[:200],  # Truncate
            "image": resize_image(post.image_url, width=480),  # 480p
            "likes": post.like_count  # Just count, not list of users
        }
        for post in posts
    ]

# Web BFF: Rich data for web
@web_bff.get("/web/feed")
async def get_web_feed(user_id: int):
    """Full-featured feed for web"""
    posts = await feed_service.get_posts(user_id, limit=50)
    
    # Fetch additional data (parallel calls)
    user_ids = [post.author_id for post in posts]
    users = await user_service.get_users_batch(user_ids)
    
    return [
        {
            "id": post.id,
            "text": post.text,  # Full text
            "image": post.image_url,  # 4K image
            "author": {
                "id": users[post.author_id].id,
                "name": users[post.author_id].name,
                "avatar": users[post.author_id].avatar
            },
            "likes": await like_service.get_like_details(post.id),  # Full list
            "comments": await comment_service.get_comments(post.id, limit=5)
        }
        for post in posts
    ]
```

---

## API Gateway Technologies

| Technology | Best For | Scale | Notes |
|-----------|---------|-------|-------|
| **NGINX** | Simple routing, static config | 100K+ RPS | Lightweight, battle-tested |
| **Kong** | Plugins (auth, rate limit, logging) | 50K+ RPS | Built on NGINX, Lua plugins |
| **AWS API Gateway** | Serverless, AWS ecosystem | Auto-scale | Pay-per-request, managed |
| **Envoy** | Service mesh, dynamic routing | 100K+ RPS | Used by Istio, gRPC native |
| **Zuul (Netflix)** | Java microservices, filters | 10K+ RPS | JVM-based, Netflix OSS |
| **Traefik** | Kubernetes, Docker, auto-discovery | 50K+ RPS | Cloud-native, dynamic config |

---

## API Gateway vs Service Mesh

**API Gateway**: **External** traffic (clients → backend)

**Service Mesh**: **Internal** traffic (service → service)

```
┌──────────┐
│ Client   │
└────┬─────┘
     │
     ▼
┌─────────────────┐  ◄─── API Gateway (external)
│   API Gateway   │       - Authentication
└────┬────────────┘       - Rate limiting
     │                    - SSL termination
     ▼
┌─────────────────┐
│  Service Mesh   │  ◄─── Service Mesh (internal)
│  (Istio/Envoy)  │       - Service discovery
│                 │       - Load balancing
│  ┌───┐  ┌───┐  │       - Circuit breaking
│  │ A │─▶│ B │  │       - Observability
│  └───┘  └───┘  │
│    │            │
│    ▼            │
│  ┌───┐          │
│  │ C │          │
│  └───┘          │
└─────────────────┘
```

**When do you need both?**
- API Gateway: If you have external clients (mobile, web, partners)
- Service Mesh: If you have 10+ microservices talking to each other

---

## Interview Answer Template

**Q: "How would you design an API Gateway for a ride-sharing app?"**

**Answer framework**:

1. **Requirements**:
   - Handle 100K RPS (peak)
   - 3 client types: Mobile (iOS/Android), Web, Driver App
   - Must authenticate every request
   - Rate limit: 1000 req/min per user

2. **Technology choice**:
   - Use **Kong** (NGINX + plugins)
   - Why? Built-in rate limiting, JWT auth, observability
   - Alternative: AWS API Gateway (if serverless)

3. **Architecture**:
```
Mobile App ─┐
Web App ────┼───▶ Kong API Gateway ───┐
Driver App ─┘     (3 replicas)        │
                  - JWT validation    │
                  - Rate limiting     ├───▶ User Service
                  - Request logging   │
                                      ├───▶ Ride Service
                                      │
                                      ├───▶ Payment Service
                                      │
                                      └───▶ Notification Service
```

4. **Key decisions**:
   - **BFF pattern**: Separate `/mobile/*` and `/web/*` endpoints
   - **Rate limiting**: Redis-backed (shared state across gateway replicas)
   - **Auth**: Validate JWT at gateway, inject user_id header for downstream
   - **Caching**: Cache user profile at gateway (reduce Auth Service load)

5. **Scaling**:
   - Horizontal: 3 replicas behind load balancer (handle 100K RPS)
   - Rate limiting: Redis cluster (failover for high availability)
   - Monitoring: Prometheus metrics (latency, error rate, RPS per endpoint)

---

# 🔍 Distributed Tracing

> **Interviewer asks: "Request is slow, how do you debug across 10 microservices?"** - Answer: Distributed Tracing

## The Problem

```
User → API Gateway → Service A → Service B → Service C → Database
                         │
                         └────▶ Service D → Cache

Which service is slow? 🤔
```

## The Solution: Trace Every Request

**Distributed tracing** = Follow a single request across all services

```
Trace ID: abc123
│
├─ Span: API Gateway (10ms)
│   │
│   ├─ Span: Service A (50ms)
│   │   │
│   │   ├─ Span: Service B (30ms)
│   │   │   └─ Span: Database query (25ms) ◄─── Bottleneck!
│   │   │
│   │   └─ Span: Service D (5ms)
│   │       └─ Span: Cache lookup (2ms)
│
Total: 60ms
```

---

## OpenTelemetry Example

```python
# Service A: Create a trace
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
from opentelemetry.exporter.jaeger import JaegerExporter
import requests

# Setup tracing
trace.set_tracer_provider(TracerProvider())
jaeger_exporter = JaegerExporter(
    agent_host_name="localhost",
    agent_port=6831,
)
trace.get_tracer_provider().add_span_processor(
    BatchSpanProcessor(jaeger_exporter)
)

tracer = trace.get_tracer(__name__)

# Instrument your code
@app.get("/api/orders/{order_id}")
async def get_order(order_id: int):
    # Start a span for this operation
    with tracer.start_as_current_span("get_order") as span:
        span.set_attribute("order_id", order_id)
        
        # Call Service B (automatically creates child span)
        with tracer.start_as_current_span("call_user_service"):
            user_data = requests.get(
                f"http://user-service:8001/users/{user_id}",
                headers={"X-Trace-ID": span.get_span_context().trace_id}
            )
        
        # Call database
        with tracer.start_as_current_span("database_query"):
            order = db.query("SELECT * FROM orders WHERE id = ?", order_id)
        
        return {"order": order, "user": user_data}
```

## What Gets Logged

```json
{
  "traceId": "abc123",
  "spanId": "span1",
  "operationName": "get_order",
  "startTime": 1700000000000,
  "duration": 60,
  "tags": {
    "order_id": 12345,
    "http.method": "GET",
    "http.status_code": 200
  },
  "logs": [
    {"timestamp": 1700000000010, "message": "Fetching user data"},
    {"timestamp": 1700000000040, "message": "User data retrieved"}
  ],
  "references": [
    {"refType": "CHILD_OF", "traceId": "abc123", "spanId": "span0"}
  ]
}
```

---

## Trace Propagation (Pass Context Between Services)

```python
# Service A: Inject trace context into HTTP headers
import requests
from opentelemetry.propagate import inject

headers = {}
inject(headers)  # Adds traceparent header

response = requests.get(
    "http://service-b:8002/api/data",
    headers=headers  # Propagates trace context
)

# Service B: Extract trace context from headers
from opentelemetry.propagate import extract

@app.get("/api/data")
async def get_data(request: Request):
    # Extract trace context from incoming request
    context = extract(request.headers)
    
    # Create child span (linked to parent trace)
    with tracer.start_as_current_span("get_data", context=context):
        # Your code here
        data = fetch_data()
        return data
```

---

## Sampling Strategies

**Problem**: Tracing 1M requests/sec = too much data

**Solution**: Sample intelligently

| Strategy | When to Use | Example |
|---------|-------------|---------|
| **Always On** | Dev/staging | Trace 100% of requests |
| **Probabilistic** | Production (high traffic) | Trace 1% of requests |
| **Rate Limiting** | Production (medium traffic) | Trace 100 req/sec |
| **Error-based** | Production (debugging) | Trace all errors + 1% success |
| **Tail-based** | Advanced | Trace slow requests (>1s) + errors |

```python
# Probabilistic sampling: 1% of requests
from opentelemetry.sdk.trace.sampling import TraceIdRatioBased

sampler = TraceIdRatioBased(0.01)  # 1% sampling
tracer_provider = TracerProvider(sampler=sampler)
```

---

## Tracing Technologies

| Technology | Best For | Notes |
|-----------|---------|-------|
| **Jaeger** | Self-hosted, Kubernetes | CNCF project, Cassandra/Elasticsearch backend |
| **Zipkin** | Self-hosted, simple setup | Twitter OSS, in-memory or MySQL backend |
| **AWS X-Ray** | AWS services | Managed, integrates with Lambda, ECS, etc. |
| **Google Cloud Trace** | GCP services | Managed, integrates with GKE, Cloud Run |
| **Datadog APM** | SaaS, full observability | Paid, includes metrics + logs + traces |
| **New Relic** | SaaS, easy setup | Paid, automatic instrumentation |

---

## Interview Answer Template

**Q: "How would you debug a slow request across microservices?"**

**Answer**:

1. **Use distributed tracing** (Jaeger or AWS X-Ray)
   - Assign unique Trace ID to each request
   - Propagate Trace ID via HTTP headers (traceparent)
   - Each service creates spans (operation + timing)

2. **Visualize the trace** (Jaeger UI shows waterfall):
```
GET /api/orders/123 (120ms total)
├─ API Gateway (5ms)
├─ Auth Service (10ms)
├─ Order Service (100ms) ◄─── Slow!
│  ├─ Database query (95ms) ◄─── Bottleneck!
│  └─ Cache lookup (5ms)
└─ User Service (5ms)
```

3. **Drill down**:
   - Database query slow → Check EXPLAIN plan (missing index?)
   - Service B slow → Check metrics (CPU/memory maxed out?)
   - Network slow → Check service mesh metrics (retries? timeouts?)

4. **Sampling**: Production traces 1% of requests (too expensive to trace all)

5. **Correlation**: Link traces with logs (same Trace ID in log messages)

---

**End of API Gateway & Distributed Tracing** 🚪🔍

---

# 🗄️ Database Deep-Dive for Interviews

> **Most system design questions involve database decisions** - Master this section!

## Database Indexing (Often Poorly Understood!)

### B-Tree Index (Default in Most Databases)

**How it works**: Balanced tree, O(log n) lookups

```
Index on user_id:
              [50]
            /      \
        [25]        [75]
       /    \      /    \
    [10]  [40]  [60]  [90]
     ↓     ↓     ↓     ↓
  (row1)(row2)(row3)(row4)
```

**Good for**:
- Range queries: `WHERE age BETWEEN 25 AND 35`
- Sorting: `ORDER BY created_at DESC`
- Equality: `WHERE user_id = 12345`

**Example**:
```sql
-- Slow: Full table scan (1 million rows)
SELECT * FROM users WHERE email = 'john@example.com';
-- Execution time: 500ms

-- Fast: Index scan
CREATE INDEX idx_email ON users(email);
SELECT * FROM users WHERE email = 'john@example.com';
-- Execution time: 2ms (250x faster!)
```

---

### Composite Index (Multi-Column)

**Order matters!**

```sql
-- Index on (country, city, age)
CREATE INDEX idx_location_age ON users(country, city, age);

-- ✅ Uses index (leftmost prefix)
SELECT * FROM users WHERE country = 'USA';
SELECT * FROM users WHERE country = 'USA' AND city = 'NYC';
SELECT * FROM users WHERE country = 'USA' AND city = 'NYC' AND age > 25;

-- ❌ Does NOT use index (missing leftmost column)
SELECT * FROM users WHERE city = 'NYC';
SELECT * FROM users WHERE age > 25;
```

**Rule**: Index can be used if query filters on **leftmost columns**

**Interview example**:
```sql
-- Query: Find active users in NYC created in 2024
SELECT * FROM users 
WHERE city = 'NYC' 
  AND status = 'active' 
  AND created_at >= '2024-01-01';

-- Option 1: Index on (city, status, created_at) ✅ Best
-- Option 2: Index on (status, city, created_at) ❌ Wrong order
-- Option 3: Three separate indexes ❌ Query uses only one
```

---

### Covering Index (Index-Only Scan)

**Definition**: Index contains all columns needed by query (no table lookup!)

```sql
-- Query: Get user names and emails
SELECT user_id, name, email FROM users WHERE country = 'USA';

-- Regular index: idx_country → Fetch rows from table (slow)
CREATE INDEX idx_country ON users(country);

-- Covering index: All columns in index → No table lookup (fast!)
CREATE INDEX idx_country_covering ON users(country, user_id, name, email);
```

**Performance**:
- Regular index: 100ms (index lookup + table lookup)
- Covering index: 10ms (index lookup only) → **10x faster!**

---

### When Indexes HURT Performance

**Problem**: Indexes slow down writes (INSERT/UPDATE/DELETE)

```sql
-- Table with 5 indexes
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(255),
    name VARCHAR(255),
    country VARCHAR(50),
    city VARCHAR(50),
    age INT
);

CREATE INDEX idx_email ON users(email);
CREATE INDEX idx_name ON users(name);
CREATE INDEX idx_country ON users(country);
CREATE INDEX idx_city ON users(city);
CREATE INDEX idx_age ON users(age);

-- Every INSERT updates 6 data structures (table + 5 indexes)!
INSERT INTO users VALUES (1, 'john@example.com', 'John', 'USA', 'NYC', 30);
-- Execution time: 50ms (vs 5ms with no indexes)
```

**Interview answer**:
- **Read-heavy**: Add more indexes (optimize SELECTs)
- **Write-heavy**: Minimize indexes (optimize INSERTs)
- **Common pattern**: Index foreign keys, unique columns, query filters (not everything!)

---

## Database Replication Strategies

### 1. Single-Leader Replication (Primary-Secondary)

**How it works**:
```
        ┌──────────────┐
        │  PRIMARY DB  │ ◄─── All writes go here
        │  (Read/Write)│
        └──────┬───────┘
               │
        Replication log
               │
       ┌───────┴────────┐
       ▼                ▼
┌─────────────┐  ┌─────────────┐
│  REPLICA 1  │  │  REPLICA 2  │ ◄─── Read-only
│  (Read-Only)│  │  (Read-Only)│
└─────────────┘  └─────────────┘
```

**Pros**:
- Simple to understand
- Strong consistency on primary
- Read scaling (add more replicas)

**Cons**:
- Replication lag (replica might be seconds behind)
- Primary is single point of failure

**Interview scenario**:
```
User posts a comment:
1. App writes to PRIMARY (user_id=123, comment="Hello")
2. App redirects to profile page
3. App reads from REPLICA (might not have new comment yet!) ❌

Solution: "Read-your-writes" consistency
→ After write, read from PRIMARY for 1 second
→ Then switch to REPLICA
```

---

### 2. Multi-Leader Replication (Master-Master)

**How it works**: Multiple databases accept writes

```
┌──────────────┐     ┌──────────────┐
│  LEADER 1    │────▶│  LEADER 2    │
│  (US East)   │◀────│  (EU West)   │
└──────────────┘     └──────────────┘
     Write to            Write to
     Leader 1            Leader 2
        ▲                    ▲
        │                    │
    US users            European users
```

**Pros**:
- Low latency (write to nearest leader)
- High availability (both leaders can accept writes)

**Cons**:
- **Conflict resolution** (two users edit same row simultaneously)

**Conflict example**:
```
User A (US): UPDATE users SET name = 'Alice' WHERE id = 1;
User B (EU): UPDATE users SET name = 'Anna' WHERE id = 1;

Which name wins? 🤔

Conflict resolution strategies:
1. Last-write-wins (timestamp) → "Anna" (if EU update is newer)
2. Application-defined logic → Merge: "Alice/Anna"
3. Prompt user to resolve → Show both versions, ask user to choose
```

---

### 3. Leaderless Replication (Quorum-Based)

**How it works**: Write to N replicas, read from N replicas, use voting

```
┌─────────────┐
│  Client     │
└──────┬──────┘
       │
   Write "X"
       │
┌──────┼──────┬──────┐
▼      ▼      ▼      ▼
┌───┐  ┌───┐  ┌───┐  ┌───┐
│ A │  │ B │  │ C │  │ D │  (4 replicas)
└───┘  └───┘  └───┘  └───┘
 "X"    "X"    "X"   (old)  ← One replica failed!

Read: Query 3 replicas
 A="X", B="X", D=(old) → Return "X" (majority vote)
```

**Quorum formula**: W + R > N
- N = Total replicas (e.g., 3)
- W = Write quorum (e.g., 2) → Write succeeds if 2 replicas confirm
- R = Read quorum (e.g., 2) → Read from 2 replicas, take majority

**Example: Cassandra**
```cql
-- Write with quorum (2 out of 3 replicas)
INSERT INTO users (id, name) VALUES (1, 'Alice')
WITH CONSISTENCY QUORUM;

-- Read with quorum
SELECT * FROM users WHERE id = 1
WITH CONSISTENCY QUORUM;
```

**Interview talking points**:
- **No leader** = No single point of failure
- **Eventual consistency** = Replicas might disagree temporarily
- **Used by**: Cassandra, DynamoDB, Riak

---

## Database Sharding (Horizontal Partitioning)

### When to Shard?

❌ **DON'T shard if**:
- < 1TB data (vertical scaling + read replicas work fine)
- < 10K QPS (single database can handle this)

✅ **DO shard if**:
- > 10TB data (single database can't store it)
- > 100K QPS (single database CPU/memory maxed out)
- Hot partitions (some users generate 100x more traffic)

---

### Sharding Strategy 1: Hash-Based

**Partition by hash of user_id**

```
user_id → hash(user_id) % 4 → shard_number

user_id=123 → hash=7 → 7 % 4 = 3 → Shard 3
user_id=456 → hash=2 → 2 % 4 = 2 → Shard 2

┌─────────────┐
│   Shard 0   │  user_id ∈ {4, 8, 12, ...}
├─────────────┤
│   Shard 1   │  user_id ∈ {1, 5, 9, ...}
├─────────────┤
│   Shard 2   │  user_id ∈ {2, 6, 10, ...}
├─────────────┤
│   Shard 3   │  user_id ∈ {3, 7, 11, ...}
└─────────────┘
```

**Pros**:
- Even data distribution
- Simple to implement

**Cons**:
- Resharding is hard (if you add Shard 4, all keys need rehashing!)
- Range queries don't work (`WHERE user_id BETWEEN 100 AND 200` spans all shards)

**Solution for resharding**: Consistent hashing (see Code Patterns section)

---

### Sharding Strategy 2: Range-Based

**Partition by ranges of user_id**

```
┌─────────────────────┐
│   Shard 0           │  user_id: 1 - 1,000,000
├─────────────────────┤
│   Shard 1           │  user_id: 1,000,001 - 2,000,000
├─────────────────────┤
│   Shard 2           │  user_id: 2,000,001 - 3,000,000
└─────────────────────┘
```

**Pros**:
- Range queries work (`WHERE user_id BETWEEN 1M AND 2M` → Query Shard 1 only)
- Easy to add new shards (just add Shard 3 for IDs 3M-4M)

**Cons**:
- Uneven distribution (recent users more active → Shard 2 overloaded)

**Example**: Time-series data (logs, events)
```
┌─────────────────────┐
│   Shard 0           │  2024-01-01 to 2024-01-31
├─────────────────────┤
│   Shard 1           │  2024-02-01 to 2024-02-29
├─────────────────────┤
│   Shard 2 (HOT!)    │  2024-03-01 to 2024-03-31 ◄─── Most queries here
└─────────────────────┘
```

---

### Sharding Strategy 3: Directory-Based (Lookup Table)

**Use a lookup service to find which shard has the data**

```
┌──────────────────┐
│ Routing Service  │  (user_id → shard mapping)
│                  │
│ user_id | shard  │
│ --------|-----   │
│   123   |   0    │
│   456   |   2    │
│   789   |   1    │
└──────────────────┘
         │
    ┌────┼────┬────┐
    ▼    ▼    ▼    ▼
  Shard0 Shard1 Shard2
```

**Pros**:
- Flexible (can move user 123 from Shard 0 → Shard 1 anytime)
- Solves hot partition problem (move celebrity users to dedicated shard)

**Cons**:
- Routing service is single point of failure
- Extra network hop (latency)

**Interview example: Instagram**
```
Normal users → Sharded by hash(user_id)
Celebrities (Kim Kardashian, 300M followers) → Dedicated shard
```

---

### Cross-Shard Queries (The Hard Problem!)

**Problem**: Query spans multiple shards

```sql
-- "Get all users named John" → Must query ALL shards! ❌
SELECT * FROM users WHERE name = 'John';

-- "Get posts for user_id=123" → Query single shard ✅
SELECT * FROM posts WHERE user_id = 123;
```

**Solution strategies**:
1. **Denormalize**: Store redundant data to avoid cross-shard queries
2. **Application-level joins**: Query each shard, merge results in app
3. **AVOID sharding on this table**: Not all tables need sharding!

---

## Replication + Sharding Combined

**Most production systems use BOTH**:

```
┌────────────────────────────────┐
│         Shard 0                │
│  ┌──────────┐   ┌──────────┐  │
│  │ PRIMARY  │──▶│ REPLICA  │  │
│  └──────────┘   └──────────┘  │
└────────────────────────────────┘

┌────────────────────────────────┐
│         Shard 1                │
│  ┌──────────┐   ┌──────────┐  │
│  │ PRIMARY  │──▶│ REPLICA  │  │
│  └──────────┘   └──────────┘  │
└────────────────────────────────┘

┌────────────────────────────────┐
│         Shard 2                │
│  ┌──────────┐   ┌──────────┐  │
│  │ PRIMARY  │──▶│ REPLICA  │  │
│  └──────────┘   └──────────┘  │
└────────────────────────────────┘
```

**Benefits**:
- **Sharding**: Scale writes (distribute across shards)
- **Replication**: Scale reads (read from replicas) + high availability

**Example: Instagram**
- 1,000 shards (each shard = 1 primary + 2 replicas)
- 3,000 PostgreSQL databases total!

---

## SQL vs NoSQL Decision Tree (For Interviews)

```
Start
  │
  ▼
Need ACID transactions?
(e.g., banking, payments)
  │
  ├─ YES → Use SQL (PostgreSQL, MySQL)
  │
  ▼ NO
Need complex joins?
(e.g., social graph, e-commerce)
  │
  ├─ YES → Use SQL
  │
  ▼ NO
Need >100K QPS or >10TB data?
  │
  ├─ NO → Use SQL (default choice!)
  │
  ▼ YES
Data model is simple (key-value or document)?
  │
  ├─ YES → Use NoSQL
  │        └─ Document: MongoDB, DynamoDB
  │        └─ Key-Value: Redis, DynamoDB
  │        └─ Wide-Column: Cassandra, HBase
  │
  ▼ NO
Use SQL + sharding (Instagram, Uber, Twitter all use sharded PostgreSQL!)
```

---

## Interview Answer Template

**Q: "Instagram has 2 billion users. How do you store user data?"**

**Answer**:

1. **Start with PostgreSQL** (SQL):
   - ACID transactions (ensure data consistency)
   - Schema validation (enforce user data structure)
   - Battle-tested (Instagram uses sharded PostgreSQL!)

2. **Vertical scaling first** (delay sharding):
   - Scale up to 96-core, 512GB RAM machine
   - Add read replicas (5 replicas for read scaling)
   - Use connection pooling (PgBouncer)
   - Result: Can handle 10M users, 10K QPS

3. **Shard when needed** (>100M users):
   - Shard by `hash(user_id) % 1000` → 1,000 shards
   - Each shard: 2M users, 100GB data
   - Each shard: 1 primary + 2 replicas (high availability)

4. **Optimize**:
   - **Indexes**: `user_id` (primary key), `email` (login), `username` (profile lookup)
   - **Caching**: Redis cache hot users (top 1% = 20M users cached)
   - **CDN**: Profile pictures on CloudFront (not in database!)

5. **Handling hot partitions**:
   - Celebrities → Dedicated shard (avoid overloading one shard)
   - Use directory-based sharding for flexibility

---

**End of Database Deep-Dive** 🗄️

---

# 📝 More System Design Interview Questions

> **Complete walkthroughs for 10 common questions**

---

## 1. Design Dropbox / Google Drive

**Requirements**:
- Upload/download files (any size, up to 10GB)
- Sync across devices (desktop, mobile, web)
- Share files with other users
- Version history (restore previous versions)
- Scale: 500M users, 100M DAU, 10PB total storage

---

### High-Level Architecture

```
┌────────────┐
│   Client   │ (Desktop/Mobile/Web)
└──────┬─────┘
       │
       ▼
┌─────────────────┐
│  API Gateway    │ (Upload/Download/Sync endpoints)
└────────┬────────┘
         │
    ┌────┼────┬─────────┐
    ▼    ▼    ▼         ▼
┌───────┐ ┌────────┐ ┌──────────┐
│ Auth  │ │ Meta   │ │ Sync     │
│ Svc   │ │ Svc    │ │ Service  │
└───────┘ └───┬────┘ └────┬─────┘
              │           │
              ▼           ▼
         ┌──────────┐  ┌─────────────┐
         │ MySQL    │  │ Message     │
         │ (Metadata)│  │ Queue       │
         └──────────┘  └─────────────┘

         ┌──────────────────────┐
         │  Blob Storage (S3)   │ (Actual files)
         └──────────────────────┘
```

---

### API Design

```
POST   /api/v1/files/upload
GET    /api/v1/files/{file_id}/download
GET    /api/v1/files/{file_id}/metadata
POST   /api/v1/files/{file_id}/share
GET    /api/v1/sync/changes?since=timestamp
```

---

### Data Model

**MySQL (Metadata)**:
```sql
CREATE TABLE users (
    user_id BIGINT PRIMARY KEY,
    email VARCHAR(255) UNIQUE,
    storage_used BIGINT,  -- Bytes
    storage_quota BIGINT  -- 15GB free, 2TB paid
);

CREATE TABLE files (
    file_id VARCHAR(64) PRIMARY KEY,  -- UUID
    user_id BIGINT,
    filename VARCHAR(255),
    file_size BIGINT,
    mime_type VARCHAR(100),
    s3_key VARCHAR(512),  -- s3://bucket/user_123/file.pdf
    version INT,  -- Increments on update
    is_deleted BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP,
    updated_at TIMESTAMP,
    INDEX idx_user_id (user_id),
    INDEX idx_updated_at (updated_at)  -- For sync
);

CREATE TABLE file_versions (
    version_id BIGINT PRIMARY KEY,
    file_id VARCHAR(64),
    version INT,
    s3_key VARCHAR(512),  -- s3://bucket/user_123/file_v2.pdf
    file_size BIGINT,
    created_at TIMESTAMP,
    INDEX idx_file_id (file_id)
);

CREATE TABLE file_shares (
    share_id BIGINT PRIMARY KEY,
    file_id VARCHAR(64),
    owner_user_id BIGINT,
    shared_with_user_id BIGINT,
    permission ENUM('read', 'write'),
    created_at TIMESTAMP
);
```

---

### Key Design Decisions

#### 1. File Chunking (Large File Upload)

**Problem**: Uploading 10GB file in one request = timeout, no resume

**Solution**: Split file into 4MB chunks

```python
def upload_file(file_path):
    """Upload large file in chunks"""
    file_id = generate_uuid()
    chunk_size = 4 * 1024 * 1024  # 4MB
    
    with open(file_path, 'rb') as f:
        chunk_number = 0
        while True:
            chunk = f.read(chunk_size)
            if not chunk:
                break
            
            # Upload chunk to S3
            s3_key = f"uploads/{file_id}/chunk_{chunk_number}"
            s3.put_object(Bucket='dropbox-storage', Key=s3_key, Body=chunk)
            
            chunk_number += 1
    
    # Notify server: all chunks uploaded
    response = requests.post(
        '/api/v1/files/complete_upload',
        json={
            'file_id': file_id,
            'filename': 'document.pdf',
            'total_chunks': chunk_number
        }
    )
    
    return response.json()
```

**Benefits**:
- Resume upload (if chunk 5 fails, restart from chunk 5)
- Parallel uploads (upload chunks simultaneously)
- Better progress bar (show 65% uploaded)

---

#### 2. File Synchronization (Avoid Re-Downloading Everything)

**Problem**: User has 1000 files. Desktop app starts → should it download all 1000 files?

**Solution**: Delta sync (only download changes since last sync)

```python
# Client stores last sync timestamp
last_sync_time = "2024-03-15T10:30:00Z"

# Ask server: what changed since then?
response = requests.get(f'/api/v1/sync/changes?since={last_sync_time}')

changes = response.json()
# {
#   "files": [
#     {"file_id": "abc", "action": "modified", "version": 3},
#     {"file_id": "xyz", "action": "deleted"},
#     {"file_id": "new", "action": "created", "filename": "photo.jpg"}
#   ]
# }

# Download only modified/created files
for change in changes['files']:
    if change['action'] == 'created' or change['action'] == 'modified':
        download_file(change['file_id'])
    elif change['action'] == 'deleted':
        delete_local_file(change['file_id'])

# Update last sync timestamp
last_sync_time = now()
```

**Server implementation**:
```python
@app.get('/api/v1/sync/changes')
def get_changes(user_id: int, since: datetime):
    """Return files changed since timestamp"""
    changes = db.query(
        """
        SELECT file_id, filename, version, updated_at, is_deleted
        FROM files
        WHERE user_id = ? AND updated_at > ?
        ORDER BY updated_at ASC
        """,
        (user_id, since)
    )
    
    return {
        "files": [
            {
                "file_id": row.file_id,
                "action": "deleted" if row.is_deleted else "modified",
                "version": row.version,
                "filename": row.filename
            }
            for row in changes
        ]
    }
```

---

#### 3. Conflict Resolution (Two Devices Edit Same File)

**Scenario**:
```
Device A (offline): Edits document.txt → version 3
Device B (offline): Edits document.txt → version 3
Both sync to server → Conflict! 🤔
```

**Solution 1: Last-Write-Wins** (Simple but lossy)
```python
# Server keeps latest version by timestamp
if file.version == incoming_version:
    if incoming_timestamp > file.updated_at:
        # Incoming version is newer, overwrite
        update_file(file_id, incoming_data)
    else:
        # Existing version is newer, reject
        return {"error": "Conflict: File was modified by another device"}
```

**Solution 2: Create Conflict Copy** (Dropbox approach)
```python
# Keep both versions
original_file = "document.txt"  # Version from Device A
conflict_file = "document (Device B's conflicted copy).txt"

# User manually merges later
```

---

#### 4. Storage Optimization (Deduplication)

**Problem**: 1000 users upload same "company_logo.png" → Store 1000 copies? Wasteful!

**Solution**: Content-based addressing (hash file content)

```python
import hashlib

def upload_file(file_data, filename):
    # Calculate SHA-256 hash of file content
    file_hash = hashlib.sha256(file_data).hexdigest()
    
    # Check if file with same hash already exists
    existing_file = db.query(
        "SELECT s3_key FROM files WHERE content_hash = ?",
        (file_hash,)
    )
    
    if existing_file:
        # File already exists! Just create metadata entry (no S3 upload)
        db.execute(
            "INSERT INTO files (user_id, filename, s3_key, content_hash) VALUES (?, ?, ?, ?)",
            (user_id, filename, existing_file.s3_key, file_hash)
        )
        return {"status": "deduplicated"}
    else:
        # New file, upload to S3
        s3_key = f"files/{file_hash[:2]}/{file_hash}"
        s3.put_object(Bucket='dropbox-storage', Key=s3_key, Body=file_data)
        
        db.execute(
            "INSERT INTO files (user_id, filename, s3_key, content_hash) VALUES (?, ?, ?, ?)",
            (user_id, filename, s3_key, file_hash)
        )
        return {"status": "uploaded"}
```

**Savings**: If 10K users upload same 10MB file → Store 10MB once (not 100GB!)

---

### Capacity Estimation

```
Users: 500M total, 100M DAU
Average storage per user: 1GB
Total storage: 500M × 1GB = 500PB

Daily uploads: 100M users × 2 files/day × 5MB = 1PB/day
QPS: 100M DAU × 10 actions/day / 86400 sec ≈ 11.5K QPS

Bandwidth:
- Upload: 1PB/day / 86400 sec ≈ 12GB/s
- Download: 3x upload (read-heavy) = 36GB/s

Metadata database: 500M users × 100 files × 1KB = 50TB (MySQL sharded by user_id)
```

---

### Interview Talking Points

✅ **Mention**:
- File chunking (resume uploads, parallel uploads)
- Delta sync (only download changes, not all files)
- Conflict resolution (last-write-wins vs conflict copies)
- Deduplication (content hashing saves storage)
- CDN for downloads (S3 + CloudFront)

❌ **Avoid**:
- "Just store files in database" (use blob storage: S3, GCS)
- "Real-time sync" (polling every 30 seconds is fine for Dropbox)

---

## 2. Design Netflix (Video Streaming)

**Requirements**:
- Stream video (720p, 1080p, 4K)
- Personalized recommendations
- Resume playback from any device
- Scale: 200M users, 100M DAU, 10K titles, 1B hours watched/month

---

### High-Level Architecture

```
┌────────────┐
│   Client   │ (Web/Mobile/Smart TV)
└──────┬─────┘
       │
       ▼
┌─────────────────┐
│  CDN (CloudFront)│ ◄─── 95% of traffic (video streaming)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  API Gateway    │
└────────┬────────┘
         │
    ┌────┼────┬──────────┬─────────┐
    ▼    ▼    ▼          ▼         ▼
┌───────┐ ┌──────┐ ┌─────────┐ ┌─────────┐
│ Auth  │ │ User │ │ Video   │ │ Rec Svc │
│ Svc   │ │ Svc  │ │ Service │ │ (ML)    │
└───────┘ └──────┘ └────┬────┘ └─────────┘
                        │
                        ▼
                   ┌──────────┐
                   │ MySQL    │ (Video metadata)
                   └──────────┘

                   ┌──────────────────────┐
                   │  S3 (Video Files)    │ (Master files)
                   └──────────────────────┘
```

---

### API Design

```
GET    /api/v1/browse?category=action
GET    /api/v1/video/{video_id}/metadata
GET    /api/v1/video/{video_id}/stream?quality=1080p
POST   /api/v1/playback/resume
GET    /api/v1/recommendations/personalized
```

---

### Key Design Decisions

#### 1. Adaptive Bitrate Streaming (Handle Varying Network Speed)

**Problem**: User's network speed changes (WiFi → 4G) → buffering!

**Solution**: Encode video at multiple bitrates, client switches dynamically

```
Original video (4K master)
    ↓
Transcoding pipeline
    ↓
Output:
- 240p (400 kbps)  ← Low bandwidth
- 480p (1 Mbps)
- 720p (3 Mbps)
- 1080p (5 Mbps)
- 4K (15 Mbps)     ← High bandwidth

Client measures bandwidth every 10 seconds:
- Bandwidth = 2 Mbps → Play 480p
- Bandwidth drops to 500 kbps → Switch to 240p (no buffering!)
- Bandwidth increases to 6 Mbps → Switch to 1080p
```

**Technology**: HLS (HTTP Live Streaming) or DASH

**HLS manifest file** (.m3u8):
```
#EXTM3U
#EXT-X-STREAM-INF:BANDWIDTH=400000,RESOLUTION=426x240
240p.m3u8
#EXT-X-STREAM-INF:BANDWIDTH=1000000,RESOLUTION=640x480
480p.m3u8
#EXT-X-STREAM-INF:BANDWIDTH=3000000,RESOLUTION=1280x720
720p.m3u8
```

Each quality has its own playlist (e.g., 720p.m3u8):
```
#EXTM3U
#EXTINF:10.0
segment0.ts  ← 10-second video chunk
#EXTINF:10.0
segment1.ts
#EXTINF:10.0
segment2.ts
```

Client downloads chunks progressively (not entire video upfront).

---

#### 2. Video Encoding Pipeline (Offline Processing)

**When**: New movie/show uploaded → background job encodes it

```
┌─────────────────┐
│ Content Studio  │ (Uploads 4K master file)
└────────┬────────┘
         │
         ▼
┌────────────────────────────────┐
│  S3 (Raw Video Storage)        │
│  movie_master.mp4 (50GB)       │
└────────┬───────────────────────┘
         │
         ▼ (S3 event triggers Lambda)
┌────────────────────────────────┐
│  AWS Elemental MediaConvert    │ (Transcode to 5 resolutions)
│  - 240p, 480p, 720p, 1080p, 4K │
│  - Generate HLS segments       │
└────────┬───────────────────────┘
         │
         ▼
┌────────────────────────────────┐
│  S3 (Processed Video Storage)  │
│  movie/240p/segment0.ts        │
│  movie/480p/segment0.ts        │
│  ...                           │
└────────┬───────────────────────┘
         │
         ▼ (Push to CDN edge locations)
┌────────────────────────────────┐
│  CloudFront CDN                │ (Users stream from nearest edge)
└────────────────────────────────┘
```

**Encoding time**: 2-hour movie → 30 minutes to encode (parallel processing)

---

#### 3. Resume Playback (Across Devices)

**Data model**:
```sql
CREATE TABLE playback_position (
    user_id BIGINT,
    video_id BIGINT,
    position_seconds INT,  -- 1847 (30 min 47 sec into movie)
    updated_at TIMESTAMP,
    PRIMARY KEY (user_id, video_id)
);
```

**Client updates position every 10 seconds**:
```python
# Client (JavaScript)
setInterval(() => {
    const current_position = video_player.currentTime();  // 1850 seconds
    
    fetch('/api/v1/playback/resume', {
        method: 'POST',
        body: JSON.stringify({
            video_id: 12345,
            position_seconds: Math.floor(current_position)
        })
    });
}, 10000);  // Every 10 seconds
```

**Server** (write to database):
```python
@app.post('/api/v1/playback/resume')
def update_playback_position(user_id: int, video_id: int, position_seconds: int):
    db.execute(
        """
        INSERT INTO playback_position (user_id, video_id, position_seconds, updated_at)
        VALUES (?, ?, ?, NOW())
        ON DUPLICATE KEY UPDATE 
            position_seconds = ?,
            updated_at = NOW()
        """,
        (user_id, video_id, position_seconds, position_seconds)
    )
```

**Client loads position on start**:
```python
# Client fetches last position
response = requests.get(f'/api/v1/playback/{video_id}/position')
position = response.json()['position_seconds']  # 1847

# Seek to saved position
video_player.seek(position)
```

---

#### 4. Personalized Recommendations (Machine Learning)

**Architecture**:
```
User watches video → Event logged to Kafka → ML pipeline
                                               ↓
                                    ┌──────────────────────┐
                                    │ Recommendation Model │
                                    │ (Collaborative       │
                                    │  Filtering)          │
                                    └──────────┬───────────┘
                                               ↓
                                    ┌──────────────────────┐
                                    │ Pre-computed Results │
                                    │ (Redis Cache)        │
                                    │ user:123 → [v1, v2]  │
                                    └──────────────────────┘
```

**Two-phase approach**:
1. **Offline** (nightly batch job): Train model, pre-compute recommendations for all users
2. **Online** (API request): Fetch pre-computed recommendations from Redis

**Why pre-compute?**
- ML inference is slow (100ms to rank 10K videos)
- Pre-computing = instant response (2ms Redis lookup)

**Algorithm** (simplified collaborative filtering):
```python
# Find users similar to you
similar_users = find_users_with_similar_viewing_history(user_id=123)

# Recommend videos they watched but you haven't
recommendations = []
for similar_user in similar_users:
    their_videos = get_watched_videos(similar_user)
    my_videos = get_watched_videos(user_id=123)
    
    new_videos = their_videos - my_videos  # Set difference
    recommendations.extend(new_videos)

# Rank by popularity + genre preference
recommendations = rank_videos(recommendations, user_preferences)

return recommendations[:20]  # Top 20
```

---

### Capacity Estimation

```
Users: 200M total, 100M DAU
Videos watched: 1B hours/month = 33M hours/day
Average watch time: 33M hours / 100M users = 20 min/user/day

Video storage:
- 10K titles × 2 hours avg × 5 qualities × 3GB per quality = 300TB (master + transcoded)
- With CDN caching: 3PB (replicated to 100 edge locations)

Bandwidth:
- 1B hours/month streaming
- Assume 3 Mbps average (mix of 720p/1080p)
- 1B hours × 3600 sec × 3 Mbps / (30 days × 86400 sec) ≈ 4.2 Tbps (terabits per second!)

QPS:
- Metadata API: 100M users × 10 requests/day / 86400 ≈ 11.5K QPS
- Video streaming: Served by CDN (not backend API)
```

---

### Interview Talking Points

✅ **Mention**:
- Adaptive bitrate streaming (HLS/DASH)
- CDN for video delivery (99% of bandwidth)
- Offline transcoding (AWS MediaConvert)
- Resume playback (simple database table)
- Pre-computed recommendations (not real-time ML inference)

❌ **Avoid**:
- "Store video in database" (use S3 + CDN)
- "Real-time ML inference on every request" (too slow, pre-compute nightly)

---

## 3. Design Web Crawler (Google Bot)

**Requirements**:
- Crawl 10 billion web pages
- Respect robots.txt (don't crawl disallowed pages)
- Avoid infinite loops (cyclic links)
- Politeness (don't overwhelm servers with requests)
- Freshness (re-crawl popular pages daily, obscure pages monthly)
- Scale: 10K pages/sec crawl rate

---

### High-Level Architecture

```
┌─────────────────────┐
│  Seed URLs          │ (Starting point: reddit.com, nytimes.com, ...)
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  URL Frontier       │ (Queue of URLs to crawl)
│  (Priority Queue)   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Fetcher Threads    │ (100 workers fetch HTML in parallel)
│  (HTTP GET)         │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  HTML Parser        │ (Extract text + links)
└──────────┬──────────┘
           │
      ┌────┼────┐
      ▼    ▼    ▼
┌─────────┐ ┌──────────────┐ ┌─────────────────┐
│ Content │ │ URL Extractor│ │ Duplicate Checker│
│ Store   │ │ (Find links) │ │ (Bloom Filter)   │
│ (S3)    │ └──────┬───────┘ └─────────────────┘
└─────────┘        │
                   ▼
            Add new URLs to frontier (loop)
```

---

### Key Components

#### 1. URL Frontier (Priority Queue)

**Challenge**: Not all URLs are equal priority

```
High priority: nytimes.com (crawl every hour)
Low priority: myunknownblog.com (crawl every month)
```

**Solution**: Multi-level priority queue

```python
from queue import PriorityQueue

class URLFrontier:
    def __init__(self):
        self.queues = {
            'high': PriorityQueue(),     # Popular sites (crawl hourly)
            'medium': PriorityQueue(),   # Normal sites (crawl daily)
            'low': PriorityQueue()       # Obscure sites (crawl monthly)
        }
    
    def add_url(self, url, priority='medium'):
        """Add URL to appropriate queue"""
        timestamp = time.time()
        self.queues[priority].put((timestamp, url))
    
    def get_next_url(self):
        """Get next URL to crawl (high priority first)"""
        for priority in ['high', 'medium', 'low']:
            if not self.queues[priority].empty():
                _, url = self.queues[priority].get()
                return url
        return None
```

---

#### 2. Duplicate Detection (Avoid Crawling Same Page Twice)

**Problem**: Same page linked from multiple places → crawl once!

**Solution**: Bloom filter (space-efficient set membership)

```python
from bloom_filter import BloomFilter

# Bloom filter: 10B URLs, 1% false positive rate → ~1.5GB memory
seen_urls = BloomFilter(max_elements=10_000_000_000, error_rate=0.01)

def should_crawl(url):
    """Check if URL already crawled"""
    if url in seen_urls:
        return False  # Already crawled (or false positive, rare)
    else:
        seen_urls.add(url)
        return True  # New URL, crawl it!
```

**Alternative**: Store URL hashes in distributed database (Cassandra)
```python
url_hash = hashlib.sha256(url.encode()).hexdigest()

# Check if exists
existing = cassandra.execute(
    "SELECT 1 FROM crawled_urls WHERE url_hash = ?",
    (url_hash,)
)

if not existing:
    # New URL, crawl it
    cassandra.execute(
        "INSERT INTO crawled_urls (url_hash, crawled_at) VALUES (?, NOW())",
        (url_hash,)
    )
```

---

#### 3. Politeness (Don't Overwhelm Servers)

**Problem**: Crawling nytimes.com at 100 req/sec → DDoS attack! ❌

**Solution**: Rate limit per domain

```python
from collections import defaultdict
import time

class PoliteCrawler:
    def __init__(self):
        self.last_crawl_time = defaultdict(float)  # domain → timestamp
        self.crawl_delay = 1.0  # 1 second between requests to same domain
    
    def fetch(self, url):
        domain = extract_domain(url)  # "nytimes.com"
        
        # Check if we need to wait
        time_since_last_crawl = time.time() - self.last_crawl_time[domain]
        if time_since_last_crawl < self.crawl_delay:
            sleep_time = self.crawl_delay - time_since_last_crawl
            time.sleep(sleep_time)
        
        # Fetch page
        response = requests.get(url)
        
        # Update last crawl time
        self.last_crawl_time[domain] = time.time()
        
        return response.text
```

**Bonus**: Read robots.txt (respect website's crawl policy)
```python
# GET https://nytimes.com/robots.txt
# User-agent: *
# Disallow: /admin/
# Crawl-delay: 2

def is_allowed(url):
    """Check robots.txt"""
    domain = extract_domain(url)
    robots_txt = fetch_robots_txt(domain)
    
    # Parse robots.txt (use libraries like robotparser)
    return robots_txt.can_fetch("Googlebot", url)
```

---

#### 4. Distributed Crawling (Scale to 10K Pages/Sec)

**Single machine**: 10 pages/sec (limited by network I/O)

**Solution**: 1000 crawler machines (10K pages/sec total)

**Architecture**:
```
┌─────────────────────┐
│  URL Coordinator    │ (Kafka: distributes URLs to crawlers)
│  (Kafka Topic)      │
└──────────┬──────────┘
           │
     ┌─────┼─────┬─────┬───────┐
     ▼     ▼     ▼     ▼       ▼
  ┌────┐ ┌────┐ ┌────┐ ┌────┐ ... (1000 crawler workers)
  │ C1 │ │ C2 │ │ C3 │ │ C4 │
  └─┬──┘ └─┬──┘ └─┬──┘ └─┬──┘
    │      │      │      │
    └──────┴──────┴──────┴────────▶ Store content in S3
```

**Partitioning strategy**: Assign domains to workers (consistent hashing)
```python
# Worker 1 crawls: *.com domains (hash % 1000 == 0)
# Worker 2 crawls: *.com domains (hash % 1000 == 1)
# ...

worker_id = hash(domain) % num_workers
```

**Benefit**: Same worker always crawls same domain → easy to enforce politeness per domain

---

### Data Model

**Cassandra (Crawled URLs)**:
```cql
CREATE TABLE crawled_urls (
    url_hash TEXT PRIMARY KEY,  -- SHA-256 of URL
    url TEXT,
    crawled_at TIMESTAMP,
    content_s3_key TEXT,  -- s3://crawl-data/2024/03/15/page123.html
    status_code INT       -- 200, 404, 500
);
```

**S3 (HTML Content)**:
```
s3://crawl-data/
  2024/
    03/
      15/
        page0001.html
        page0002.html
        ...
```

---

### Capacity Estimation

```
Pages to crawl: 10B
Crawl rate: 10K pages/sec
Time to crawl all: 10B / 10K = 1M seconds ≈ 11.5 days (fresh index every 2 weeks)

Storage per page: 100KB (HTML)
Total storage: 10B × 100KB = 1PB

Bandwidth:
- Download: 10K pages/sec × 100KB = 1GB/sec

Metadata (Cassandra):
- 10B URLs × 200 bytes (url_hash + url + metadata) = 2TB

Worker machines:
- 1 machine = 10 pages/sec
- 1000 machines = 10K pages/sec ✅
```

---

### Interview Talking Points

✅ **Mention**:
- URL frontier (priority queue for fresh vs stale pages)
- Duplicate detection (Bloom filter or Cassandra)
- Politeness (rate limit per domain, respect robots.txt)
- Distributed crawling (partition domains across workers)

❌ **Avoid**:
- "Crawl everything every day" (impossible, 10B pages × 100KB = 1PB/day)
- "BFS from seed URLs" (too simplistic, no priority, no politeness)

---

## 4. Design Leaderboard (Real-Time Gaming)

**Requirements**:
- Track scores for millions of players
- Get top 100 players globally (real-time)
- Get player's rank (e.g., "You are rank #45,231")
- Update scores frequently (every game completion, ~10K updates/sec)
- Low latency (<50ms for reads)

---

### Architecture

```
┌────────────┐
│  Client    │ (Mobile game)
└──────┬─────┘
       │
       ▼
┌─────────────────┐
│  API Gateway    │
└────────┬────────┘
         │
    ┌────┼────┐
    ▼    ▼    ▼
┌────────────┐ ┌──────────────┐
│ Leaderboard│ │ User Service │
│ Service    │ │              │
└──────┬─────┘ └──────────────┘
       │
       ▼
┌─────────────────┐
│ Redis Sorted Set│ (In-memory, sorted by score)
└─────────────────┘
```

---

### Data Model (Redis Sorted Set)

**Redis** is perfect for leaderboards!

```redis
# Sorted set: scores are automatically sorted
ZADD leaderboard:global 1500 "player:123"
ZADD leaderboard:global 2000 "player:456"
ZADD leaderboard:global 1800 "player:789"

# Get top 10 players (O(log N + 10))
ZREVRANGE leaderboard:global 0 9 WITHSCORES
# Returns:
# 1. "player:456" (score 2000)
# 2. "player:789" (score 1800)
# 3. "player:123" (score 1500)

# Get player's rank (O(log N))
ZREVRANK leaderboard:global "player:123"
# Returns: 2 (0-indexed, so rank = 3)

# Get player's score (O(1))
ZSCORE leaderboard:global "player:123"
# Returns: 1500

# Update score (O(log N))
ZINCRBY leaderboard:global 50 "player:123"  # Add 50 points
# New score: 1550
```

---

### API Design

```
POST   /api/v1/scores/update
       Body: {"player_id": "123", "score": 1500}

GET    /api/v1/leaderboard/top?limit=100
       Returns: [{"player_id": "456", "score": 2000, "rank": 1}, ...]

GET    /api/v1/leaderboard/rank?player_id=123
       Returns: {"player_id": "123", "score": 1500, "rank": 3}

GET    /api/v1/leaderboard/nearby?player_id=123&range=10
       Returns: Players ranked ±10 around player 123
```

---

### Implementation

```python
import redis

r = redis.Redis(host='localhost', port=6379)

@app.post('/api/v1/scores/update')
def update_score(player_id: str, score: int):
    """Update player's score"""
    # Update score in Redis sorted set
    r.zadd('leaderboard:global', {f'player:{player_id}': score})
    
    # Also store in MySQL (persistent backup)
    db.execute(
        "INSERT INTO scores (player_id, score, updated_at) VALUES (?, ?, NOW()) "
        "ON DUPLICATE KEY UPDATE score = ?, updated_at = NOW()",
        (player_id, score, score)
    )
    
    return {"status": "success"}

@app.get('/api/v1/leaderboard/top')
def get_top_players(limit: int = 100):
    """Get top N players"""
    # ZREVRANGE: Get top players (sorted descending by score)
    results = r.zrevrange('leaderboard:global', 0, limit - 1, withscores=True)
    
    return [
        {
            "player_id": player_id.decode().split(':')[1],
            "score": int(score),
            "rank": idx + 1
        }
        for idx, (player_id, score) in enumerate(results)
    ]

@app.get('/api/v1/leaderboard/rank')
def get_player_rank(player_id: str):
    """Get player's rank"""
    # ZREVRANK: Get rank (0-indexed)
    rank = r.zrevrank('leaderboard:global', f'player:{player_id}')
    
    if rank is None:
        return {"error": "Player not found"}
    
    # ZSCORE: Get score
    score = r.zscore('leaderboard:global', f'player:{player_id}')
    
    return {
        "player_id": player_id,
        "score": int(score),
        "rank": rank + 1  # Convert to 1-indexed
    }

@app.get('/api/v1/leaderboard/nearby')
def get_nearby_players(player_id: str, range: int = 10):
    """Get players ranked near this player"""
    # Get player's rank
    rank = r.zrevrank('leaderboard:global', f'player:{player_id}')
    
    if rank is None:
        return {"error": "Player not found"}
    
    # Get players in range [rank - 10, rank + 10]
    start = max(0, rank - range)
    end = rank + range
    
    results = r.zrevrange('leaderboard:global', start, end, withscores=True)
    
    return [
        {
            "player_id": player_id.decode().split(':')[1],
            "score": int(score),
            "rank": start + idx + 1
        }
        for idx, (player_id, score) in enumerate(results)
    ]
```

---

### Scaling Strategies

#### 1. Sharding (For >10M Players)

**Problem**: Single Redis instance has memory limit (64GB typical)

**Solution**: Shard leaderboard by region or game mode

```
Global leaderboard → Too big!

Regional leaderboards:
- leaderboard:us (5M players)
- leaderboard:eu (3M players)
- leaderboard:asia (8M players)

Game mode leaderboards:
- leaderboard:ranked
- leaderboard:casual
```

#### 2. Caching Top Players (Reduce Redis Load)

```python
# Cache top 100 in application memory (refresh every 10 seconds)
top_100_cache = None
last_refresh = 0

@app.get('/api/v1/leaderboard/top')
def get_top_players(limit: int = 100):
    global top_100_cache, last_refresh
    
    if time.time() - last_refresh > 10:
        # Refresh cache from Redis
        top_100_cache = r.zrevrange('leaderboard:global', 0, 99, withscores=True)
        last_refresh = time.time()
    
    return format_leaderboard(top_100_cache)
```

---

### Capacity Estimation

```
Players: 10M active players
Leaderboard updates: 10K updates/sec (after each game)

Memory (Redis):
- 10M players × 100 bytes (player_id + score) = 1GB ✅ Fits in memory!

QPS:
- Updates: 10K writes/sec
- Reads (top 100): 100K reads/sec (popular feature)
- Redis can handle 100K+ QPS easily

Persistence:
- Redis snapshot every hour → MySQL backup (in case Redis crashes)
```

---

### Interview Talking Points

✅ **Mention**:
- Redis Sorted Set (ZADD, ZREVRANGE, ZREVRANK) → O(log N) operations
- Global vs regional leaderboards (sharding strategy)
- Cache top 100 (reduce Redis load)
- MySQL backup (persistence, but reads come from Redis)

❌ **Avoid**:
- "Sort all players on every request" (too slow, O(N log N))
- "Use SQL with ORDER BY" (slow for millions of rows)

---

**End of Interview Questions** 📝
