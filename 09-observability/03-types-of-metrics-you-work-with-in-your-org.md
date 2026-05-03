## Types of Metrics in Prometheus for DevOps Monitoring (Including Kubernetes)

When working with **Prometheus** in a **DevOps** role, you will typically handle a range of metrics that provide insights into application health, system performance, and infrastructure. These metrics can be broadly categorized into **application metrics**, **system metrics**, **infrastructure metrics**, and **Kubernetes (K8s) metrics**.

---

### **1. Application Metrics**

These metrics are emitted by your application to measure performance, health, and business operations.

#### **a. Request Metrics**:
- **http_requests_total**: Counter metric for the total number of HTTP requests to the application, often labeled by `status_code`, `method`, and `endpoint`.
  
  Example:
  `http_requests_total{status_code="200", method="GET", endpoint="/api/users"} 150`

- **http_request_duration_seconds**: Histogram metric to track the duration of HTTP requests (e.g., time taken for each API request).
  
  Example:
  `http_request_duration_seconds{method="GET", endpoint="/api/users"} 0.75`

#### **b. Error Metrics**:
- **http_requests_failed_total**: Counter metric that counts the number of failed requests (e.g., 500 errors).
  
  Example:
  `http_requests_failed_total{status_code="500", method="POST", endpoint="/api/login"} 10`

#### **c. Latency Metrics**:
- **request_duration_seconds**: Histogram metric for the latency of a specific request in the application.

  Example:
  `request_duration_seconds_bucket{le="0.1", endpoint="/api/orders"} 234`

#### **d. Custom Business Metrics**:
- **user_signups_total**: Counter to track the number of user signups.
  
  Example:
  `user_signups_total{platform="mobile"} 12345`

- **order_payment_amount_sum**: Gauge metric for tracking the total amount of payments processed.

  Example:
  `order_payment_amount_sum{currency="USD"} 5000`

---

### **2. System Metrics**

These metrics reflect the health of the system (e.g., server, container, or VM) on which your application is running.

#### **a. CPU Usage**:
- **process_cpu_seconds_total**: Counter metric that tracks CPU time consumed by your application.
  
  Example:
  `process_cpu_seconds_total{job="app", instance="app-server-1"} 200`

- **node_cpu_seconds_total**: Counter metric tracking the total CPU time on the node (host machine).
  
  Example:
  `node_cpu_seconds_total{mode="user", cpu="0"} 1000`

#### **b. Memory Usage**:
- **process_resident_memory_bytes**: Gauge metric tracking memory usage by the application in bytes.
  
  Example:
  `process_resident_memory_bytes{job="app", instance="app-server-1"} 150000000`

- **node_memory_MemAvailable_bytes**: Gauge metric for available memory on the node.
  
  Example:
  `node_memory_MemAvailable_bytes{instance="app-server-1"} 8000000000`

#### **c. Disk Usage**:
- **node_disk_io_time_seconds_total**: Counter metric for disk I/O time on the node.

  Example:
  `node_disk_io_time_seconds_total{device="sda"} 200`

- **node_filesystem_avail_bytes**: Gauge metric for available space on the filesystem.

  Example:
  `node_filesystem_avail_bytes{mountpoint="/", device="sda1"} 5000000000`

---

### **3. Container Metrics (Kubernetes)**

Prometheus can also collect **container-specific metrics** when running in environments like Kubernetes or Docker.

#### **a. Container CPU and Memory**:
- **container_cpu_usage_seconds_total**: Counter metric that tracks CPU usage by a container.
  
  Example:
  `container_cpu_usage_seconds_total{container="my-app", pod="my-app-pod"} 100`

- **container_memory_usage_bytes**: Gauge metric for memory usage by a container.
  
  Example:
  `container_memory_usage_bytes{container="my-app", pod="my-app-pod"} 150000000`

#### **b. Container Network Metrics**:
- **container_network_transmit_bytes_total**: Counter metric for bytes transmitted over the network by a container.
  
  Example:
  `container_network_transmit_bytes_total{container="my-app", pod="my-app-pod"} 50000`

