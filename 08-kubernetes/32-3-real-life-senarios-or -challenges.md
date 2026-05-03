# 🎯 1. Resource Contention / Cluster Instability
## ⭐ Question:

“Tell me about a Kubernetes challenge you faced.”

## ✅ Answer (STAR)

### Situation:
In one of my projects, we had a shared Kubernetes cluster used by multiple teams like payments, frontend, and order services. Initially, we only used namespaces for separation.

### Task:
My responsibility was to ensure cluster stability and fair resource usage across teams.

### Action:
We started noticing random pod crashes across namespaces. After investigation, I found one service in the payments namespace had a memory leak and was consuming a huge amount of RAM, impacting the entire cluster.

So I implemented a two-level control:

First, I applied resource quotas at namespace level after discussing with teams and getting their performance benchmarking numbers.
Then I enforced resource limits at pod level in deployment YAMLs to prevent individual pods from over-consuming.

### Result:
This significantly improved stability. The blast radius reduced from cluster-wide failures to just a single pod. Also, troubleshooting became easier because we could quickly identify which pod was causing issues.

> After this, cluster outages due to resource contention almost stopped.

# 🎯 2. OOMKilled / CrashLoopBackOff Debugging
## ⭐ Question:

How do you handle a pod going into CrashLoopBackOff?

## ✅ Answer (STAR)

### Situation:
We had a production issue where a Java-based microservice kept going into CrashLoopBackOff.

### Task:
I had to identify whether it was an infrastructure issue or application issue and resolve it quickly.

### Action:
First, I checked the pod using kubectl describe and confirmed it was failing due to OOMKilled.

I verified that resource limits and quotas were already correctly configured, so the issue wasn’t due to noisy neighbors.

Then I logged into the pod and collected heap dump and thread dump, and shared them with the development team for analysis.

### Result:
The dev team identified a memory leak in one of the threads and fixed it in the next release. After deployment, the issue was resolved.


> From DevOps side, my focus was isolating the issue and providing the right debugging data quickly.

# 🎯 3. Kubernetes Upgrade (High-impact answer)
## ⭐ Question:

Have you worked on Kubernetes upgrades?

## ✅ Answer (STAR)

### Situation:
Yes, we had to upgrade our cluster from one version to another as part of security and feature updates.

### Task:
My responsibility was to perform the upgrade with minimal downtime and zero impact to running services.

### Action:
I created a detailed upgrade plan which included:

- Taking backups (of critical data like etcd)
- Carefully reviewing release notes for breaking changes
- Planning upgrade sequence

For execution:

- I took backups of critical data like etcd
- I then tested the upgrade in a staging environment before applying it to production
- I upgraded the control plane first, since worker nodes must remain compatible with it.
- Then followed a rolling upgrade strategy for worker nodes
  - Drained nodes & Marked them unschedulable
  - Upgraded kubelet and packages
  - Bringing back nodes to cluster (Uncordon)

- I monitored pod rescheduling, checked for failures like CrashLoopBackOff or OOMKilled, and ensured services remained healthy throughout

### Result:
The upgrade was completed without downtime, and all services continued running smoothly.


> Having a documented runbook helped us repeat the process safely in future upgrades.

> I also validated workloads post-upgrade to ensure no hidden issues from API deprecations or dependency incompatibilities.

## EKS Specific

### Situation:
Yes, I’ve worked on upgrading an EKS cluster to keep it within the supported Kubernetes versions and apply security updates.

### Task:
My goal was to perform the upgrade with zero downtime and ensure all workloads and integrations remained stable.

### Action:
Since EKS is a managed service, I first reviewed AWS and Kubernetes release notes to identify breaking changes and deprecated APIs. I also verified compatibility of components like Ingress controllers, autoscalers, and monitoring tools.

I validated everything in a staging cluster before touching production.

For the upgrade process, I upgraded the EKS control plane using AWS-managed upgrades, which are highly available and handled by AWS.

After that, I upgraded the worker nodes using a rolling strategy:

- Drained nodes one by one and Marked them unschedulable
- Updated node groups (or AMIs)
- Brought nodes back into the cluster”

> I ensured PodDisruptionBudgets were respected so that services remained available during node rotations.

> I continuously monitored workloads for issues like CrashLoopBackOff, OOMKilled, or scheduling failures.

### Result:
The upgrade was completed without downtime, and workloads were rescheduled seamlessly across updated nodes.


> Using managed control plane upgrades in EKS reduced risk, while controlled node group upgrades ensured application stability.
