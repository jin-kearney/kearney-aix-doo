# D-1 Physical Data — Research Notes: What (Core) Cluster
**Cluster:** What (core) — definition / scope / key components / data-collection architecture / standards / data flow / glossary
**Audience:** Manufacturing — sensors, edge computing, time-series, OT/IT convergence
**Mindset:** NOT "building AI" — preparing/curating physical/sensor data FOR AI
**Date:** 2026-06-09
**Source IDs:** src-101 through src-112

---

## 1. Definition and Scope of Physical Data

Physical Data refers to all data that originates from the physical world of a manufacturing facility — specifically from sensors, equipment (machines, motors, pumps, conveyors), and field/shop-floor systems. It is the raw signal layer that tells AI systems what is actually happening on the factory floor at any given moment.

**What counts as Physical Data (scope boundary):**

| Data Type | Examples | System Source |
|---|---|---|
| Sensor readings | Vibration (Hz, g), temperature (°C), pressure (bar/kPa), flow rate, humidity, current (A), voltage (V) | IoT sensors, transducers |
| Equipment status | Running/stopped/fault, RPM, torque, cycle count, power consumption | PLC registers, motor drives |
| PLC tag data | Digital I/O states, analog values, set-points, actual vs. set-point deltas | PLC/DCS systems |
| MES data | Work order status, production counts, cycle times, downtime reason codes | MES (Level 3) |
| QMS/inspection data | Dimensional measurements, pass/fail flags, CPK values, vision system results | Inspection equipment, gauges |
| Alarm and event logs | Alarm IDs, timestamps, duration, severity, operator acknowledgment | SCADA alarm historian |
| Operating conditions | Operating hours, boot cycle counters, environmental data (ambient temp, humidity) | Environmental sensors, CMMS |
| SCADA data | Process variables, control loops, supervisory overrides, HMI events | SCADA systems |

**What is NOT in scope for D-1:** Business transaction data (ERP orders, invoices), purely IT-generated data (user logs, network traffic), CAD/design data, material composition databases. The scope boundary is sensors and equipment only — Levels 0–2 of the ISA-95 hierarchy plus the MES data that represents those physical processes.

**Key analogy for easy understanding:**
Physical Data is like the nervous system of a factory — sensors are the nerve endings, edge devices are the peripheral nerves, and the data lake/IoT platform is the brain. Without a healthy, well-structured nervous system, no AI can function. A doctor cannot diagnose a patient from nerve signals that have wrong timestamps, inconsistent units, or missing context. The same is true for AI on the factory floor.

---

## 2. Why Physical Data Is the Foundation for Manufacturing AI

From Gartner (source: src-107, Gartner G00841619, December 2025):
- Over 50% of AI projects are failing to reach production
- Data issues are blocking 40% of initiatives
- 60% of AI projects will be abandoned because they lack AI-ready data

The core problem is not the AI model. It is the data foundation beneath it. Gartner identifies **three building blocks** every manufacturing CIO must have in place before AI can scale:
1. **Data Curation** via Unified Namespaces and OPC UA
2. **AI Data Preparation and Delivery** via MQTT and metadata management
3. **OT/IT Metadata Management** — active, continuously updated, enriched metadata

Raw sensor data from the factory floor is high-volume, unstructured, and inconsistent across lines, sites, and machine generations. Without data curation, AI models work from noise rather than signal. [src-107]

---

## 3. Key Components of Physical Data Collection Architecture

The architecture follows a five-layer flow: **Sensors → Edge Devices → IoT Gateway/Platform → MES/QMS → Data Lake**

### Layer 1: Sensors and Field Instrumentation (ISA-95 Level 0–1)
Sensors are the origin points of all physical data. They convert physical phenomena into electrical signals.

**Common sensor types in manufacturing:**
- **Vibration sensors** (accelerometers) — placed on bearing housings and motor frames; detect mechanical stress; primary input for predictive maintenance AI [src-111]
- **Temperature sensors** (thermocouples, RTDs, infrared) — winding temperature, bearing temperature, process temperature; track thermal variation
- **Pressure sensors** — hydraulic and pneumatic systems; process pressure monitoring
- **Current/voltage sensors** — monitor motor circuits; detect electrical anomalies
- **Flow sensors** — liquid/gas flow measurement in process manufacturing
- **Proximity/position sensors** — detect machine states (open/closed, present/absent)
- **Vision systems** — dimensional inspection, surface defect detection; generate structured pass/fail + measurement data
- **Environmental sensors** — ambient temperature, humidity, particulate count (clean rooms, paint shops)

