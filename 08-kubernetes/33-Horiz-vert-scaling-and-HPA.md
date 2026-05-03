## Question

Difference between horizontal scaling and vertical scaling ?

## Answer

Horizontal scaling means adding more machines (nodes/instances), while vertical scaling means increasing the power (CPU/RAM) of an existing machine

### Vertical Scaling 

#### ✅ Pros
- Simple to implement
- No architecture changes
- Good for small systems
#### ❌ Cons
- Hardware limits (you can’t scale forever)
- Single point of failure
- Downtime may be required to upgrade

### Horizontal Scaling

#### ✅ Pros
- Highly scalable
- Fault tolerant
- No single point of failure
#### ❌ Cons
- More complex architecture
- Needs load balancing
- Requires stateless design (usually)

> Kubernetes mainly supports Horizontal scaling; VPA exists too.

### Summary

Vertical scaling means increasing resources like CPU and memory of a single machine, while horizontal scaling means adding more machines to distribute the load.
Vertical scaling is simpler but limited by hardware, whereas horizontal scaling is more scalable and fault-tolerant and is the preferred approach in distributed
systems like Kubernetes.

> Modern cloud-native systems prefer horizontal scaling because it aligns with distributed, fault-tolerant architecture.

---

## HPA

HPA (Horizontal Pod Autoscaler) automatically scales the number of Pod replicas based on observed metrics like CPU, memory, or custom metrics.
HPA continuously monitors metrics and adjusts the ReplicaSet size to match demand.

### How HPA works:
1. Metrics collection:
- Metrics Server collects:
   - CPU usage
   - Memory usage (optional)
- Or external systems like:
   - Prometheus (custom metrics)

   👉 It fetches:

   Current Pod usage
   Node-level aggregated data

2. HPA Controller loop:
   Inside kube-controller-manager, the HPA controller runs a loop:
- Every ~15 seconds (default)

   It:

- Reads metrics
- Compares with target values

3. Decision making

   HPA calculates based on metrics to scale up or down

4. Scaling action

   HPA updates:

- Deployment → ReplicaSet → Pods

   Then Kubernetes:

- Creates or deletes Pods
- Scheduler assigns new Pods to nodes
- Kubelet runs them

5. Stabilization (important)

   HPA avoids flapping using:

- Cooldown periods
- Scaling policies (min/max limits)
  
### ⚠️ Key limitations
1. Metrics delay
- Not real-time (15–30 sec delay)
2. Only reactive
- Scales after load increases
3. CPU-based scaling is tricky
- CPU spikes ≠ always need scaling

`Traffic increases → CPU rises → Metrics Server reports → HPA controller calculates → Deployment scaled → Scheduler places new Pods → load is distributed`

### Summary

HPA works by continuously monitoring resource metrics using the Metrics Server or external metric providers.
The HPA controller, which runs inside the kube-controller-manager, periodically compares current usage with the target thresholds 
and calculates the desired number of replicas. It then updates the Deployment, which triggers the creation or deletion of Pods.
The scheduler and kubelet handle placement and execution of these Pods.

> HPA is a control loop that continuously reconciles actual resource usage with desired thresholds to maintain application performance.

---

## HPA vs VPA vs Cluster Autoscaler

|  Feature |         HPA         |           VPA          | Cluster Autoscaler |
|:--------:|:-------------------:|:----------------------:|:------------------:|
| Scales   | Pods                | CPU/Memory             | Nodes              |
| Level    | App                 | Pod resources          | Infrastructure     |
| Trigger  | CPU/metrics         | Usage history          | Pending Pods       |
| Action   | Add/remove replicas | Change requests/limits | Add/remove nodes   |
| Downtime | No                  | Sometimes (restart)    | No                 |

- HPA handles application scaling.
- VPA optimizes resource sizing, not replica count. VPA often restarts Pods to apply new resources
- Cluster Autoscaler fixes capacity problems at the node level.

### 🧩 How they work together

In real systems:

Flow:
- Traffic increases
- HPA increases Pods
- Pods don’t fit → Cluster Autoscaler adds nodes
- VPA adjusts resource sizing over time

> They complement each other

### Common pitfalls

1. HPA + VPA conflict
  - HPA uses CPU %
  - VPA changes CPU request
> Can cause unstable scaling loops
2. VPA vs HPA together
  - Often not recommended on same metrics
3. Cluster Autoscaler dependency
  - Only reacts when Pods are pending

### Summary

HPA scales the number of Pods based on metrics like CPU and memory. 
VPA adjusts the resource requests and limits of individual Pods based on historical usage. 
Cluster Autoscaler operates at the infrastructure level and adds or removes nodes when Pods cannot be scheduled due to insufficient capacity. 
Together, they handle scaling at different layers: application, Pod, and cluster.

> HPA handles demand, VPA optimizes sizing, and Cluster Autoscaler ensures capacity.

