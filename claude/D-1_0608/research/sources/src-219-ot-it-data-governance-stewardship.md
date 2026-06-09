# src-219: OT Data Governance and Data Stewardship in Manufacturing

**URL:** https://en.pailot.com/blog/data-steward
**URL-2:** https://www.ovaledge.com/blog/data-governance-in-manufacturing
**URL-3:** https://cimsoft.com/2026/02/18/industrial-data-trust-sensor-validation/
**URL-4:** https://www.capgemini.com/us-en/insights/research-library/enabling-intelligent-manufacturing-with-an-ot-data-foundation/
**Title:** Data Steward in Manufacturing; Data Governance in Manufacturing; Industrial Sensor Data Validation
**AccessDate:** 2026-06-09
**Related section:** Operations & management (who curates, refresh on equipment change, periodic quality checks)

---

## Key Excerpts

**Data Steward Role:**
- "A data steward is responsible for managing, monitoring and maintaining company data — acting as the guardian of data quality"
- In manufacturing: incorrect or incomplete data can lead to production downtime, rework or even safety risks
- Data stewards: implement validation processes, clean up incorrect data records, carry out random checks to ensure digital information matches physical reality

**Governance Structure:**
- Governance responsibilities span: production teams, plant managers, engineering teams, compliance teams, and IT teams
- "Named data stewards are designated for each operational domain with documented responsibilities"

**The OT Data Gap:**
- "OT data often falls outside enterprise resource planning (ERP) activities and is not addressed by typical IT data governance"
- Critical: "it's important from the outset to assign ownership and accountability for maintaining the quality of OT data"
- OT data lacks the IT governance frameworks applied to business data

**Platform Team vs. Business Owner Split:**
- Platform Team: ensures the platform provides good quality data, easy access for all users; leads governance activities
- Business owners: own the data — the responsibility for data content lies with the business, not IT
- Clear separation of responsibilities prevents "nobody's job" gaps

**Who Updates What (Refresh Cadence):**
- Equipment change (e.g., sensor replacement, motor swap): OT engineer updates tag mapping, resets baseline
- Calibration updates: maintenance team records new calibration factors; data steward triggers re-validation
- Protocol/network changes: IT team updates connectivity config; OT team verifies data continuity
- Periodic quality audits: monthly/quarterly check of data completeness, outlier rates, timestamp accuracy

**Industrial Sensor Data Validation (CIMSoft, Feb 2026):**
- "Sensor trust" framework: each sensor has a validated operating range, expected behavior model, and confidence score
- Validation rules: range check, rate-of-change check, cross-sensor plausibility check (physics-based)
- Sources: https://cimsoft.com/2026/02/18/industrial-data-trust-sensor-validation/

**Capgemini Research — OT Data Foundation:**
- "Enabling Intelligent Manufacturing with an OT Data Foundation"
- OT data governance needs to be treated as a first-class concern alongside IT data governance
- Recommends: OT Data Council with representatives from operations, engineering, IT
- Source: https://www.capgemini.com/us-en/insights/research-library/enabling-intelligent-manufacturing-with-an-ot-data-foundation/
