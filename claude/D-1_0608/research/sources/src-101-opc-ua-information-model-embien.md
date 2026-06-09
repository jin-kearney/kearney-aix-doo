# src-101: Exploring the OPC-UA Information Model
URL: https://www.embien.com/industrial-insights/exploring-the-opc-ua-information-model
Title: Exploring the OPC-UA Information Model
AccessDate: 2026-06-09
Related section: What (OPC UA standard — information model, address space, companion specs)

---

## Key Excerpts

OPC-UA (Open Platform Communications Unified Architecture) is a machine-to-machine (M2M) communication protocol that enables seamless data exchange between various industrial devices, systems, and applications. It was developed by the OPC Foundation, a non-profit organization dedicated to the advancement of open connectivity solutions for industrial automation.

The OPC-UA information model is a comprehensive framework that defines the structure and semantics of the data exchanged within an OPC-UA system.

**Key node classes:**
- View: Defines a subset of nodes in the Address Space.
- ObjectType: Provides definition for objects.
- Object: Represent systems, system components, real-world objects and software objects.
- ReferenceType: Defines the meaning of the nodes relationship.
- DataType: Define simple and complex data types of the Variable values.
- VariableType: Provides type definition for variables.
- Variable: Provides the value information.
- Method: Represents lightweight function, whose scope is bounded by an owning object.

**Base Models defined in OPC UA:**
- DataAccess (DA) Base Model: defines structure and semantics for representing real-time data, such as sensor readings, process variables, and control signals.
- Alarm & Conditions (AC) Base Model: defines structure and semantics for representing alarms, events, and other conditional states.
- Historical Access (HA) Base Model: provides a standardized way to access and manage historical data.
- Programs (Prog) Base Model: defines structure and semantics for representing and executing programs, scripts, and other executable logic.

**Companion Specifications** are domain-specific extensions developed by industry groups — examples include OPC UA for Devices (DI), OPC UA for Robotics (ROB), OPC UA for PLCopen, and OPC UA for ISA-95.

**Service Sets** include:
- Discovery Service Set
- SecureChannel Service Set
- Session Service Set
- NodeManagement Service Set
- View Service Set
- Query Service Set
- Attribute Service Set
- Method Service Set
- Subscription Service Set

Unlike raw sensor protocols, OPC UA's information modeling includes engineering units, quality indicators, and equipment context. An OPC UA-enabled pressure sensor could convey the measurement unit, measurement range, sensor type, serial number, manufacturer, and even measurement quality metrics in addition to just the raw pressure value.
