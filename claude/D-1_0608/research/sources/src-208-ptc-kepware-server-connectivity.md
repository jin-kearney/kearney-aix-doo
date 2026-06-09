# src-208: Kepware Server | PTC Industrial Connectivity

**URL:** https://www.ptc.com/en/products/kepware/kepware-server
**URL-2:** https://www.ptc.com/en/products/kepware
**Title:** Kepware Server — The Standard for Industrial Connectivity | PTC
**AccessDate:** 2026-06-09
**Related section:** Tool map (PTC ThingWorx Kepware — connectivity layer)

---

## Key Excerpts

**Overview:**
- "Kepware Server is the proven standard for industrial connectivity" — formerly known as KEPServerEX and ThingWorx Kepware Server
- Transforms proprietary device data into standardized formats for any application — traditional OT (SCADA, historians) or modern IT (cloud)

**Protocol Drivers:**
- 150+ pre-built drivers and scriptable options for custom interfaces
- Protocols: OPC UA, Modbus, Siemens S7, Allen-Bradley EtherNet/IP, BACnet, MQTT, Fanuc FOCAS, and more

**Data Flow:**
- Converts proprietary protocols into open standards like OPC UA and MQTT
- Enables real-time, condition-based linking between devices
- Bridges OT and IT: integrates device data into ERP, MES, SCADA, and cloud platforms (AWS, Azure)

**ThingWorx Integration:**
- Kepware acts as the industrial connectivity layer for ThingWorx, providing drivers for 150+ industrial protocols
- Connects PLCs, SCADA systems, industrial equipment → normalizes data → publishes to ThingWorx through Industrial Gateway

**Enterprise Scale:**
- Kepware+ Manager: centralized monitoring and configuration across multiple servers
- "Proven at scale across production lines, sites, and entire enterprises — trusted by manufacturers for over 30 years"

**Key Role in Data Prep Pipeline:**
- Acts as the OT protocol translation layer — the first step converting machine-specific data (PLC registers, CNC data, fieldbus signals) into an OPC UA/MQTT format that can be ingested by higher-level systems
- Critical for legacy equipment integration: many factory floor devices speak proprietary protocols inaccessible to modern analytics platforms

**InfluxDB Integration:**
- Kepware OPC monitoring: direct integration with InfluxDB for time-series storage
  URL: https://www.influxdata.com/integration/kepware/