**IO-Link sensors** (IEC 61131-9) represent the modern generation — they communicate not just the measurement value but also diagnostic data (sensor health, calibration status, operating hours) over a single cable. [src-111]

### Layer 2: Edge Devices / Industrial Controllers (ISA-95 Level 1–2)

**PLC (Programmable Logic Controller):**
The primary controller on the shop floor. PLCs store data as "tags" — each tag represents a specific value from a specific sensor/actuator. Key tag attributes:
- Measurement value (actual reading from sensor at a timestamp)
- Quality/status indicator (valid, sensor error, communication failure, manual entry)
- Engineering unit (°C, bar, RPM, etc.)
- Data source (specific sensor or device ID)
- Location (machine, line, cell)
- Alarm/event information [src-111]

**SCADA (Supervisory Control and Data Acquisition):**
Operates at Level 2. Polls PLCs at configurable intervals (typically 100ms–10s), aggregates tag data, generates alarm logs, and presents to operators. SCADA systems include a built-in data historian (legacy PI System/AVEVA PI being the dominant industrial standard).

**Edge Computing devices:**
Modern edge devices (e.g., industrial PCs running Litmus Edge, AWS IoT Greengrass, or Siemens Industrial Edge) sit between PLCs/SCADA and the cloud. They perform:
- **Protocol translation**: Convert Modbus RTU/TCP, OPC UA, Profinet, EtherNet/IP to MQTT/HTTP
- **Data normalization**: Standardize units, naming conventions, timestamp format
- **Filtering**: Suppress unchanged values (dead-banding), reduce data volume
- **Pre-processing**: Calculate derived metrics, detect anomalies
- **Local buffering**: Store data during network outages; forward with historical flags on reconnect [src-108]

### Layer 3: IoT Gateway / Messaging Layer

**OPC UA (OPC Unified Architecture):**
The primary standard for semantic machine data communication. Published by the OPC Foundation. Key properties:
- **Information model**: Hierarchical, object-oriented representation of industrial assets. Each node represents an entity (device, sensor, variable) with attributes (value, engineering unit, quality, timestamp)
- **Address Space**: Virtual representation of all industrial assets. Clients browse and subscribe to nodes
- **Service Sets**: Discovery, Session, NodeManagement, View, Attribute (read/write/subscribe), Method, Subscription
- **Base Models**: DataAccess (DA) for real-time values; Alarm & Conditions (A&C) for events; Historical Access (HA) for historian queries
- **Companion Specifications**: Domain-specific extensions — OPC UA for Devices (DI), for Robotics (ROB), for PLCopen, for ISA-95/B2MML [src-101, src-110]
- **Key capability for AI**: Unlike raw Modbus or proprietary protocols, OPC UA carries not just a value but the full semantic context — engineering unit, measurement range, quality indicator, equipment hierarchy. A pressure sensor node self-describes its unit (bar), range (0–20 bar), sensor type, and serial number, in addition to the current reading [src-101]

**MQTT / Sparkplug B:**
MQTT is the lightweight publish-subscribe protocol most widely used for IoT data transport, especially in constrained or bandwidth-limited environments. MQTT alone does not define data structure — this is where Sparkplug B adds value.

Sparkplug B (Eclipse Foundation, governed by EFSP) extends MQTT 3.1.1 with:
- **Structured topic namespace**: `spBv1.0/{group_id}/{message_type}/{edge_node_id}/{device_id}` — eliminates vendor-specific chaos where Machine A uses `factory/line1/temp`, Machine B uses `sensors/machineB/env`, and Machine C uses `data/mc/status` [src-102]
- **Google Protocol Buffer payloads**: Binary, compact, strongly typed — significantly smaller than JSON; supports Int8–Int64, Float, Double, Boolean, String, Timestamp, UUID, Binary, Datasets, Templates
- **Birth/Death certificates**: NBIRTH/DBIRTH announce all available metrics and their types when a device connects. NDEATH/DDEATH signal disconnections (using MQTT Last Will and Testament). This self-description enables automatic device discovery without external configuration files
- **Sequence numbers**: Enable detection of lost messages
- **Timestamps per metric**: Timestamp of capture, not transmission — critical for AI training
- **Quality flags**: Historical (store-and-forward), Transient (not to be stored), Null (sensor failure)
- **Metric aliasing**: Short numeric aliases replace full metric names in recurring messages, reducing bandwidth [src-102]

