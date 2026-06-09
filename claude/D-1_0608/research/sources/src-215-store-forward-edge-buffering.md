# src-215: Store-and-Forward at the Edge — Buffering During Network Outages

**URL:** https://flowfuse.com/blog/2025/11/store-and-forward-edge-data-buffering/
**URL-2:** https://www.wirtek.com/blog/connectivity-reliability-in-industrial-iot-designing-for-intermittent-and-degraded-networks
**Title:** Store-and-Forward at the Edge: Buffering Production Data During Network Outages
**AccessDate:** 2026-06-09
**Published:** November 2025 (FlowFuse Blog)
**Related section:** Edge computing role — store-and-forward, buffering

---

## Key Excerpts

**Core Problem:**
- Production metrics, quality measurements, and alarm events accumulate with no path to historian or cloud platform when connectivity fails
- Gaps in operational records cause: incomplete batch records for quality audits, missing data for troubleshooting, compliance documentation that doesn't hold up under review

**Store-and-Forward Solution:**
- "Store-and-forward protocols locally collect and store data if a connection is ever lost — some for up to 35 days — before transferring all logged data once the connection is reestablished"
- Data remains buffered at the edge until transmission is reestablished
- No data loss from intermittent connectivity

**Industrial Network Failure Modes:**
- Full outages to subtle latency and packet loss
- Wireless dead zones in large factory buildings
- VPN tunnels disrupted during maintenance windows
- ISP outages at remote facilities

**Business Impact:**
- "Unplanned downtime costs industrial manufacturers an estimated $50 billion per year" (Deloitte)
- Store-and-forward prevents data gaps that undermine AI training datasets and process analytics

**Implementation Approaches:**
- Edge computing allows critical logic and telemetry buffering to happen locally on devices and gateways
- Industrial systems continue to operate even when cloud is unreachable
- Tools: FlowFuse (Node-RED based), Litmus Edge, AWS IoT Greengrass, Azure IoT Edge all support store-and-forward

**Critical Design Consideration:**
- Buffer storage sizing: must account for maximum expected outage duration × data rate
- For high-frequency vibration sensors: consider edge compression/aggregation to reduce buffer requirements
- For low-frequency process sensors: full fidelity buffering is practical
