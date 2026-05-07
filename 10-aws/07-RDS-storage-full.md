# 🔹 What Will You Do When AWS RDS Storage Is Full?

---

## 🎯 Best Way to Start

I would handle this in two phases — immediate mitigation to restore service, and long-term remediation to prevent recurrence.

Strong opening — sounds very professional.

---

## ✅ Phase 1: Immediate Mitigation (Restore Service Fast)

### 🔸 1. Assess Impact

First, I would verify whether the database is read-only, failing writes, or fully unavailable.

> Shows operational thinking.

---

### 🔸 2. Take Snapshot (If Possible)

Before making major changes, I would ensure recent snapshots/backups are available.

**⚠️ Note:**  
- If storage is completely exhausted, snapshot creation may fail or be risky.  
- Better wording:  
   Verify backup availability or take snapshot if feasible.

---

### 🔸 3. Increase Storage (Immediate Relief)

Then I would increase allocated storage or enable storage autoscaling if not already configured.

**Mention:**  
- Amazon RDS supports storage autoscaling for many engines.

---

### 🔸 4. Monitor Recovery

I would monitor free storage space, DB health, and application recovery after scaling.

---

## ✅ Phase 2: Root Cause Analysis (MOST IMPORTANT 🔥)

### 🔸 5. Identify What Is Consuming Space

Next, I would identify which database objects are consuming abnormal storage.

**Check for:**  
- Large databases  
- Large tables  
- Index growth  
- Temporary tables  
- Logs / WAL / binlogs

---

### 🔸 6. Run DB-Level Analysis

**Example:**  
- PostgreSQL → table size queries  
- MySQL → information_schema analysis  

> Exact SQL not required unless interviewer asks.

---

### 🔸 7. Work With Dev/DB Teams

I would collaborate with developers or DBAs to determine why storage grew unexpectedly.

**Possible causes:**  
- Unbounded logging  
- Poor retention policy  
- Duplicate data  
- Missing cleanup jobs  
- Large indexes  
- Table bloat

---

### 🔸 8. Optimize Storage Usage

**Possible fixes:**  
- Archive old data  
- Delete unused objects  
- Vacuum/analyze (PostgreSQL)  
- Partition tables  
- Optimize indexes  

> This is the real long-term fix.

---

## ✅ Phase 3: Prevention & Monitoring

### 🔸 9. Configure Monitoring & Alerts

To prevent recurrence, I would configure monitoring and alerts.

**Use:**  
- Amazon CloudWatch metric: `FreeStorageSpace`  

**Alerts:**  
- 20% free → warning  
- 10% free → critical

---

### 🔸 10. Capacity Planning

I would review storage growth trends for better capacity planning.

> Very strong operational point.

---

## 🔥 Final Polished Answer (Say This)

I would first focus on restoring service by verifying backups and increasing RDS storage or enabling autoscaling if needed.
After stabilizing the system, I would analyze which databases, tables, indexes, or logs are consuming excessive storage and work with developers or DBAs
to optimize retention, cleanup, and indexing strategies. Finally, I would configure CloudWatch alerts on FreeStorageSpace and review storage growth trends to
prevent future incidents.

---

## 🔥 Tough Follow-ups (Very Common)

- **Can you reduce RDS storage after increasing it?**  
  > No, RDS storage generally cannot be reduced directly.

- **What causes sudden storage growth?**  
  > Logs, large transactions, table bloat, uncleaned temp data, or retention issues.

- **What CloudWatch metric would you monitor?**  
  > `FreeStorageSpace`

- **How do you prevent this long-term?**  
  > Monitoring, retention policies, cleanup jobs, partitioning, and storage trend analysis.

---

## 🚀 Killer Line (Use This)

> “Increasing storage solves the symptom; identifying abnormal storage growth solves the real problem.”

---

## 🚀 Ultra Short Revision

- Restore service  
- Increase storage  
- Identify large tables/logs/indexes  
- Optimize cleanup/retention  
- Add CloudWatch alerts  
- Plan capacity better
