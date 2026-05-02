## Your Deployment Has `replicas: 3`, but Only 1 Pod Is Running — What Could Be Wrong?

### Short explanation of the question  
This scenario tests your ability to troubleshoot **replica mismatches** in Kubernetes — where the desired state (3 pods) doesn’t match the actual state (1 pod running).

---

### Answer  
There could be several reasons: resource constraints on nodes, scheduling issues, crashlooping pods, or affinity/taint restrictions that prevent pods from starting.

---

### Detailed explanation of the answer for readers’ understanding

---

### 🔍 Troubleshooting Checklist

#### ✅ 1. **Check Pod Statuses**

Run:
```bash
kubectl get pods -l app=my-app
```

You might see:
- 1 Running
- 2 Pending / CrashLoopBackOff / ImagePullBackOff

---

#### ✅ 2. **Describe the Deployment and Pods**

```bash
kubectl describe deployment my-deployment
kubectl describe pod <pod-name>
```

Look for:
- Events at the bottom (e.g., “failed scheduling”)
- Crash loop messages
- Image pull errors
- Volume mounting errors

---

#### ✅ 3. **Check Node Capacity**

Maybe the other pods can’t be scheduled due to insufficient **CPU/memory**.

Run:
```bash
kubectl describe nodes
```

If the nodes are out of resources, new pods won’t start.

---

#### ✅ 4. **Check Affinity Rules and Taints**

If your deployment or namespace has node affinity or tolerations set, it may restrict where pods can land.

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
        - matchExpressions:
            - key: disktype
              operator: In
              values:
                - ssd
```

Pods will only be scheduled on matching nodes.

---

#### ✅ 5. **Check for Pod Crashes**

Run:
```bash
kubectl logs <pod-name>
```

If pods are crashing, Kubernetes may try to restart them, but they'll never stay in the "Running" state.

---
#### ✅ 6. Readiness / liveness probe failures
- Pods are running but not ready
- Service might only send traffic to 1 pod

---
#### ✅ 7. Quotas / limits
- Namespace has ResourceQuota
- Pod creation is blocked

---

### 🧪 Real-World Example

> “We once set a memory limit of `100Mi` in the deployment, but the app needed 200Mi. Only one pod was running because the others kept OOMKilled. Increasing the memory resolved the issue.”

---

### 🔄 Summary Table

| Check                         | What It Tells You                           |
|------------------------------|---------------------------------------------|
| `kubectl get pods`           | Status of all pods                          |
| `kubectl describe`           | Events and reasons for pending/crashes      |
| Node capacity                | Resource exhaustion                         |
| Affinity/Taints              | Constraints preventing scheduling           |
| Container logs               | Runtime crashes or app issues               |

---

### Key takeaway

> If `replicas: 3` but only 1 pod is running, start by checking pod status, node resource limits, crash logs, and affinity/taint rules. The issue is often with scheduling or container-level crashes.
