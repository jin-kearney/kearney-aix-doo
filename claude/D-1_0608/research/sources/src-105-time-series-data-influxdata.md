# src-105: Managing Time Series Data in Industrial IoT
URL: https://www.influxdata.com/blog/managing-time-series-data-industrial-iot/
Title: Managing Time Series Data in Industrial IoT (InfluxData)
AccessDate: 2026-06-09
Related section: What (time-series data model, IIoT data characteristics, legacy historians)

---

## Key Excerpts

**Time-series data as the critical context:**
No matter what type of reading a sensor collects, it always includes a timestamp. Time-series data provides a shared context for these readings and becomes the critical fulcrum for processing and understanding Industry 4.0 IoT data.

**Industry 4.0 principles:**
- Interconnection: ability for devices, sensors, and people to connect and communicate
- Information transparency: collecting large amounts of data from all points of the manufacturing process
- Technical assistance: aggregating and visualizing collected data for informed decisions
- Decentralized decisions: systems perform tasks autonomously based on collected data

**IIoT Time Series Use Case table:**
| IIoT use case | |
|---|---|
| Metrics | Temperature, pressure, flow, valve state, etc. |
| Resolution | (Sub) seconds |
| Retention | 5 to 10 years (or longer), no downsampling |
| Main goals | Quality guarantee, OEE, predictive maintenance |

**Problems with legacy data historians:**
- Cost: expensive to set up and maintain, annual license/support fees, custom development required
- Vendor lock-in: Windows-based, no open API, must buy all integrations from single vendor
- Scalability: built with limited dataset in mind — cannot handle AI/ML which needs far more data
- Poor developer experience: closed design, limited API support, no developer community
- Siloed data: data at site level is separated by firewalls and subnets; no interoperability with cloud/OSS

**Why relational databases also fail:**
Relational databases can't scale for high-volume data and lack of fixed schema that characterize time-series data.

**Open-source time-series platform advantages (InfluxDB example):**
- Purpose-built to handle volume and velocity of time-series data
- Uses APIs to integrate with virtually any connected device
- Schemaless platform — automatically adjusts to changes in shape of incoming IIoT data
- Telegraf plugin-based collection agent: 100+ plugins for OPC-UA, MQTT, Modbus, AMQP, Kafka
- Integrations with PTC Kepware, PTC ThingWorx, Siemens WinCC OA, Bosch ctrlX

**Scale example:**
Three facilities across the country — Telegraf and InfluxDB collect data from every sensor on every machine, aggregate on-site, then roll up to a central instance for company-wide insights.
