## Difference Between Monitoring and Observability

### **Monitoring**: `The When`
- **Definition**: Monitoring refers to the process of collecting, analyzing, and using data to ensure the health and performance of your systems. It’s often about tracking specific metrics and setting up alerts when those metrics cross certain thresholds (e.g., CPU usage, memory usage, response time).
  
- **Key Points**:
  - Focuses on predefined metrics (e.g., uptime, error rates, latency).
  - Used to detect when things go wrong or when a system is underperforming.
  - Monitoring systems typically have dashboards that show you what’s happening at a high level.
  - It’s more reactive—alerts and thresholds give you warnings about issues.
  
- **Tools**: Prometheus, Grafana, Nagios, Datadog, New Relic, Zabbix.

### **Observability**: `The Why`
- **Definition**: Observability is a broader concept that allows you to not only see what is happening in your systems but also understand why it's happening. It’s the ability to explore and analyze your system deeply and dynamically, based on the data collected from logs, metrics, and traces.
  
- **Key Points**:
  - Focuses on the ability to query and gain insights into the inner workings of your systems in real time.
  - Involves more than just monitoring—it’s about *investigating* issues, understanding root causes, and getting a holistic view of system behavior.
  - You can proactively investigate any system behavior, not just monitor known metrics.
  
- **Tools**: OpenTelemetry, Jaeger (for tracing), Elasticsearch, Kibana, Splunk, Prometheus (metrics), Loki (logs).

### **Key Differences**:
1. **Purpose**:
   - **Monitoring**: Alerts you when something goes wrong or when thresholds are crossed.
   - **Observability**: Helps you understand *why* something went wrong and lets you dig deeper to troubleshoot issues.

2. **Scope**:
   - **Monitoring**: Limited to a specific set of metrics and checks.
   - **Observability**: Provides full system visibility, combining logs, metrics, and traces for deeper insight.

3. **Data Types**:
   - **Monitoring**: Primarily focused on metrics (e.g., performance, health).
   - **Observability**: Combines metrics, logs, and traces to provide a more comprehensive view.

4. **Approach**:
   - **Monitoring**: Reactive—get alerts when something is wrong.
   - **Observability**: Proactive and exploratory—enables you to understand and fix issues even when they aren’t obvious.

---

### Example in Practice:
- **Monitoring**: You set up a monitor to track CPU usage, and you receive an alert when it hits 90%. It’s useful for being alerted when there's an issue, but it doesn't tell you why the CPU is spiking.
- **Observability**: With observability tools, you can dive deeper. You can check the logs to understand what processes were running at that time, trace requests through the system, and determine if a specific API call caused the spike. This helps you pinpoint the root cause of the problem.
