# src-104: Unified Namespace (UNS): Architecture, Benefits & Solution
URL: https://www.emqx.com/en/blog/unified-namespace-next-generation-data-fabric-for-iiot
Title: Unified Namespace (UNS): Architecture, Benefits & Solution
AccessDate: 2026-06-09
Related section: What (Unified Namespace architecture, IIoT data fabric)

---

## Key Excerpts

**Walker Reynolds definition:** UNS is not only a naming convention but an architecture that meets five requirements:
1. UNS is the semantic hierarchy structure of business data and events.
2. UNS is the hub that connects all smart devices and IT infrastructure.
3. UNS is the single source of truth for all data and information in business.
4. UNS is the foundation of future digital transformation.
5. UNS is where the current state of business lives, enabling real-time snapshots of business.

The first UNS project was built by Walker Reynolds in 2005 using Dynamic Data Exchange (DDE) with Excel spreadsheets for a salt mining field, then adapted to MQTT technology.

**UNS vs ISA-95/Industry 3.0:**
In Industry 3.0: ERP → WMS → MES → Shop floor (reports via spreadsheets). Data is not real-time and cannot accurately reflect current business state. AI/ML components usually require a comprehensive dataset, but there is no easy way to create a unified dataset. ERP cannot connect to factory machinery directly, leading to "data spaghetti."

**UNS Layer Architecture (bottom to top):**
1. Physical devices (HMI/PLC) — data generation layer; each PLC/HMI or tag is a namespace pushing readings and events
2. Data acquisition layer — industrial gateway (e.g., NeuronEX) deploys here for data collection
3. Manufacturing Execution Layer (MES) — coordinates between ERP/CRM and plant floor
4. Traditional ERP layer — processes customer sales, plans manufacturing, manages inventory
5. Business Intelligence Cloud — manages the business layer

**4 Types of Unified Namespaces:**
1. Functional Namespaces — grouped by function/purpose (e.g., production OEE, maintenance); used to measure OEE across Availability, Performance, Quality
2. Informative Namespace — grouped by informational content (temperature, pressure, etc.)
3. Definitional Namespace — grouped by definitions/attributes (motors, sensors, etc.)
4. Ad Hoc Namespace — temporary grouping for urgent specific purposes (e.g., sudden equipment failure)

**Key Advantages:**
- Easy integration: producers and consumers plug into the network without specialized expertise
- Improved agility: real-time access to current state of data
- Scalability: millions of nodes can be connected through a central hub

**Implementation (EMQX + NeuronEX):**
EMQX serves as central messaging broker; NeuronEX industrial gateway accesses OT sensors/devices with various industrial protocols. Together they act as the primary conduit connecting data sources (devices, sensors, machines) to data consumers (ERP, MES, databases, analytics platforms).
