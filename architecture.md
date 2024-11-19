# Coroot Architecture

Coroot is a sophisticated open-source observability platform designed to collect, process, and analyze telemetry data from your systems. Below is a detailed explanation of its components and functionality.

---

## Coroot-Node-Agent

The **Coroot-Node-Agent** is a lightweight, node-level observability agent that leverages **eBPF (extended Berkeley Packet Filter)** technology to efficiently collect data from all containers running on a node.

### Features

- **Data Collection**:
  - **Metrics**:
    - Exposed in Prometheus format for scraping.
    - Can also be sent directly to Coroot using the **Prometheus Remote Write protocol**.
  - **Logs and Traces**:
    - Sent via the **OpenTelemetry protocol (OTLP)** to Coroot.
  - **Profiles**:
    - Sent using a custom **HTTP-based protocol**.

### Deployment

- Requires installation on every node in the cluster for full coverage.
- In **Kubernetes**, deployed as a **DaemonSet**, ensuring that every node has the agent automatically.

---

## Coroot-Cluster-Agent

The **Coroot-Cluster-Agent** is responsible for collecting telemetry data at the cluster level.

### Features

1. **Database Metrics**:

   - Discovers databases (e.g., **Postgres, MySQL, Redis, Memcached, MongoDB**) using Coroot's **Service Map**.
   - Connects to databases using Coroot-provided credentials and collects metrics.
   - Sends these metrics to Coroot using the **Prometheus Remote Write protocol**.

2. **Application Profiling**:

   - Works alongside the node agent's **eBPF-based profiler** to support application-level profiling.
   - Discovers **Go applications** annotated with `coroot.com/profile-scrape` and `coroot.com/profile-port` to collect **CPU** and **memory profiles**.

3. **AWS Integration**:
   - Discovers **RDS** and **ElastiCache** clusters in AWS environments and collects their telemetry data.

---

## OpenTelemetry Integration

Coroot supports the **OpenTelemetry Protocol (OTLP)** for logs and traces, enabling easy integration with applications instrumented using OpenTelemetry SDKs.

### Data Flow Options

1. **Direct Sending**: Applications send telemetry directly to Coroot.
2. **Collector Routing**: Data is routed through the **OpenTelemetry Collector** for preprocessing.

---

## Prometheus Integration

Coroot integrates with **Prometheus**, treating it as a source for metrics while using an optimized caching mechanism.

### Features

1. **Caching**:

   - Coroot maintains an on-disk cache of metrics for faster access, continuously retrieving updates from Prometheus.
   - Prometheus can be configured with a shorter retention period (e.g., a few hours), reducing storage requirements.

2. **Compatibility**:
   - Compatible with any **Prometheus-compatible time series database**, such as **VictoriaMetrics, Thanos**, or **Grafana Mimir**.

---

## ClickHouse Integration

Coroot uses **ClickHouse** for storing:

- Logs
- Traces
- Profiles

### Benefits of ClickHouse

1. **Efficient Compression**:
   - Offers a compression ratio of 10x or more, significantly reducing storage costs.
2. **High Performance**:
   - Handles large volumes of telemetry data efficiently, making it a suitable backend for high-throughput observability needs.

---

## Summary

- **Coroot-Node-Agent**: Node-level data collection using eBPF.
- **Coroot-Cluster-Agent**: Cluster-wide telemetry, database metrics, and profiling.
- **OpenTelemetry**: Logs and traces integration via OTLP.
- **Prometheus**: Metrics storage with caching and time series compatibility.
- **ClickHouse**: Efficient storage for logs, traces, and profiles.
