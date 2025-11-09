# Modern System Design - Refactored Sample

This file demonstrates the improved structure and note-taking style for system design documentation.

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
