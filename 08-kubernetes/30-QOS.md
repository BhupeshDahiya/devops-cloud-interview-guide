## Question

What is QOS?

## Answer

In Kubernetes, QoS (Quality of Service) is a way to classify Pods based on how much CPU and memory they request and how strictly those resources are defined.
It helps Kubernetes decide which Pods to protect and which to evict first under resource pressure.

---

#### When a node runs out of resources:

Kubernetes must decide which Pods to kill first
QoS provides that priority order


#### 📊 The 3 QoS classes
### 1. Guaranteed (Highest priority 🔵)

> Most stable Pods

Conditions:
- CPU and memory requests = limits
- All containers in Pod must define both

### 2. Burstable (Medium priority 🟡)

> Flexible Pods

Conditions:
- Requests defined, but limits may be higher or missing

### 3. BestEffort (Lowest priority 🔴)

> No resource guarantees

Conditions:
- No requests or limits defined

---

### When node is under pressure:

`BestEffort → Burstable → Guaranteed`

### Summary

QoS in Kubernetes classifies Pods into Guaranteed, Burstable, and BestEffort based on their CPU and memory requests and limits.
This classification determines eviction priority when a node experiences resource pressure, with BestEffort Pods being evicted first and Guaranteed Pods
being the last.

