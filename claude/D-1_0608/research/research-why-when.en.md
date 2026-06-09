# D-1 Physical Data — Research Notes: Why + When
**Topic:** D-1 Physical Data (sensor & equipment data as AI input)
**Cluster:** Why (concept/structure) + When (conditions/triggers)
**Audience:** Manufacturing leaders
**Date:** 2026-06-09

---

## WHY: Raw Physical Data Cannot Be Used Directly by AI

### 1. OT Data Was Never Designed for AI — The Compliance-Grade vs. Analytics-Grade Gap

The fundamental problem: sensors and OT systems were installed to support compliance, traceability, and basic monitoring — not to feed AI models. This creates a structural gap between what exists and what AI needs.

Schneider Electric's industrial experts articulate this gap directly:
> "Much of the OT data collected today was designed for compliance, traceability, or basic monitoring, not for optimization. Compliance-grade data answers questions such as whether a process stayed within acceptable limits... Analytics-grade data, by contrast, must support continuous analysis, comparison, and improvement. It requires different sampling rates, structures, and contextual information." [src-301]

As a result, industrial organizations are "data-rich and insight-poor: surrounded by operational data, yet unable to use it consistently at scale." [src-301]

The industrial data scientist community has documented this from the practitioner side: "Data acquisition and systems' architectures, including the installation of sensors, were not aimed at collecting data for AI. The systems solved their local tasks (incident analysis, diagnostics, reporting, manual process monitoring, regulatory documentation requirements), therefore data collection is not optimally organized." [src-302]

### 2. The Eight Structural Defects of Raw Sensor Time-Series Data

Raw sensor data coming out of OT systems arrives with the following structural defects that make it directly unusable for AI/ML [src-302]:

| Defect | Description | AI Impact |
|---|---|---|
| Missing values (data loss) | Gaps in the sequence due to network drops, sensor failures | Breaks sequence models; creates bias |
| Irregular / absent sample rate | Timestamps arrive at variable intervals or without a time grid | Makes almost all time-series analysis methods inapplicable |
| Noisy data / inconsistent noise level | Signal noise changes over time | Obscures real patterns; degrades model accuracy |
| Outliers and impossible values | Values outside physical range | Corrupts training data |
| Abrupt changes / concept drift | Process mode changes, sensor replacement — the statistical distribution of the signal changes | Model trained on one mode fails on another |
| Unit inconsistency | Same physical quantity in cm vs. inch, Celsius vs. Fahrenheit across data sources | Silently corrupts multi-sensor models |
| Time synchronization mismatch | Timestamps from different OT sources use different time zones or clocks (e.g., UTC+0 vs UTC+3) | Sensor readings can't be aligned for multi-variate models |
| No contextual metadata | A reading is a number with a timestamp — no asset ID, no production line, no work order | AI model cannot locate the event in operational reality |

A temperature reading with a timestamp does not tell you whether it is Fahrenheit or Celsius, which factory generated it, which production line, or which asset. Without that context, the data cannot drive a decision. [src-305]

### 3. The OT/IT Divide: Why Sensor Data Stays Siloed

OT (Operational Technology) systems and IT systems were built independently, with different goals, different owners, and different protocols. This divide is structural, not just technical:

**Organizational divide:**
- OT teams prioritize machine uptime and physical safety
- IT teams prioritize standardization and cybersecurity
- Neither was designed to share data ownership or change-control responsibilities [src-309]

**Technical divide:**
- OT systems run on proprietary protocols (Modbus, PROFIBUS, Siemens S7, OPC-DA, proprietary historian formats) "built decades ago and never intended to be connected to anything outside the plant floor" [src-309]
- The traditional ISA-95 pyramid architecture moves data "up or down one layer at a time using point-to-point connections" — to get PLC data to a cloud AI platform requires passing through SCADA, then MES, then ERP, each requiring specialized engineering [src-303]
- "Traditional data integration uses proprietary application interfaces only accessible by specific technology vendors. This leaves companies with hundreds, if not thousands, of point-to-point connections that only a specialized group of people understand." [src-303]
- Machine sensor data "arrives in millisecond intervals, in proprietary formats, with calibration drift, and without the metadata that data scientists expect." [src-306]

**The silo inventory in a typical manufacturer:**
Equipment-level control (PLCs, DCS, SCADA) — MES — ERP — Quality Management — Supply Chain — Maintenance Management — each maintains "its own data storage, formats, and access methods, creating significant barriers to enterprise-wide integration." [src-306]

### 4. Why Context (Semantics/Metadata) Is the Missing Layer

Raw sensor values are meaningless without context. Contextualization means attaching metadata that gives a reading its operational meaning:
- Which factory, production line, asset
- Which work order, batch, product, customer
- Operating state (normal run, startup, maintenance mode)
- Maintenance and event history