- **container_network_receive_bytes_total**: Counter metric for bytes received by a container.
  
  Example:
  `container_network_receive_bytes_total{container="my-app", pod="my-app-pod"} 40000`

---

### **4. Kubernetes (K8s) Metrics**

Kubernetes environments offer specific metrics for clusters, nodes, pods, and containers. These are essential for monitoring the health and performance of your Kubernetes-based applications.

#### **a. Node Metrics**:
- **kube_node_status_capacity_cpu_cores**: Gauge metric that tracks the total number of CPU cores on a Kubernetes node.
  
  Example:
  `kube_node_status_capacity_cpu_cores{node="node-1"} 16`

- **kube_node_status_capacity_memory_bytes**: Gauge metric for total memory capacity of a Kubernetes node.
  
  Example:
  `kube_node_status_capacity_memory_bytes{node="node-1"} 32000000000`

#### **b. Pod Metrics**:
- **kube_pod_container_resource_requests_cpu_cores**: Gauge metric tracking the CPU resources requested by a container in a pod.
  
  Example:
  `kube_pod_container_resource_requests_cpu_cores{pod="my-pod", container="my-container"} 1`

- **kube_pod_container_resource_requests_memory_bytes**: Gauge metric for memory requested by a container in a pod.
  
  Example:
  `kube_pod_container_resource_requests_memory_bytes{pod="my-pod", container="my-container"} 512000000`

#### **c. Kubernetes Cluster Metrics**:
- **kube_pod_status_phase**: Gauge metric for tracking the status of pods in the cluster (e.g., running, pending, failed).
  
  Example:
  `kube_pod_status_phase{pod="my-pod", phase="running"} 1`

- **kube_deployment_status_replicas**: Gauge metric tracking the number of replicas in a Kubernetes deployment.
  
  Example:
  `kube_deployment_status_replicas{deployment="my-deployment"} 5`

#### **d. Kubernetes Network Metrics**:
- **kube_pod_network_receive_bytes_total**: Counter metric for bytes received by pods over the network.
  
  Example:
  `kube_pod_network_receive_bytes_total{pod="my-pod"} 200000`

- **kube_pod_network_transmit_bytes_total**: Counter metric for bytes transmitted by pods over the network.
  
  Example:
  `kube_pod_network_transmit_bytes_total{pod="my-pod"} 150000`

---

### **5. Infrastructure Metrics**

These metrics provide insights into the underlying infrastructure supporting your application.

#### **a. Database Metrics**:
- **postgresql_connections**: Gauge metric for the number of active PostgreSQL database connections.
  
  Example:
  `postgresql_connections{database="mydb"} 50`

- **mysql_up**: Gauge metric indicating whether a MySQL instance is up (1) or down (0).
  
  Example:
  `mysql_up{instance="mysql-db"} 1`

#### **b. Load Balancer Metrics**:
- **nginx_upstream_response_time_seconds**: Histogram for tracking response times from upstream servers behind an Nginx load balancer.

  Example:
  `nginx_upstream_response_time_seconds{upstream="backend"} 0.3`

---

### **6. Health & Availability Metrics**

These metrics monitor the overall health and availability of your services.

#### **a. Service Health**:
- **service_up**: Gauge metric indicating whether a service is up (1) or down (0).
  
  Example:
  `service_up{service="user-service"} 1`

- **application_health_status**: Custom gauge metric indicating the health status of an application (1 for healthy, 0 for unhealthy).
  
  Example:
  `application_health_status{app="my-app"} 1`

---

### **Conclusion**

Prometheus allows you to track a wide range of metrics, from **application-specific** metrics like request counts and latency, to **system-level** metrics for CPU, memory, and disk usage, and **Kubernetes-specific** metrics for cluster, pod, and node performance. As a **DevOps engineer**, you will configure Prometheus to collect, aggregate, and visualize these metrics, set up alerting, and ensure that the monitoring system is properly optimized for performance and reliability.
