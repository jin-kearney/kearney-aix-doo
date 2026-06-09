# src-408 — KPI Depot: Data Completeness — KPI Definition, Formula & Benchmarks

**URL:** https://kpidepot.com/kpi/data-completeness
**Title:** Data Completeness — KPI Definition, Formula, & Benchmarks
**AccessDate:** 2026-06-09
**Related section:** KPI (completeness / availability benchmark)

---

## Key Excerpts

> High values in data completeness indicate robust data management practices, while low values suggest potential gaps in data collection or integrity.

## Benchmark Thresholds

| Level | Completeness % | Interpretation |
|---|---|---|
| Excellent | ≥ 95% | Robust data management; ready for AI/ML |
| Acceptable | 85–95% | Functional but requires review and improvement |
| Concerning | < 85% | Immediate action required; AI outputs unreliable |

## Formula

`Data Completeness (%) = (Number of complete records / Total expected records) × 100`

For time-series sensor data:
`Completeness = (Samples received / Expected samples in period) × 100`

## Relevance

- Sensor data completeness is a direct proxy for sensor health and network reliability.
- Low completeness → AI training sets with systematic gaps → biased or unreliable models.
- Target direction: ↑ (higher is better)
