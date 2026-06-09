# src-213: OPC UA and MQTT Sparkplug — Industrial Data Standardization Protocols

**URL:** https://ifactoryapp.com/blog/opc-ua-mqtt-ai-ready-factory-sensor-data
**URL-2:** https://www.hivemq.com/blog/iiot-protocols-opcua-vs-mqtt-sparkplug-digital-transformation/
**URL-3:** https://cirrus-link.com/mqtt-vs-rest-vs-opc-ua-which-fits-modern-industrial-architecture/
**Title:** Using OPC UA and MQTT for AI-Ready Factories; Key Differences Between OPC UA and MQTT Sparkplug
**AccessDate:** 2026-06-09
**Related section:** Stage 4: Standardize units & IDs; metadata/tag standardization; real-time transport protocols

---

## Key Excerpts

**Industry Consensus (2026):**
- "OPC UA organizes the data, MQTT moves it"
- OPC UA: industrial-automation standard for structured, secure, and contextualized data exchange; hierarchical data models with metadata, units, and semantics — ideal for deterministic, high-integrity communication inside plants
- MQTT: lightweight publish/subscribe protocol for reliable communication over low-bandwidth or unreliable networks — perfect for moving industrial data efficiently between edge devices and cloud systems

**Sparkplug B:**
- Open standard maintained by Eclipse Foundation that defines how MQTT messages are structured for industrial use
- Adds: context, state awareness, and standard naming conventions to MQTT payloads
- Defines topic namespace structure: Enterprise → Site → Area → Line → Device
- Makes data consistent and interoperable across different systems and vendors
- Sparkplug B = MQTT + structure + state awareness + standardized encoding (Protocol Buffers)

**OPC UA Key Features:**
- Hierarchical address space with information models
- Built-in security (authentication, encryption)
- Semantics and metadata embedded in data structure (engineering units, min/max ranges, descriptions)
- Widely supported by PLCs, DCS, SCADA, historians

**MQTT Key Features:**
- Lightweight: minimal bandwidth, low latency
- Publish/subscribe: decoupled producers and consumers
- Quality of Service (QoS) levels: 0, 1, 2
- Works over intermittent/unreliable networks

**AI Readiness:**
- "When paired with Sparkplug B and OPC UA, sensor data arrives at AI/analytics platforms with metadata, units, and hierarchical context — no manual mapping required"
- Standard naming conventions via Sparkplug B eliminate the "tag soup" problem where each PLC vendor uses different naming

**Unified Namespace (UNS):**
- Architecture pattern using MQTT broker as central data hub
- All systems publish to and subscribe from the UNS
- ISA-95 hierarchy levels used to organize topic structure: Enterprise/Site/Area/Line/Device/Tag
- Source: https://teeptrak.com/en/unified-namespace-uns-mqtt-sparkplug-iiot-2027/