**The industrial integration problem without Sparkplug B:**
Every machine manufacturer implements MQTT differently. Each device requires custom parsing logic, error handling, and documentation. Integration complexity grows exponentially with each new device type. With Sparkplug B, all devices publish to structured topics with consistent Protocol Buffer payloads — integration becomes predictable and maintainable. [src-102]

### Layer 4: MES / QMS (ISA-95 Level 3)

**MES (Manufacturing Execution System):**
Sits at ISA-95 Level 3. Collects and contextualizes production data — work orders, schedules, production counts, downtime reason codes, OEE metrics. MES bridges the gap between shop-floor reality (PLC/SCADA data) and business planning (ERP). MES data represents physical process outcomes in structured, human-meaningful form.

**B2MML (Business to Manufacturing Markup Language):**
The XML schema that implements ISA-95 data objects for ERP↔MES communication. B2MML defines standard objects for production orders, work schedules, material lots, equipment capability, and performance responses. OPC UA carries the Level 2→3 conversation; B2MML carries the Level 3→4 conversation (orders down, performance up). [src-103, src-112]

### Layer 5: Data Lake / Data Platform

The final destination for physical data, where it becomes available for AI training and analytics. Modern platforms (AWS IoT SiteWise, Azure IoT Operations, Siemens Insights Hub) store physical data in:
- **Time-series databases** (InfluxDB, TimescaleDB, TDengine, AVEVA PI) — optimized for high-frequency writes, temporal queries, downsampling
- **Data lakes** (S3, Azure Data Lake, ADLS) — raw data archive, long-term retention
- **Feature stores** — pre-computed ML features from sensor data

---

## 4. The ISA-95 Hierarchy and Data Flow

ISA-95 (ANSI/ISA-95, also IEC 62264) is the global standard defining how business systems and the shop floor exchange information. It structures manufacturing IT into **five levels** [src-103, src-112]:

| Level | Name | Systems | Data Examples |
|---|---|---|---|
| 0 | Physical Process | Sensors, actuators, machines | Raw analog/digital signals |
| 1 | Sensing & Manipulation | Transducers, I/O modules, PLCs | Sensor readings, I/O states |
| 2 | Control | PLCs, DCS, SCADA | Tag values, set-points, alarm logs |
| 3 | Manufacturing Operations | MES, CMMS, QMS, Lab systems | Production counts, OEE, quality results |
| 4 | Business Planning | ERP, MRP, SCM | Orders, schedules, inventory, KPIs |

**IT/OT Convergence:** Traditional manufacturing has kept OT (Levels 0–3) and IT (Level 4) separate — different protocols, teams, technologies, and security models. IT/OT convergence means creating data flows and governance structures that allow physical data to be consumed by IT systems, cloud analytics, and AI — without compromising the real-time determinism and security of OT systems. Level 3 (MES) is the critical bridge. [src-103, src-112]

**ISA-95 Data Flow Challenge:**
In traditional ISA-95 implementations, data moves strictly up the hierarchy one level at a time. Production data from Level 0–2 rarely reaches Level 4. When it does, it is delayed, inaccurate, and stripped of context. This is the core reason AI projects fail — the training data is insufficient, stale, or decontextualized. [src-103]

---

## 5. Unified Namespace (UNS) — The Modern Architecture

The **Unified Namespace (UNS)** replaces the rigid ISA-95 point-to-point hierarchy with a publish-subscribe data fabric where every device, application, and system publishes data to and consumes data from a central hub (typically an MQTT broker). [src-104]

**Definition (Walker Reynolds, 2005):** UNS is not just a naming convention — it is an architecture meeting five requirements:
1. Semantic hierarchy structure of business data and events
2. Hub connecting all smart devices and IT infrastructure
3. Single source of truth for all data and information
4. Foundation of digital transformation
5. Where the current state of business lives (real-time snapshot)

**UNS vs. Traditional ISA-95 (Industry 3.0) Architecture:**

| | Traditional (ISA-95 Hierarchy) | Unified Namespace |
|---|---|---|
| Data flow | Sequential, layer-by-layer | Any producer to any consumer |
| Integration | Point-to-point, custom connectors | Plug-and-play via broker |
| Real-time | No — data delayed moving up hierarchy | Yes — current state always available |
| AI/ML suitability | Poor — data silos, no unified dataset | High — all data accessible |
| Scalability | Complex — n² connections | Linear — all connect to one hub |

