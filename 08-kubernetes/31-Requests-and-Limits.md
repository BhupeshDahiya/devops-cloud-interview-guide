### Question

What are resource requests and limits ?

### Answer

Resource requests define the minimum CPU and memory a container needs and are used by the scheduler to place Pods on nodes. 
Resource limits define the maximum resources a container can consume and are enforced at runtime using cgroups. 
If a container exceeds CPU limits, it gets throttled, and if it exceeds memory limits, it is OOMKilled.

---

### Why this matters?

#### 1. Scheduling decisions

Nodes are chosen based on requests, not limits.

#### 2. Stability

Limits prevent:

- noisy neighbors
- resource starvation

### 3. QoS classification

Requests/limits define QoS:

- Guaranteed → request = limit
- Burstable → request < limit
- BestEffort → none set

---

### how wrong requests/limits can destabilize a cluster ?

Bad resource requests/limits lead to poor scheduling decisions, resource starvation, and unstable nodes due to throttling or OOM kills.

#### ⚠️ 1. Requests set wrong → bad scheduling (bin-packing failure)
**Case: Requests too low**

Example:

App actually needs 1 CPU
You set request = 100m

👉 What happens:

- Scheduler overpacks nodes
- Too many Pods land on same node
- Node becomes overloaded in reality

🔥 Result:

- High CPU contention
- Latency spikes
- Pods fighting for resources

**Case: Requests too high**

Example:

App needs 200m CPU
You set request = 2 CPU

👉 What happens:

- Scheduler thinks Pod is “expensive”
- Leaves nodes underutilized
- Pods stay Pending

🔥 Result:

- Cluster looks “full” but is actually idle
- Wasted capacity
#### ⚠️ 2. Limits too low → unstable applications
**Case: CPU limit too strict**
App needs bursts of CPU
Limit is low

👉 Result:

- CPU throttling
- Slow requests
- Performance degradation

**Case: Memory limit too low**
App exceeds memory slightly

👉 Result:

- OOMKilled
- Frequent restarts
- CrashLoopBackOff
#### ⚠️ 3. Limits too high → node instability
**Problem:**
No effective upper bound per Pod

👉 Result:

- One Pod can consume:
- CPU spikes
- Memory exhaustion

🔥 Node-level impact:

- Other Pods starve
- kubelet triggers eviction
- Possible node pressure
#### ⚠️ 4. No requests/limits (Worst case)

👉 Pods become BestEffort

Result:
- No scheduling guarantees
- First to be evicted under pressure
- Unpredictable performance
#### ⚠️ 5. Wrong requests affect QoS → eviction risk

If requests/limits are misconfigured:

- Pods end up as BestEffort or Burstable
- Under pressure → they are evicted first

---

|      Mistake      |               Impact              |
|:-----------------:|:---------------------------------:|
| Requests too low  | Overloaded nodes, noisy neighbors |
| Requests too high | Pending Pods, wasted capacity     |
| Limits too low    | OOMKilled, throttling             |
| Limits too high   | Node starvation risk              |
| No limits         | Unpredictable cluster behavior    |

---

### Summary
ncorrect resource requests and limits can destabilize a Kubernetes cluster in multiple ways. 
If requests are too low, the scheduler overpacks nodes, leading to CPU and memory contention. 
If requests are too high, Pods remain pending and cluster capacity is wasted. Improper limits can cause CPU throttling or OOM kills, 
while overly high limits allow a single Pod to consume excessive resources and starve others, leading to node pressure and evictions.
Overall, wrong configurations disrupt scheduling efficiency, application stability, and cluster reliability.

