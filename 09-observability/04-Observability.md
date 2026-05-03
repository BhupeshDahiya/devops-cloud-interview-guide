# 🔹 **Observability**

## ✅ **What is Observability?**

The ability to understand the internal state of a system using:

- **Metrics** → Numbers (CPU, memory, requests)
- **Logs** → Events/messages
- **Traces** → Request flow across services

## ✅ **Why Observability?**
- **Detect system health**
- **Debug production issues**
- **Understand performance bottlenecks**
- **Handle high traffic scenarios**

---

## 🔥 **Real Use Case (Core Story)**

### **Problem:**
Payment service failing under high concurrent requests.

### **Goal:**
Identify:
- Why the service goes down.
- Where latency/bottleneck exists.
- System behavior over time.

---

## 🔹 **Implementation Breakdown**

### 1️⃣ **Logs (First Layer)**

#### **What you did:**
- Asked devs to use a logging framework (e.g., Log4j).
- Enforced log levels:
  - **INFO**
  - **DEBUG**
  - **ERROR**
  - **TRACE**

#### **Stack:**
- **ELK Stack**:
  - **Elasticsearch** → Storage
  - **Logstash** → Pipeline
  - **Kibana** → Visualization

#### **Purpose:**
- **Debug issues using timestamps.**
- **Understand failures quickly.**

---

### 2️⃣ **Metrics (Second Layer)**

#### **Stack:**
- **Prometheus** → Scraping
- **Grafana** → Dashboards
- **Node Exporter** → Infra metrics

#### **What you tracked:**
- **CPU usage**
- **Memory usage**
- **Traffic patterns**

#### **Custom Metrics (Important 🔥):**
- **Total payment requests**
- **Request spikes**

#### **👉 Developers instrumented metrics using OpenTelemetry**

---

### 3️⃣ **Tracing (Advanced Layer)**

#### **Tool:**
- **Jaeger**

#### **Purpose:**
- **Track the full request journey:**
  - User → Payment service → DB → Other services
- **Identify:**
  - Latency
  - Bottlenecks
  - Slow dependencies

---

## 🔥 **Key Insight (Say This in Interview)**

Logs help with debugging, metrics help with trends, and traces help with request flow analysis.

---

## 🎯 **STAR FORMAT ANSWER (Highly Important)**

### **⭐ Question:**
**“Have you worked on observability?”**

#### **✅ Answer**

**Situation:**
In our system, we had a payment service that was critical, and it started failing under high concurrent user requests.

**Task:**
My task was to identify the root cause and ensure we had better visibility into system behavior.

**Action:**
I implemented observability using three pillars: logs, metrics, and traces.

- For logs, I standardized logging with proper levels and centralized logs using ELK stack.

- For metrics, I set up Prometheus and Grafana, and also worked with developers to expose custom metrics like total payment requests using OpenTelemetry.

- For tracing, I implemented distributed tracing using Jaeger to understand request flow and identify latency between services.

**Result:**
This helped us identify bottlenecks and understand traffic patterns. We were able to troubleshoot issues faster and improve system reliability.

---

### 💬 **Real-sounding closing line:**

After this, debugging became much faster and we had clear visibility into system behavior under load.

---

## 🔥 **Tough Follow-ups (VERY IMPORTANT)**

### 🎯 **Q1: “Why not just logs? Why metrics & tracing?”**
**✅ Answer:**
Logs are useful for debugging specific issues, but they don’t give trends or system-wide insights. Metrics help with monitoring and alerting, while tracing helps identify latency across microservices.

### 🎯 **Q2: “What custom metrics did you create?”**
**✅ Answer:**
One key metric was total number of payment requests. This helped correlate traffic spikes with failures.

### 🎯 **Q3: “How does Prometheus get data?”**
**✅ Answer:**
Prometheus pulls metrics from targets using HTTP endpoints, typically `/metrics`, exposed by exporters or applications.

### 🎯 **Q4: “What problem did tracing solve?”**
**✅ Answer:**
Tracing helped us identify latency between services, especially when the payment service was calling DB or other microservices.