**UNS Layers (bottom to top):**
1. Physical devices (HMI/PLC/sensors) — each tag or device is a namespace
2. Data acquisition layer — industrial gateways (NeuronEX, Litmus, HighByte) collect and normalize
3. Manufacturing Execution Layer (MES)
4. ERP layer
5. Business Intelligence / Cloud Analytics [src-104]

**4 Types of Namespaces within UNS:**
- **Functional**: grouped by purpose (production OEE, maintenance, quality) — used for OEE measurement across Availability × Performance × Quality
- **Informative**: grouped by data type (all temperature readings across the plant)
- **Definitional**: grouped by asset type (all motors, all sensors of type X)
- **Ad Hoc**: temporary groupings for urgent specific analysis [src-104]

---

## 6. Time-Series Data Model — The Native Format for Physical Data

All physical data is inherently time-series data. Every sensor reading carries a timestamp. Time-series data is the "shared context" that makes AI able to correlate readings across sensors, detect trends, and build predictive models. [src-105]

**Why time-series is different from transactional data:**
- Continuous writes at high frequency (sub-second to seconds)
- No natural batching boundaries
- Schema evolves as new sensors are added
- Temporal ordering is critical — milliseconds apart may need different handling
- Historical data needed for AI training: 5–10 years, no downsampling [src-105, src-108]

**Time-Series Data Schema fields (per data point):**
- `timestamp` — ISO 8601 or Unix epoch (nanosecond precision for high-frequency)
- `measurement` — the metric name (tag_id, sensor_id)
- `value` — numeric (float, int, boolean)
- `quality` — validity flag (good, bad, uncertain, historical, null/sensor_failure)
- `unit` — engineering unit (bar, °C, RPM, m/s²)
- `asset_id` — equipment/machine identifier
- `location` — site, line, cell, equipment
- `tags` — key-value metadata (equipment_type, manufacturer, model)

**Why legacy data historians fail for AI:**
Legacy SCADA historians (OSI PI / AVEVA PI, etc.) were designed for compliance and operator monitoring — not AI training. They are Windows-only, proprietary, expensive to scale, have no open APIs, and cannot integrate with modern cloud or open-source tools. A manufacturing facility can generate 1–10 TB of time-series data per day; legacy historians were built for megabytes. [src-105]

**Modern time-series databases for IIoT:**
- **InfluxDB 3** (FDAP stack: Flight, DataFusion, Arrow, Parquet) — millions of points/second ingestion; native MQTT support; edge agents; AWS/cloud integration [src-108]
- **TimescaleDB** — PostgreSQL extension; strong for SQL analytics teams
- **TDengine** — open-source, 10x compression; extreme cost sensitivity
- **AVEVA PI** — enterprise historian; legacy standard in process industries; being displaced by open alternatives [src-108]

**Storage tiering for physical data:**
- Hot (NVMe, <7 days): <1ms latency — real-time dashboards, edge decisions
- Warm (SSD, 7–90 days): <10ms — operational analytics, short-term AI inference
- Cold (HDD, 90–365 days): <100ms — historical analysis
- Archive (S3/object storage, >365 days): minutes — AI training data, compliance [src-108]

---

## 7. Key Industrial Standards Summary

| Standard | Full Name | Governing Body | Scope/Role for Physical Data |
|---|---|---|---|
| OPC UA | OPC Unified Architecture | OPC Foundation (opcfoundation.org) | Semantic machine data communication; information model; companion specs; covers DA, A&C, HA services |
| MQTT 3.1.1 / MQTT 5.0 | Message Queuing Telemetry Transport | OASIS Standard | Lightweight publish-subscribe transport; used for IoT edge-to-cloud messaging |
| Sparkplug B | MQTT Sparkplug B Specification | Eclipse Foundation (EFSP) | Industrial-specific MQTT extension with structured topic namespace, Protobuf payloads, birth/death certificates, metric aliasing |
| ISA-95 / IEC 62264 | Enterprise-Control System Integration | ISA (International Society of Automation) / IEC | Five-level hierarchy model; defines data objects for ERP↔MES integration; IT/OT convergence framework; latest version: ANSI/ISA 95.00.01-2025 |
| ISA-88 / IEC 61512 | Batch Control | ISA / IEC | Batch manufacturing data model; complements ISA-95 for process industries |
| B2MML | Business To Manufacturing Markup Language | MESA International / WBF | XML schema implementing ISA-95 data objects for ERP↔MES communication |
| IEC 61131 | Programmable Controllers | IEC | PLC programming standard; IEC 61131-3 defines PLC languages; IEC 61131-9 defines IO-Link sensor communication |
| IO-Link | IO-Link | IO-Link Consortium | Point-to-point sensor communication standard; smart sensors report value + diagnostics |
| Modbus | Modbus Protocol | Modbus Organization | Legacy serial/TCP fieldbus; widely deployed in legacy PLCs; no semantics — value only |