"All the OT data assets typically have been constructed by different teams and integrators at different times, causing each system to have its own unique tag naming and contextualization environments." [src-310]

The Unified Namespace (UNS) concept addresses this: it is an architecture and design pattern that creates "a centralized, hierarchical data structure that aggregates and organizes OT data in real time and acts as a single source of truth for industrial environments, exposing live data from machines, sensors, and PLCs in a structured and accessible way." [src-310]

UNS is structured according to ISA-95 hierarchy (Enterprise → Site → Area → Line → Machine → Tag), so every data point carries its full operational context from the moment it is published.

### 5. AI Amplifies Inconsistency — It Does Not Fix It

A common mistake in manufacturing AI programs: start with an AI use case and assume the data problem will be resolved along the way. The opposite is true.

"AI does not resolve inconsistency. It amplifies it. If the inputs are incomplete, time-misaligned, or missing context, the outputs may look polished while being harder to validate, and trust erodes quickly." [src-304]

"A common rule of thumb holds that 80% of an AI project is spent on data preparation. In that sense, AI is the finale, not the opening act. It magnifies existing data foundations; if OT data is not consistently modeled and exposed, AI becomes fragile, expensive, and difficult to scale." [src-301]

"Even successful AI pilot projects fail to scale beyond single production lines because each deployment requires expensive, time-consuming custom integrations." [src-306]

### 6. What Becomes Impossible Without Physical Data Preparation — Cause-Effect Chain

The following cause-effect chains show what breaks when physical data preparation is absent:

**Predictive Maintenance:**
- Cause: Vibration sensor data has irregular sample rates + no time synchronization with temperature data from another system → Effect: Multi-variate model cannot be trained; early fault detection fails
- Cause: A sensor was miscalibrated for 6 months without detection → Effect: The entire historical training dataset contains corrupted baselines; model accuracy is unreliable from day one [src-307]
- Cause: No asset ID or maintenance history linked to sensor readings → Effect: Model cannot distinguish normal post-maintenance behavior from anomaly; false positive rate explodes [src-308]
- "For equipment utilizing multiple sensors, the complete loss of data from a single sensor significantly diminishes the predictive maintenance capability of AI models." [src-308]

**Quality Prediction / Anomaly Detection:**
- Cause: Process sensor readings in different units (bar vs. psi, °C vs. °F) from machines on the same line → Effect: Multi-sensor quality model produces nonsensical correlations
- Cause: No batch context (which product, which recipe, which operator shift) attached to sensor streams → Effect: Cannot train a conditional model; prediction accuracy collapses across product transitions
- "Without IT/OT integration, the data needed to interpret equipment behavior remains fragmented, which limits model trust and makes it difficult to scale predictive maintenance across plants." [src-304]

**Digital Twin:**
- Cause: OT historian data locked behind proprietary protocols with no API → Effect: Twin cannot receive real-time physical state updates; becomes a static simulation, not a live mirror

### 7. The Structural Solution: Edge → Context → Cloud Flow

The answer to raw physical data's AI-readiness problem is a deliberate preparation architecture along the edge-to-cloud path:

1. **Edge layer (at or near the machine):** Protocol conversion from proprietary OT to open standards (OPC-UA, MQTT Sparkplug); initial data normalization; time-stamping; local buffering for resilience
2. **Contextualization layer (UNS / data platform):** Attach asset hierarchy, batch/order context from MES/ERP; normalize units; enforce a regular time grid; validate ranges; resolve tag naming into equipment IDs
3. **Cloud/analytics layer:** AI model training and inference on clean, contextualized, time-aligned time-series

"Real-time data at the edge feeds calculations, cleansing, and contextualization that serve the shop floor directly. A subset of that data flows to the cloud for longer-term predictive modeling, equipment optimization, and capital expenditure planning." [src-305]

IIoT platforms that support this flow require: "Connectivity & protocol conversion: Bridging legacy field-bus and proprietary PLC protocols to IIoT standards. Data services: Built-in tools for contextualization (tags to assets), normalization (units, timestamps, hierarchies), transformation and quality management." [src-306]

---

## WHEN: Physical Data Preparation Is Always Required / Conditionally Required

### Always Required (Universal Condition)

Physical data preparation is a necessary precondition for **any** AI or ML use of sensor/equipment data. There is no exception. The moment an organization decides to use sensor or equipment data as an AI model input, the following preconditions must hold:

1. **Sensor data is consistently accessible** (not locked in proprietary historians with no extraction path)
2. **Data has a regular, reliable time grid** (timestamps are consistent; sample rate is stable or predictably variable)
3. **Units are standardized** across all sensors feeding the same model
4. **Time synchronization** is resolved across sources
5. **Contextual metadata** (asset identity, operational state, batch/order) is attached
6. **Sufficient, uninterrupted history** exists for the target failure mode or quality event

