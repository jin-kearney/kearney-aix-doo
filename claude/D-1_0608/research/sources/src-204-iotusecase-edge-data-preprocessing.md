# src-204: Edge Computing & Data Preprocessing — Proven IIoT Solutions

**URL:** https://www.iotusecase.com/en/building-blocks/data-preprocessing
**Title:** Edge Computing & Data Preprocessing — Proven IIoT Solutions
**AccessDate:** 2026-06-09
**Author:** IoT Use Case
**Related section:** Edge computing's role in data prep

---

## Key Excerpts

**Definition:**
Industrial data preprocessing = process of filtering, aggregating, and transforming raw data directly at the source — before it is transmitted. Edge devices process data in real time — from simple filtering/averaging to complex frequency spectra analysis in vibration data.

**Concrete Processing Steps at the Edge:**
- Filtering and aggregation: averages, min/max, or sums over time windows — only meaningful information leaves the edge
- Outlier detection: anomalous measured values detected and filtered at the edge or flagged separately — before they distort analyses or trigger false alarms
- Protocol conversion: raw data from OPC UA, Modbus, MQTT, or proprietary protocols converted into a uniform format
- Local control loops and alerting: time-critical responses (shutdown commands, warning messages) triggered directly at the edge in milliseconds — no cloud detour
- Data enrichment and contextualization: raw data enriched with metadata (timestamp, machine ID, shift information)
- Containerized edge deployments: Docker and similar technologies for standardized, updatable edge deployments without production downtime

**Why Edge Preprocessing is Difficult:**
- Limited processing power of existing PLCs and older controllers (insufficient memory, outdated hardware)
- Uncontrolled data volumes: sensors continuously generate enormous data volumes — without preprocessing, networks become overloaded
- Lack of real-time capability: response times of seconds not tolerable in production for control loops/alerting
- Heterogeneous data formats: different machines, protocols, formats must be converted into a uniform schema
- Data privacy/sovereignty: sensitive production data should not leave own infrastructure

**Measurable Results from Edge-Based Preprocessing:**
- Real-time processing for faster decisions
- Significantly reduced transmission costs (only relevant information forwarded)
- Better data quality for more precise analyses
- Relief for central IT systems
- Increased data security and compliance
- Automation of complex data preparation processes
