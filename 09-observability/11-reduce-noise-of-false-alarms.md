# 🔹 Problem

**You got a false alert at 2 AM — how do you reduce alert noise?**

---

# 🎯 Problem Framing

False alerts (alert noise) reduce signal quality and lead to **alert fatigue**, causing real issues to be ignored.

👉 Goal:

- Reduce noise  
- Improve alert quality and actionability  

---

# ✅ Core Strategy

My approach is not to eliminate alerts, but to **make them meaningful and actionable**.

---

# 🔥 Step-by-Step Strategy

## 🔸 1. Alert Tuning (Primary Fix)

Tune thresholds and conditions to avoid transient spikes.

✔ Improvements:

- Increase threshold (e.g., 80% → 90%)  
- Add duration (avoid instant triggers)  

👉 Example:

- Trigger alert if CPU > 90% for 5 minutes  

💡 Key idea:

- Avoid alerts on short spikes  
- Focus on sustained impact  

---

## 🔸 2. Use Baseline / Relative Alerts

Prefer baseline-based alerts over static thresholds.

👉 Example:

- Normal CPU = 20%  
- Alert if CPU increases significantly and sustains  

✔ Why:

- More adaptive  
- Reduces false positives  

---

## 🔸 3. SLO-Based Alerting (VERY IMPORTANT 🔥)

Alert based on **user impact**, not raw numbers.

❌ Bad:

- Alert if 100 errors (ok when 1000-10000 requests but what if there are 10000000 requests)

✅ Good:

- Alert if error rate > 1% for 5 minutes  

👉 Why:

- Raw numbers lack context  
- Percentages reflect real impact  

---

## 🔸 4. Alert Severity & Routing

Classify alerts based on importance.

👉 Example:

- Critical → PagerDuty (wake up)  
- Warning → Slack / email  

✔ Benefit:

- Avoid unnecessary escalations  
- Reduce fatigue  

---

## 🔸 5. Reduce Duplicate Alerts

Ensure alerts are deduplicated and grouped.

👉 Goal:

- One issue → one alert  
- Not multiple alerts for the same problem  

---

## 🔸 6. Post-Incident Review

Continuously refine alerts after incidents.

✔ Actions:

- Review false alerts  
- Adjust thresholds and conditions  

👉 Shows continuous improvement mindset  

---

# 🔥 Final Answer

To reduce alert noise, I focus on tuning alerts rather than removing them. I adjust thresholds and add duration to prevent alerts from triggering on short spikes. I also use SLO-based alerting, where alerts are based on user impact instead of raw metrics. Additionally, I classify alerts by severity to avoid unnecessary escalations, deduplicate alerts, and continuously refine alert rules based on past incidents.

---

# ⚠️ Common Mistakes

❌ Removing alerts completely  
❌ Only increasing thresholds  
❌ Ignoring duration  
❌ Not considering user impact  

---

# 🔥 Common Follow-ups

## 🎯 Can we eliminate false alerts completely?

No, but we can minimize them through better tuning and alert design.

---

## 🎯 What makes a good alert?

- Actionable  
- Indicates real user impact  
- Requires immediate attention  

---

## 🎯 What is alert fatigue?

Too many false alerts causing engineers to ignore or miss critical alerts.

---

# 🚀 Killer Line

Alerts should wake you up only when users are impacted—not for every small spike.

---

# 🚀 Quick Revision

- Tune thresholds + duration  
- Use SLO-based alerts  
- Add severity levels  
- Reduce duplicates  
- Improve continuously  