---

## 8. Data Flow — End-to-End Physical Data Pipeline for AI

```
[Sensors / Field Instruments]         Layer 0-1 (Physical + Sensing)
        ↓  (analog/digital signal, IO-Link, Modbus RTU)
[PLCs / DCS / SCADA]                   Layer 2 (Control)
   - PLC tags: value + timestamp + quality + unit
   - SCADA alarm/event logs
        ↓  (OPC UA, Modbus TCP, Profinet, EtherNet/IP)
[Edge Gateway / Industrial IoT Hub]    Layer 2-3 bridge
   - Protocol translation → MQTT / Sparkplug B
   - Data normalization: units, naming conventions
   - Dead-banding: filter unchanged values
   - Local buffering: store-and-forward during outage
   - Edge validation: range checks, rate-of-change anomalies
        ↓  (MQTT/Sparkplug B, OPC UA over internet, HTTPS)
[IoT Platform / Message Broker]        Layer 3 (MES/Operations)
   - Unified Namespace (MQTT broker, e.g., HiveMQ, EMQX)
   - Topic structure: spBv1.0/{group}/{type}/{edge}/{device}
   - MES context overlay: work order, product, shift, machine state
        ↓  (streaming: Kafka, Kinesis, Event Hub)
[Time-Series Database + Data Lake]     Storage layer
   - TSDB: InfluxDB/TimescaleDB for operational queries
   - Data Lake: S3/ADLS for AI training data archive
   - Asset model: ISA-95 equipment hierarchy
        ↓  (feature extraction, ML pipelines)
[AI / Analytics Applications]         Consumption layer
   - Predictive maintenance: vibration + temp + current → RUL estimation
   - Anomaly detection: multivariate sensor patterns
   - OEE optimization: production count + cycle time + quality flags
   - Process optimization: set-point + actual + quality → parameter tuning
```

**Data preparation steps at each stage for AI readiness:**
1. **At sensor**: Ensure calibration, IO-Link diagnostics enabled, correct engineering unit configured
2. **At PLC/SCADA**: Consistent tag naming convention (ISA S88 tag format: area_equipment_instrument_type), alarm priority scheme
3. **At edge**: Timestamp normalization (UTC, NTP synchronized), unit standardization, dead-banding tuning
4. **At broker/UNS**: Sparkplug B NBIRTH/DBIRTH ensures self-describing data; metadata enrichment (asset hierarchy, operational state)
5. **At TSDB**: Schema design with proper indices on asset_id + timestamp; retention policies; compression
6. **At data lake**: Feature engineering (lag features, rolling windows, cross-sensor ratios); operational state labeling; data lineage tracking

---

## 9. What Goes Wrong Without Physical Data Preparation (Pain Points)

The following failure modes are specific to lack of AI-ready Physical Data preparation:

1. **Clock drift / no time synchronization**: Sensors from different vendors run on different clocks. Correlation analysis produces false leads. AI models find "patterns" that are actually artifacts of timing offsets. Fix: NTP/PTP synchronization at every edge device, UTC timestamps, Sparkplug B per-metric timestamps. [src-106]

2. **Frozen sensor / stuck values**: A sensor reads the same value despite changing conditions — common with failed transducers or broken wiring. AI model trained on frozen data learns wrong baselines. Fix: Rate-of-change validation at edge; heartbeat diagnostics (IO-Link). [src-106]

3. **No engineering unit context**: Modbus registers carry raw integers (0–65535). Is that °C, °F, °K, or 0.01-scale? Without unit metadata, AI models cannot generalize across sites with different configurations. Fix: OPC UA with engineering unit nodes; Sparkplug B typed metrics. [src-101, src-102]

