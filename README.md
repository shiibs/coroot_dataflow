# Coroot Data Flow and Agent Functionality

## Data Flow in Coroot

Coroot's data flow can be categorized into three main stages: **Data Collection**, **Aggregation and Processing**, and **Visualization**.

---

### 1. Data Collection

Coroot collects data from multiple sources across the infrastructure, using its agents and integrations. The sources include nodes, clusters, PostgreSQL databases, and Prometheus-exported metrics.

#### 1.1 Node Agent

- **Purpose**: Collects low-level system metrics and logs directly from individual nodes.
- **Mechanism**:
  - Leverages **eBPF (Extended Berkeley Packet Filter)** for lightweight, high-performance system data collection.
  - Captures data such as CPU usage, memory utilization, disk I/O, and network traffic.
- **Output**: The Node Agent sends this data to the Coroot Server for aggregation.

#### 1.2 Cluster Agent

- **Purpose**: Collects cluster-level metrics from orchestrators like Kubernetes.
- **Mechanism**:
  - Queries the Kubernetes API for pod status, resource usage (CPU, memory, disk), and node health.
  - Monitors service status, deployments, and events in the cluster.
- **Output**: The Cluster Agent forwards the collected metrics and event data to the Coroot Server.

#### 1.3 Prometheus Integration

- **Purpose**: Retrieves application and exporter metrics from Prometheus.
- **Mechanism**:
  - Queries Prometheus servers to fetch metrics such as application health, container statistics, and node metrics.
  - Supports integration with popular exporters like **cAdvisor** and **Node Exporter**.
- **Output**: The Coroot Server ingests these metrics for unified analysis.

##### cAdvisor

- **cAdvisor (Container Advisor)**: An open-source tool that provides container users with resource usage and performance characteristics. It collects, aggregates, processes, and exports information about running containers.

##### Node Exporter

- **Node Exporter**: A Prometheus exporter for hardware and OS metrics. It allows Prometheus to scrape system-level metrics like CPU, memory, disk, and network statistics.

#### 1.4 Coroot PG Agent

- **Purpose**: Gathers metrics from PostgreSQL databases.
- **Mechanism**:
  - Monitors database-specific performance metrics like query execution time, connection counts, query throughput, and memory usage.
  - Collects logs and other operational data from PostgreSQL instances.
- **Output**: Sends collected PostgreSQL data to the Coroot Server.

---

### 2. Data Aggregation and Processing

Once collected, the data flows into the Coroot Server for processing and aggregation.

#### 2.1 Aggregation

- The Coroot Server aggregates data from multiple sources:
  - **Node-level metrics** from Node Agents.
  - **Cluster-level metrics** from Cluster Agents.
  - **Application metrics** from Prometheus exporters.
  - **Database metrics** from the Coroot PG Agent.

#### 2.2 Storage

- Data is stored in **ClickHouse**, a high-performance time-series database, which ensures:
  - Efficient storage and retrieval of large volumes of time-series metrics, logs, and traces.
  - Scalability to handle growing datasets over time.

#### 2.3 Processing

- Collected data is processed through **Collectors** and **Watchers**:
  - **Collectors**: Transform raw metrics into actionable insights, such as average resource utilization or error rates.
  - **Watchers**: Continuously monitor data for events like SLO violations, unexpected deployments, or abnormal trends. Triggers actions or alerts based on predefined rules.

---

### 3. Data Visualization

The Coroot Server processes and makes data accessible via an intuitive web interface.

- **Dashboards**:  
   Interactive dashboards visualize:
  - Metrics and logs for root cause analysis.
  - Traces for application performance monitoring.
  - SLO compliance reports and cost optimization insights.
- **Service Maps**:  
   Illustrate dependencies between components and highlight potential bottlenecks.
- **Real-Time Analysis**:  
   Provides live insights and allows for historical data exploration using ClickHouse queries.
