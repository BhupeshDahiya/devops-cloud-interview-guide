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

> QoS in Kubernetes classifies Pods into Guaranteed, Burstable, and BestEffort based on their CPU and memory requests and limits.
This classification determines eviction priority when a node experiences resource pressure, with BestEffort Pods being evicted first and Guaranteed Pods
being the last.

> QoS affects both eviction and OOMKill behavior. In OOM situations, the Linux kernel uses QoS-based OOM scores where BestEffort Pods are killed first, followed by Burstable, and Guaranteed last. For node-pressure eviction, kubelet proactively removes Pods in the same order but gracefully before the kernel OOM killer is triggered.

---

### Extras

#### 1. How QoS affects OOMKill behavior
Kubernetes + QoS decide who gets killed first, QoS influences OOMKill behavior by assigning different OOM scores to Pods. BestEffort Pods are killed first, Burstable next, and Guaranteed Pods are the most protected because they have strict resource reservations.

#### 2. QoS + Node Pressure Eviction System
Guaranteed Pods are not absolutely safe—they are only the last to be evicted or OOM-killed, but can still be removed if the node is critically under pressure.