4. **Inconsistent tag naming**: FIC-101 vs FIC_101 vs Line1_FIC101 vs FLOW_CTRL_01. The same sensor has 4 different names across sites. AI model trained on one site cannot be applied to another. Fix: Corporate naming standards enforced at edge/UNS; ISA S88 tag format. [src-106]

5. **Data spaghetti (point-to-point integrations)**: Every application has a direct connection to every data source. Adding a new AI application requires N new integrations. Changes in PLC configuration break all downstream consumers. Fix: Unified Namespace — all producers publish once, all consumers subscribe. [src-104]

6. **Operational state unlabeled**: The same sensor reading of 50°C means different things during startup, steady-state, and cooldown. AI trained without operational state labels produces high false-positive rates. Fix: Event-based layering in UNS; MES work order context overlay. [src-106]

7. **Legacy historian cannot feed AI**: OSI PI/AVEVA PI data is locked behind proprietary APIs, no open access, expensive per-tag licensing, cannot handle the volume needed for AI training. Fix: Open-source TSDB migration or federation; InfluxDB/TimescaleDB with Telegraf collectors. [src-105]

8. **Missing alarm context**: Alarm log contains alarm ID and timestamp but not the equipment state when it fired, or the sensor values 60 seconds before it fired. AI cannot learn early-warning signatures. Fix: Alarm historian enrichment with contemporaneous sensor values at triggered timestamps. [src-111]

---

## 10. AI-Ready Physical Data — Key Preparation Requirements

For physical data to serve as reliable input to AI (predictive maintenance, anomaly detection, OEE optimization, process optimization):

| Requirement | What It Means | Standard/Method |
|---|---|---|
| Time synchronization | All sensors on UTC + NTP/PTP; per-metric timestamps in Sparkplug B | NTP/PTP, Sparkplug B |
| Semantic context | Every data point carries: unit, asset ID, location, operational state | OPC UA Information Model, UNS |
| Self-describing data | Birth certificates announce metric types/ranges; no out-of-band config | Sparkplug B NBIRTH/DBIRTH |
| Quality flags | Every reading carries: Good/Bad/Uncertain + specific quality codes | OPC UA DA quality, Sparkplug B quality flags |
| Consistent naming | Standardized tag names across sites/lines/machines | ISA-88 tag format, corporate naming standards |
| Operational state labels | Startup, normal, shutdown, maintenance periods clearly marked | MES context, UNS event-based layering |
| Data lineage | Know the path from physical sensor through edge → broker → TSDB | OPC UA HA, metadata catalog |
| Adequate retention | 5–10 years without downsampling for AI training | Open-source TSDB (InfluxDB, TimescaleDB) |
| Edge validation | Range checks, rate-of-change checks before forwarding | HiveMQ Edge, Litmus Edge, HighByte |
| High-frequency capture | Sub-second for vibration/acoustic; 1s for temperature/pressure | Configurable PLC polling + Sparkplug B |

---

## 11. Solutions and Vendors — Backup Material

### Platform/Solution Comparison Table