This is not a one-time project — it is ongoing infrastructure. A sensor calibration drift, a hardware replacement that changes a tag name, or a shift in sampling rate will silently degrade model performance unless a data quality monitoring layer catches it. [src-304]

### Specific Use-Case Triggers: When Organizations Start This Work

Physical data preparation typically becomes urgent at one of the following trigger points [src-305, src-304, src-308]:

| Trigger | Why It Forces the Data Issue |
|---|---|
| First predictive maintenance (PdM) pilot | Data scientist discovers sensor history is insufficient, unsynchronized, or uncalibrated |
| Quality prediction or SPC automation | Requires correlating multi-sensor process data with quality outcomes; unit/time alignment issues surface immediately |
| Anomaly detection deployment | Model needs clean baseline; any data drift or gaps produce unacceptable false alarm rates |
| Digital twin initiative | Requires real-time physical state; locked OT data becomes a blocker |
| AI/ML pilot fails to scale from 1 line to 2+ lines | Each new line has different tag names, different protocols, different historians — no reusability |
| Cross-site benchmarking program | OEE, energy, or quality KPIs cannot be compared because sensors and units differ by site |

### Conditional: Depth of Preparation Scales with AI Complexity

Not every use case requires the same depth of physical data preparation:

- **Rule-based threshold alerting:** Minimal preparation needed; basic timestamp and range validation sufficient
- **Statistical anomaly detection (univariate):** Requires stable sampling rate, range normalization, outlier handling for a single sensor
- **Multivariate ML (PdM, quality prediction):** Requires time synchronization, unit consistency, contextual metadata, training data with labeled events
- **Deep learning / time-series foundation models:** Requires long, high-quality, richly contextualized history; continuous data quality monitoring; concept drift detection
- **Digital twin / real-time AI:** Requires full edge-to-cloud pipeline with sub-second latency; complete asset context hierarchy

### Adoption Triggers: What Pushes Organizations to Invest

Beyond the specific use case, organizational triggers that push companies to invest in physical data preparation infrastructure include [src-301, src-304, src-306]:

- **AI program failure:** A pilot delivered good results in the lab but collapsed in production due to data quality; executive sponsor demands a proper data foundation before the next program
- **Historian end-of-life / migration:** Forced rearchitecture of OT data storage creates an opportunity to rebuild with AI readiness in mind
- **New plant or line construction (greenfield):** Ability to design sensor data architecture from scratch without legacy constraints
- **Corporate AI mandate:** Top-down directive to deploy AI across multiple sites; per-site custom integration is immediately unscalable
- **Regulatory traceability requirement:** Forces consistent, timestamped, contextualized sensor data capture — creates a foundation that can be extended for AI

---

## Sources

| id | URL | Title |
|---|---|---|
| src-301 | https://blog.se.com/industry/2026/02/04/ot-data-the-foundation-behind-industrial-analytics-and-ai/ | OT data: The foundation behind industrial analytics and AI (Schneider Electric) |
| src-302 | https://medium.com/@katser/quality-issues-in-industrial-time-series-data-b15d02c28024 | Quality Issues in Industrial Time-Series Data (Iurii Katser, Medium) |
| src-303 | https://www.hivemq.com/blog/what-is-unified-namespace-uns-iiot-industry-40/ | What is Unified Namespace (UNS) and Why Does it Matter? (HiveMQ) |
| src-304 | https://candf.com/our-insights/articles/industry40-data-architecture-uns-itot-ai-readiness/ | Industry 4.0 Data Architecture: UNS, IT/OT & AI Readiness (C&F) |
| src-305 | https://www.iiot-world.com/smart-manufacturing/edge-ai-industrial-data-context/ | What Edge AI Needs from Industrial Data (IIoT World / InfluxData + Litmus) |
| src-306 | https://www.hivemq.com/blog/enabling-scalable-industrial-data-architecture-for-ai-ready-manufacturing/ | Enabling a Scalable Industrial Data Architecture for AI-Ready Manufacturing (HiveMQ) |
| src-307 | https://www.mckinsey.com/capabilities/mckinsey-digital/our-insights/clearing-data-quality-roadblocks-unlocking-ai-in-manufacturing | Clearing data-quality roadblocks: Unlocking AI in manufacturing (McKinsey) |
| src-308 | https://www.aiventic.ai/blog/ai-challenges-in-predictive-maintenance | AI Challenges in Predictive Maintenance (Aiventic) |
| src-309 | https://zentechworks.com/when-two-worlds-collide-the-it-ot-divide-that-is-quietly-stalling-manufacturing | When Two Worlds Collide: The IT/OT Divide That Is Quietly Stalling Manufacturing (Zen Techworks) |
| src-310 | https://cirrus-link.com/understanding-the-unified-namespace-uns-in-industrial-iot/ | Understanding the Unified Namespace (UNS) in industrial IoT (Cirrus Link) |
