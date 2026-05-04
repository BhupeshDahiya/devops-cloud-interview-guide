# 🔹 Problem

**A pod crashes randomly with OOMKilled — how do you identify and fix it?**

---

# 🎯 Problem Framing

Since the pod is **OOMKilled**, it is a memory-related issue.

👉 Goal:

- Determine if it’s due to **insufficient memory limits**  
- Or an **application-level memory issue**  

---

# ✅ Step-by-Step Troubleshooting

## 🔸 1. Confirm the Issue

First, confirm the actual cause of the crash.

- kubectl get pods → check status (CrashLoopBackOff)  
- kubectl describe pod → confirm OOMKilled  

👉 Key point:

- CrashLoopBackOff = state  
- OOMKilled = actual cause  

---

## 🔸 2. Check Resource Configuration

Check memory requests and limits.

- kubectl describe deployment  

Look at:

- Memory requests  
- Memory limits  

---

## 🔸 3. Analyze Historical Usage (KEY STEP 🔥)

Check memory usage over time using metrics.

👉 Use:

- Prometheus  
- Grafana  

Look for:

- Is memory consistently near limit?  
- Or was it a sudden spike?  

---

## 🔸 4. Decision Point (CRITICAL 🚀)

### ✅ Case 1: Limits Too Low

If memory usage is consistently close to limits:

✔ Action:

- Increase memory limits gradually  
- Tune autoscaling if needed  

👉 Example:

- Limit = 1GB  
- Usage = 900MB consistently → increase  

---

### ✅ Case 2: Random / Sudden Spike

If usage is usually low but crashes randomly:

✔ Likely causes:

- Memory leak  
- Traffic spike  
- Application bug  

✔ Action:

- Check logs around crash time  
- Capture debugging data:
  - Heap dump  
  - Thread dump (for Java apps)  

---

## 🔸 5. Deep Debugging (Advanced)

If needed:

- Reproduce the issue  
- Simulate load (QA / load testing)  
- Capture dumps  

👉 Work with developers to identify root cause  

---

## 🔸 6. Fix the Issue

Fix depends on root cause.

Options:

- Increase memory (infra fix)  
- Fix memory leak (code fix)  
- Optimize queries / caching  
- Tune runtime (e.g., JVM)  

---

# 🔥 Final Answer

First, confirm the pod is OOMKilled using kubectl describe. Then check resource limits and analyze historical memory usage using Prometheus and Grafana. If the application consistently hits limits, increase memory limits. If crashes are random, investigate logs and capture heap or thread dumps to identify issues like memory leaks. If needed, reproduce the issue using load testing and fix it at the application level.

---

# ⚠️ Important Corrections

❌ Restarting pod  
👉 Only temporary fix, not a solution  

❌ Increasing memory blindly  
👉 Must be justified using metrics  

---

# 🔥 Common Follow-ups

## 🎯 Why not just increase memory?

Because it only hides the problem. If there’s a memory leak, it will happen again.

---

## 🎯 What is OOMKilled?

It occurs when a container exceeds its memory limit and is terminated by the kernel.

---

## 🎯 How do you detect memory leak?

- Analyze heap dumps  
- Observe memory continuously increasing over time  

---

## 🎯 What if multiple pods are OOMKilled?

- Check namespace-level resource usage  
- Check cluster capacity and node pressure  

---

# 🚀 Killer Line

OOMKilled is not just a resource issue—it’s either a capacity planning problem or an application memory issue, and I treat it accordingly.

---

# 🚀 Quick Revision

- Confirm OOMKilled  
- Check limits  
- Check metrics  
- If consistent → increase limits  
- If random → debug + dumps  
- Fix root cause  