| Solution | Vendor | Category | Key Features | AI/Data Features | Source |
|---|---|---|---|---|---|
| Litmus Edge | Litmus Automation | Edge data collection | 250+ protocol support (OPC UA, Modbus, FANUC, Siemens, Rockwell, etc.), edge processing, no cloud lock-in | Data normalization, edge ML, AI-ready data pipelines | src-111, src-109 |
| HighByte Intelligence Hub | HighByte (now part of HiveMQ) | Industrial DataOps / Edge | Protocol translation, semantic data modeling, data contracts, Sparkplug B support | Context-enriched data delivery, schema governance | src-109 |
| HiveMQ Platform | HiveMQ | MQTT broker + UNS | Enterprise MQTT broker, HiveMQ Edge, HiveMQ Pulse (semantic intelligence), HiveMQ Data Hub (stream governance) | Unified Namespace backbone, OT/IT metadata management, agentic AI data layer | src-106, src-107 |
| EMQX + NeuronEX | EMQ Technologies | MQTT broker + OT gateway | EMQX MQTT broker, NeuronEX industrial gateway for OT protocols, UNS implementation | Real-time data fabric, Sparkplug B support, OPC UA bridge | src-104 |
| AWS IoT SiteWise + Greengrass | Amazon Web Services | Cloud IoT + edge | Asset modeling (ISA-95 hierarchy), edge data collection, time-series ingestion, OPC UA collector | AI/ML integration (SageMaker), asset metrics, anomaly detection | src-109 |
| Azure IoT Operations | Microsoft | Cloud IoT + edge | Arc-enabled edge, OPC UA data collection, Azure Data Explorer integration, MQTT broker | Industrial AI data pipeline, AIO Data Processor | src-109 |
| Siemens Insights Hub (MindSphere) | Siemens | Industrial IoT cloud | Native Siemens OT integration, digital twin, OEE analytics, industrial edge deployment | Process analytics, AI/ML with Siemens ecosystem | src-109 |
| PTC ThingWorx + Kepware | PTC | IIoT platform + OPC gateway | Kepware: 150+ protocols to OPC UA; ThingWorx: asset models, dashboards, workflows | Custom IIoT apps, PLM/CAD/MES integration | src-105, src-109 |
| InfluxDB 3 + Telegraf | InfluxData | Time-series database | Millions of points/sec ingestion, MQTT + OPC UA plugins (Telegraf), FDAP stack (Arrow, Parquet) | ML integration, IoT anomaly detection, predictive maintenance | src-105, src-108 |
| AVEVA PI System | AVEVA (Schneider) | Industrial historian | Dominant legacy historian; deep OT integration; compliance-grade | Asset Analytics, PI Integrator for Business Intelligence; being modernized | src-105, src-108 |
| Ignition (Unified Namespace) | Inductive Automation | SCADA + UNS platform | MQTT Engine (Sparkplug B), OPC UA server/client, UNS implementation | Edge historian, data bridges | src-104 |
| MachineMetrics | MachineMetrics | CNC machine monitoring | Direct machine connectivity, ISA-95 framework, Intelligent MES | Max AI for predictive maintenance and OEE, real-time production intelligence | src-103 |

---

## 12. Glossary of Key Terms

| Term | Definition |
|---|---|
| Physical Data | Data originating from sensors, equipment, and field/shop-floor systems — the signal layer connecting the physical manufacturing world to digital/AI systems |
| OT (Operational Technology) | Hardware and software that detects or causes physical changes in industrial processes — PLCs, SCADA, DCS, sensors, actuators |
| IT (Information Technology) | Computing systems used for data processing, storage, and communication — ERP, MES databases, cloud platforms, analytics |
| IT/OT Convergence | Integration of OT systems (shop floor) with IT systems (business) to enable data-driven operations and AI |
| PLC (Programmable Logic Controller) | Ruggedized digital computer used to control manufacturing processes; stores data as "tags" — the primary source of shop-floor digital data |
| SCADA | Supervisory Control and Data Acquisition — software that monitors and controls industrial processes; collects PLC/DCS data and generates alarm logs |
| MES | Manufacturing Execution System — manages production at Level 3; bridges shop floor and ERP; provides production counts, schedules, OEE, quality results |
| OPC UA | OPC Unified Architecture — the dominant standard for semantic industrial communication; publishes an information model where every data point carries context (unit, quality, equipment ID) |
| MQTT | Message Queuing Telemetry Transport — lightweight publish-subscribe protocol for IoT; transports data from edge to cloud efficiently |
| Sparkplug B | Eclipse Foundation specification extending MQTT with standardized topic namespace, Protobuf payloads, birth/death lifecycle messages, and quality flags — the industrial IoT interoperability layer |
| ISA-95 | International standard (IEC 62264) defining the five-level manufacturing hierarchy and data objects for enterprise-control integration |
| Unified Namespace (UNS) | Architecture concept (Walker Reynolds, 2005) where all OT and IT systems publish to and consume from a central data hub — a real-time, semantic single source of truth for all factory data |
| Time-Series Data | Data points indexed by time — the native format for all physical/sensor data; requires specialized databases (InfluxDB, TimescaleDB) for high-frequency industrial workloads |
| Data Historian | Specialized time-series database for industrial process data; legacy examples: AVEVA PI, GE Proficy Historian; modern open-source alternatives: InfluxDB, TimescaleDB |
| Edge Computing | Computing at or near the data source (factory floor) rather than in the cloud; for physical data: collects, filters, normalizes, validates, and buffers sensor data before cloud transmission |
| B2MML | Business To Manufacturing Markup Language — XML schema implementing ISA-95 data objects for ERP-MES integration |
| Tag | In OT/PLC terminology, a named data point representing a specific variable (sensor reading, equipment state, calculated value) — the atomic unit of shop-floor data |
| Dead-banding | Edge filtering technique that only transmits a new value when it changes beyond a threshold — reduces bandwidth and storage without losing meaningful data |
| PTP / NTP | Precision Time Protocol / Network Time Protocol — standards for time synchronization across industrial devices; critical for multi-sensor correlation in AI applications |
| Birth Certificate (NBIRTH/DBIRTH) | In Sparkplug B, the message an edge node/device sends upon connection announcing all available metrics, their data types, and initial values — enables automatic device discovery |
| IO-Link | IEC 61131-9 standard for smart sensor communication; sensors report measurement value AND diagnostic data (calibration status, health, operating hours) |
| OEE | Overall Equipment Effectiveness = Availability × Performance × Quality — the primary KPI for manufacturing productivity; requires real-time physical data from PLCs/SCADA |
| RUL | Remaining Useful Life — predictive maintenance output; estimated time before equipment failure; computed from physical sensor data (vibration, temperature, current) using AI models |

