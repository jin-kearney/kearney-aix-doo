# src-410 — MDPI Sensors: From Sensors to Data Intelligence: Leveraging IoT, Cloud, and Edge Computing with AI

**URL:** https://www.mdpi.com/1424-8220/25/6/1763
**Title:** From Sensors to Data Intelligence: Leveraging IoT, Cloud, and Edge Computing with AI
**AccessDate:** 2026-06-09
**Related section:** KPI (data latency edge-to-platform), Roadmap (edge-cloud architecture)

---

## Key Excerpts

> Key performance indicators (KPIs) may be used to identify where sensor data is best transferred and where it is processed or stored.

> Edge computing has lower latencies compared to core network and cloud data centers, which have latencies of 50–100 ms or more. Operations at a cloud data center with such latencies may not be able to accomplish many time-critical functions.

> Edge calculations can provide computed values with low latency and independent of internet connection status to on-premise systems such as Manufacturing Execution Systems (MES).

## Latency Benchmarks

| Processing layer | Typical latency |
|---|---|
| On-device (sensor edge) | < 1 ms |
| Local edge gateway | 1–10 ms |
| On-premise server | 10–50 ms |
| Cloud data center | 50–100+ ms |

## Relevance to KPI: Data Latency (Edge-to-Platform)

- Measured as: time from sensor event occurrence to availability in the data platform (historian/data lake)
- Target: < 100 ms for real-time control applications; < 1 s for most AI inference; < 5 min for batch analytics
- Direction: ↓ (lower is better)

## Architecture Pattern

Sensors → Edge Gateway (filter/aggregate/validate) → Cloud Ingestion Layer (clean/enrich) → Data Lake + Real-Time Analytics
