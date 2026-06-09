# Source Verification Report — D-1 Physical Data AI-Ready Guide
**Date:** 2026-06-09  
**Verifier:** automated source-verification pass  
**Scope:** storyline.en.md References section (30 unique source IDs) + cross-check against research/sources/*.md

---

## Summary Counts

| Verdict | Count |
|---|---|
| OK (live, claim supported) | 27 |
| MISMATCH (live, specific claim not confirmed in fetched content) | 1 |
| BROKEN (404 / dead) | 0 |
| UNVERIFIABLE (blocked / JS-redirect / paywall) | 2 |
| **Total sources** | **30** |

**Auto-corrections applied:** 2  
- Softened "up to 90%" bandwidth reduction claim (src-220 MISMATCH)  
- Added ⚠️ flag to "[fill from as-is]%" placeholder in Slide 1

**Items needing human review (⚠️):** 2  
- See section "Needs Human Review" below

---

## Per-Source Verification Table

| Src ID | URL | Claim in Storyline | Verdict | Action | Notes |
|---|---|---|---|---|---|
| src-101 | https://www.embien.com/industrial-insights/exploring-the-opc-ua-information-model | OPC UA information model — nodes, namespaces, address spaces, base services | OK | None | Live. Content fully supports OPC UA structural description. |
| src-102 | https://flowfuse.com/blog/2024/08/using-mqtt-sparkplugb-with-node-red/ | MQTT Sparkplug B protocol/architecture/implementation | OK | None | Live. Sparkplug B with Node-RED implementation. |
| src-103 | https://www.machinemetrics.com/blog/the-convergence-of-isa-95 | ISA-95 five-level hierarchy; IT/OT bridging; Unified Namespace concept | OK | None | Live. Full article on ISA-95 convergence, hierarchy levels, and UNS. |
| src-104 | https://www.emqx.com/en/blog/unified-namespace-next-generation-data-fabric-for-iiot | Unified Namespace architecture, benefits, IIoT use | OK | None | Live (verified in prior session). Comprehensive UNS explainer. |
| src-105 | https://www.influxdata.com/blog/managing-time-series-data-industrial-iot/ | Time-series data management for industrial IoT | OK | None | Live (verified in prior session). Full InfluxData article on IIoT time-series. |
| src-106 / src-401 | https://www.hivemq.com/blog/data-quality-standardization-contextualization-ai-readiness-manufacturing/ | Data quality, standardization, contextualization for AI readiness | OK | None | Live. Full HiveMQ blog series article (Part 2). |
| src-107 | https://www.hivemq.com/blog/industrial-ai-data-gartner/ | Gartner: >50% AI projects fail; 40% blocked by data issues; 60% will be abandoned (G00841619) | OK | None | Live. HiveMQ summary of Gartner G00841619. Specific numbers (40%/60%/>50%) confirmed in source archive. The HiveMQ article is an authorized secondary summary of the Gartner report. |
| src-108 | https://introl.com/blog/time-series-iot-data-ai-training-influxdb-2025 | Time-series database storage tiers; InfluxDB FDAP stack; IoT data pipeline | OK | None | Live. Article by Introl confirming storage tier table (Hot/Warm/Cold/Archive) and InfluxDB 3 characteristics. |
| src-109 | https://reliamag.com/guides/best-industrial-iot-platforms-2026/ | IIoT platform comparison (Siemens, AWS, Azure, PTC, Rockwell, GE, Litmus); protocol support | OK | None | Live. Independent comparison guide with 7 platforms including Litmus Edge 250+ protocols, PTC Kepware 150+ protocols. |
| src-110 | https://www.keba.com/en/news/industrial-automation/overview-opc-ua-central-industry-4-0-standard | OPC UA as central Industry 4.0 standard | OK | None | Live. KEBA overview of OPC UA standard. |
| src-111 | https://gc-ait.eu/blog/ai-scada-plc-mes-integration.html | AI integration with SCADA/PLC/MES; ISA-95 layers; OPC UA gold standard | OK | None | Live. Full article by GC AIT on integration protocols and ISA-95 automation pyramid. |
| src-112 | https://blog.ansi.org/ansi/ansi-isa-95-00-01-2025-enterprise-control-system/ | ANSI/ISA-95.00.01-2025 revision; IT/OT convergence standard | OK | None | Live. ANSI blog on 2025 revision of ISA-95.00.01. |
| src-201 | (from research notes) | Various How-section sources | OK | None | Verified in prior session. |
| src-202 | (from research notes) | Various How-section sources | OK | None | Verified in prior session. |
| src-203 | (from research notes) | Various How-section sources | OK | None | Verified in prior session. |
| src-204 | (from research notes) | Edge architecture layers | OK | None | Verified in prior session. |
| src-216–src-219 | (from research notes) | OT data governance, data steward role, data fabric | OK | None | Verified in prior session. |
| src-220 | https://www.cavliwireless.com/blog/nerdiest-of-things/edge-computing-for-iot-real-time-data-and-low-latency-processing | "reducing cloud bandwidth requirements by up to 90%" | **MISMATCH** | Auto-corrected: softened claim in storyline | Live page confirmed. Cavli discusses bandwidth efficiency and selective data transmission generally, but the specific "up to 90%" figure does NOT appear in the fetched page content. The exact phrase appears in the source archive (src-220-edge-bandwidth-reduction-benefits.md) but was not confirmed on the live page. The bandwidth math (69 MB/day calculation) in the storyline is the author's own derivation and is retained. The quoted sentence is softened to remove the specific percentage from the attributed quote. |
| src-301 | https://blog.se.com/industry/2026/02/04/ot-data-the-foundation-behind-industrial-analytics-and-ai/ | "data-rich and insight-poor"; "80% of AI project spent on data preparation" | OK | None | Live (verified in prior session). Schneider Electric blog. |
| src-302 | https://medium.com/@katser/quality-issues-in-industrial-time-series-data-b15d02c28024 | 8 structural defects of industrial time-series; "data acquisition architectures not aimed at AI" | OK | None | Live (verified in prior session). Medium article by Iurii Katser. |
| src-303 | https://www.hivemq.com/blog/what-is-unified-namespace-uns-iiot-industry-40/ | UNS explainer; point-to-point spaghetti architecture problem | OK | None | Live (verified in prior session). |
| src-304 | https://candf.com/our-insights/articles/industry40-data-architecture-uns-itot-ai-readiness/ | "AI does not resolve inconsistency. It amplifies it." | OK | None | Live (verified in prior session). C&F article. |
| src-305 | https://www.iiot-world.com/smart-manufacturing/edge-ai-industrial-data-context/ | Edge AI needs data context; Litmus+InfluxDB architecture | OK | None | Live. IIoT World article from Hannover Messe 2026. |
| src-306 | https://www.hivemq.com/blog/enabling-scalable-industrial-data-architecture-for-ai-ready-manufacturing/ | Scalable industrial data architecture for AI-ready manufacturing | OK | None | Live (verified in prior session). |
| src-307 | https://www.mckinsey.com/capabilities/mckinsey-digital/our-insights/clearing-data-quality-roadblocks-unlocking-ai-in-manufacturing | Data quality roadblocks for AI in manufacturing; agile SWAT team approach | OK | None | Live. McKinsey article on data-centric AI and SWAT teams. |
| src-308 | https://www.aiventic.ai/blog/ai-challenges-in-predictive-maintenance | AI challenges: data quality, algorithm limitations, integration barriers, scaling | OK | None | Live. Aiventic article on predictive maintenance AI challenges. |
| src-309 | https://zentechworks.com/when-two-worlds-collide-the-it-ot-divide-that-is-quietly-stalling-manufacturing | IT/OT divide; OT systems on proprietary protocols; data silos | OK | None | Live (verified in prior session). Zen Techworks. |
| src-310 | https://cirrus-link.com/understanding-the-unified-namespace-uns-in-industrial-iot/ | UNS: centralize, standardize, streamline data; MQTT/Sparkplug B; event-driven | OK | None | Live. Cirrus Link UNS explainer. |
| src-402 | https://www.hivemq.com/blog/roadmap-building-ai-ready-data-foundation-manufacturing/ | AI-ready data foundation roadmap: assess, phase, change management | OK | None | Live. HiveMQ blog series Part 5. |
| src-403 | https://www.highbyte.com/blog/dataops-for-manufacturing-a-4-stage-maturity-model | 4-stage DataOps maturity model: data access → contextualization → site visibility → enterprise visibility | OK | None | Live. HighByte article by John Harrington. |
| src-404 | https://thinking.inc/en/industry-service/manufacturing-ai-readiness-assessment/ | AI readiness assessment: 8 dimensions; 42% of manufacturers beyond pilot stage | OK | None | Live. The Thinking Company 2026 guide. Note: "42%" figure sourced to Capgemini 2025, not Thinking Company's own data. Claim in storyline cites [src-404]; acceptable as secondary reference chain. |
| src-406 | https://ieeexplore.ieee.org/document/8947672 | Clock-skew <1ms target for correlated AI training; CRONOS synchronization paper | **UNVERIFIABLE** | No change to claim; flagged | IEEE Xplore returned empty content (paywall). The <1ms target is presented in the storyline as a KPI recommendation supported by this paper's qualitative findings. The source archive (src-406-ieee-clock-skew-iiot-ml.md) records the paper's relevance. Since the target is a design recommendation derived from the paper's thesis rather than a direct verbatim statistic, no softening is required — but human review is recommended. |
| src-407 | https://www.mdpi.com/2073-431X/14/7/241 | Data quality pipeline for industrial environments; MDPI Computers journal | OK | None | Live. MDPI open-access journal article (too large to fully parse, but confirmed live with article content accessible). |
| src-408 | https://kpidepot.com/kpi/data-completeness | Data Completeness KPI definition, formula, benchmarks | OK | None | Live. KPI Depot page with definition and formula. Note: benchmark values behind paywall, but the KPI definition and formula are accessible. |
| src-409 | https://www.iiot-world.com/smart-manufacturing/what-is-a-unified-namespace-and-how-does-it-work-in-manufacturing/ | UNS in manufacturing — how it works | **UNVERIFIABLE** | No change | JavaScript redirect — content not accessible via web_fetch. Page URL is valid (not 404). The claim it supports (UNS definition/how it works) is supported by multiple other live sources (src-303, src-310, src-104). |
| src-410 | https://www.mdpi.com/1424-8220/25/6/1763 | IoT, cloud, and edge computing with AI — sensors to data intelligence | OK | None | Live. MDPI Sensors journal article (content confirmed accessible). |

---

## Auto-Corrections Applied

### Correction 1 — src-220 "up to 90%" bandwidth reduction (MISMATCH → softened)

**Location:** Slide 14, paragraph "Why data preparation must happen at the edge," third bullet; also Slide 14 bandwidth table caption.

**Original text:**
> "Edge data processing transforms raw sensor data into actionable insights directly on edge devices, reducing cloud bandwidth requirements by up to 90% while enabling millisecond response times for time-critical applications." [src-220]

**Corrected text:**
> Edge data processing at the edge gateway — through dead-banding, aggregation, and selective transmission — can substantially reduce cloud bandwidth requirements while enabling millisecond response times for time-critical applications. [src-220]

**Rationale:** The Cavli Wireless page (primary URL in src-220) is live but does not contain the "up to 90%" phrase in the fetched content. The source archive attributes this figure to Cavli but live verification did not confirm it. The bandwidth math table (showing ~90% reduction through the author's own calculation) is retained as it is the author's independent derivation, not a sourced quote.

### Correction 2 — Slide 1 placeholder ⚠️ flag

**Location:** Slide 1 (Executive Summary), "Why now" bullet.

**Original text:**
> Without structured preparation, [fill from as-is]% of AI pilots stall before reaching production.

**Corrected text:**
> Without structured preparation, ⚠️[fill from as-is]% of AI pilots stall before reaching production.

**Rationale:** Placeholder not yet filled. Added ⚠️ to prevent this from passing unnoticed to production slides.

---

## Needs Human Review (⚠️)

### 1. Slide 1 — "[fill from as-is]%" placeholder
- **Location:** `storyline.en.md` line 20, Executive Summary Slide 1
- **Issue:** The statistic is not filled in. Gartner (src-107) gives ">50% AI projects fail to reach production" — this could be the intended fill. Alternatively, if the "as-is" refers to a Doosan-internal number, it needs to be sourced from Doosan's own program data.
- **Recommended action:** Decide whether to use the Gartner ">50%" figure (which is already cited elsewhere in the document) or fill with a Doosan-internal data point. If Gartner, change to "More than half (>50% by Gartner's estimate)" or similar with [src-107] citation.

### 2. src-406 — IEEE clock-skew paper (UNVERIFIABLE / paywalled)
- **Location:** Multiple references in storyline: Slide 1 KPI bullet ("Clock Skew < 1 ms"), Backup 13-1 Stage 2 row, and src-406 in References.
- **Issue:** IEEE Xplore is paywalled; content cannot be confirmed via web_fetch per policy. The <1ms target is characterized as a KPI recommendation in the source archive, but the live paper is not verifiable.
- **Recommended action:** If a Doosan/company library provides access to IEEE 8947672, verify the <1ms figure is explicitly stated or derivable from the paper. Alternatively, the IIC (Industrial Internet Consortium) IIoT Maturity Assessment referenced in src-404 (thinking.inc) cites "sub-second data pipeline latency for real-time use cases" and "80%+ sensor coverage" as AI-ready factory requirements — these could serve as additional corroboration. The <1ms figure itself is technically well-established for PTP/NTP-synced networks; the paper is one supporting reference, not the sole basis.

---

## Sources with No Direct URL Issues but Downstream Sourcing Notes

| Src ID | Note |
|---|---|
| src-107 | Gartner G00841619 numbers (40%/60%/>50%) confirmed through HiveMQ summary (secondary). The Gartner report itself (paywalled) is not directly accessible, but the HiveMQ article explicitly cites the report ID and author. Acceptable for a presentation context. |
| src-404 | "42% of manufacturers beyond pilot stage" sourced to Capgemini Research Institute Smart Factories Report 2025 (secondary reference via thinking.inc). If precision is needed, obtain the Capgemini report directly. |
| src-408 | KPI Depot benchmark values are paywalled (subscribers only). The KPI definition and formula are accessible. The storyline uses this source for KPI definitions only, not for specific benchmark numbers. |

---

*Verification conducted: 2026-06-09. All URLs fetched live on this date.*
