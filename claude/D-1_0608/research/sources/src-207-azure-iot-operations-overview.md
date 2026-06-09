# src-207: Azure IoT Operations — Industrial Data Edge Processing

**URL:** https://learn.microsoft.com/en-us/azure/iot-operations/overview-iot-operations
**URL-2:** https://azure.microsoft.com/en-us/products/iot-operations
**Title:** What Is Azure IoT Operations? | Microsoft Learn
**AccessDate:** 2026-06-09
**Related section:** Tool map (Azure IoT Operations), edge processing, OPC UA / MQTT

---

## Key Excerpts

**Overview:**
- Azure IoT Operations processes and normalizes data at the edge before it's sent to the cloud
- Supports open standards: MQTT and OPC UA (Unified Architecture)

**Key Components:**
- MQTT Broker: industrial-grade, edge-native MQTT broker for high-speed, event-driven communication between local assets and the cloud
- OPC UA Connectivity: connector for OPC UA routes messages from OPC UA servers to MQTT broker; publishes data from OPC UA servers to MQTT topics
- Data Processing: no-code data flow graphs for visual data processing pipelines using built-in transforms (Map, Filter, Branch, Window, Concatenate)

**No-Code Data Flows (GA in 2025):**
- Build visual data processing pipelines without custom code
- Data can be processed, normalized, and contextualized at edge before routing to cloud destinations (Microsoft Fabric, Azure Event Hubs)
- Improved reliability, validation, and observability

**Litmus Integration:**
- Litmus Edge collaborates with Azure IoT Operations to automate industrial IoT device discovery and schema modeling
- Accelerates OT-IT collaboration at scale

**Protocol Support:**
- OPC UA over MQTT supported
- Industry-standard protocols: OPC UA and MQTT for data transport and connectivity

**Manufacturing Use Cases (HMI 2025):**
- Avanade and Microsoft showcased closed-loop manufacturing demos using AI machine vision for quality control integrated with Azure IoT Operations

**Source URLs:**
- https://learn.microsoft.com/en-us/azure/iot-operations/overview-iot-operations
- https://learn.microsoft.com/en-us/azure/iot-operations/discover-manage-assets/overview-opc-ua-connector
- https://azure.microsoft.com/en-us/products/iot-operations
