# src-409 — IIoT World: What Is a Unified Namespace (UNS) and How Does It Work in Manufacturing?

**URL:** https://www.iiot-world.com/smart-manufacturing/what-is-a-unified-namespace-and-how-does-it-work-in-manufacturing/
**Title:** What Is a Unified Namespace (UNS)? | IIoT World
**AccessDate:** 2026-06-09
**Related section:** KPI (contextualization coverage / tag/ID standardization), Roadmap (Stage 3)

---

## Key Excerpts

> A Unified Namespace is a single access point for all manufacturing data within an organization that aggregates real-time and historical data from PLCs, ERP, MES, and other systems using MQTT brokers organized in ISA-95 hierarchical structures.

> UNS is more than just a data aggregator or tool; it is a dynamic system and strategic approach that ensures data is available, consistent, and accessible across both OT and IT layers.

> UNS's greatest strength lies in contextualization: data is organized in a tree structure (e.g., location/hall/line/asset) so that it can be immediately interpreted by IT applications.

## Relevance to KPI: Contextualization Coverage

The % of tags/equipment with standardized IDs, units, and metadata in UNS tree structure = "Contextualization Coverage Rate."

A tag is "contextualized" when it has:
- Standardized ID (following ISA-95 or corporate naming convention)
- Defined engineering unit
- Asset hierarchy reference (site/area/line/equipment)
- Minimum required metadata fields populated

Formula: `Contextualization Coverage (%) = (Tags with full context / Total tags in system) × 100`

Target: ≥ 80% for site-level AI use cases; 95%+ for enterprise-level AI.