---

## Sources

| id | Title | URL | Notes |
|---|---|---|---|
| src-101 | Exploring the OPC-UA Information Model (Embien) | https://www.embien.com/industrial-insights/exploring-the-opc-ua-information-model | OPC UA information model, node classes, companion specs, base models |
| src-102 | MQTT Sparkplug B Implementation (FlowFuse) | https://flowfuse.com/blog/2024/08/using-mqtt-sparkplugb-with-node-red/ | Sparkplug B topic namespace, Protobuf payloads, birth/death, comparison table with plain MQTT |
| src-103 | The Convergence of ISA 95 (MachineMetrics) | https://www.machinemetrics.com/blog/the-convergence-of-isa-95 | ISA-95 5-level model, IT/OT gap, data flattening, UNS introduction |
| src-104 | Unified Namespace: Architecture, Benefits & Solution (EMQX) | https://www.emqx.com/en/blog/unified-namespace-next-generation-data-fabric-for-iiot | Walker Reynolds UNS definition, 5 requirements, 4 types of namespaces, layer architecture |
| src-105 | Managing Time Series Data in Industrial IoT (InfluxData) | https://www.influxdata.com/blog/managing-time-series-data-industrial-iot/ | Time-series characteristics, IIoT use case table, legacy historian failures, open-source TSDB |
| src-106 | Data Quality, Standardization and Contextualization for AI Readiness (HiveMQ) | https://www.hivemq.com/blog/data-quality-standardization-contextualization-ai-readiness-manufacturing/ | 9 data quality dimensions, 5 standardization approaches, 4 contextualization patterns |
| src-107 | Don't Underestimate Industrial AI Data Quality Challenges, Says Gartner (HiveMQ) | https://www.hivemq.com/blog/industrial-ai-data-gartner/ | Gartner G00841619 (Dec 2025): 50% AI failure, 40% data issues, 3 building blocks: UNS/OPC UA, MQTT, metadata management |
| src-108 | Time-Series and IoT Data for AI Training (Introl) | https://introl.com/blog/time-series-iot-data-ai-training-influxdb-2025 | Storage tiering, database selection table, FDAP stack, edge buffering, industrial protocol integration |
| src-109 | Best Industrial IoT Platforms for Manufacturing in 2026 (Reliamag) | https://reliamag.com/guides/best-industrial-iot-platforms-2026/ | Platform comparison: Litmus, Siemens Insights Hub, AWS IoT SiteWise, PTC ThingWorx, selection criteria |
| src-110 | OPC UA — an overview over the central Industry 4.0 standard (KEBA) | https://www.keba.com/en/news/industrial-automation/overview-opc-ua-central-industry-4-0-standard | OPC UA SOA-based communications, semantic interoperability, Industry 4.0 scalability |
| src-111 | AI Integration with SCADA, PLC and MES Systems (GC AIT) | https://gc-ait.eu/blog/ai-scada-plc-mes-integration.html | Sensor types (vibration, temperature, current, pressure), PLC tag attributes, data quality challenges |
| src-112 | ANSI/ISA 95.00.01-2025: Enterprise Control System Integration (ANSI Blog) | https://blog.ansi.org/ansi/ansi-isa-95-00-01-2025-enterprise-control-system/ | ISA-95 2025 update, IT/OT convergence definition, B2MML and OPC UA as implementation technologies |
