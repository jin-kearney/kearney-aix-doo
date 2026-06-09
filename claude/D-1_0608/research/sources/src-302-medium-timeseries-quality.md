# src-302: Quality Issues in Industrial Time-Series Data

**URL:** https://medium.com/@katser/quality-issues-in-industrial-time-series-data-b15d02c28024
**Title:** Quality Issues in Industrial Time-Series Data
**Source:** Medium — Iurii Katser (Lead Data Scientist, Ph.D.)
**AccessDate:** 2026-06-09
**Related section:** Why — structural problems of raw sensor data (concise taxonomy)

---

## Key Excerpts

> "Problems in the data (such as outliers, gaps, sample rate variability, noise) can become a barrier (distort results, make application impossible) when applying machine learning methods and building domain/physics-based models."

> "Data acquisition and systems' architectures, including the installation of sensors, were not aimed at collecting data for AI. The systems solved their local tasks (incident analysis, diagnostics, reporting, manual process monitoring, regulatory documentation requirements), therefore data collection is not optimally organized."

> "Volume beats quality in most cases." (common industrial saying—even with large amounts of OT data, quality is the limiting factor)

## Taxonomy of Industrial Time-Series Quality Issues

### Signal processing issues:
- Missing values (data loss) — gaps in the sequence of points
- Abrupt changes — changing the statistical model (process change, sensor replacement)
- Range changes and data drift — abrupt or slow value range shifts
- Signal alterations — signals "trade places"

### Data acquisition issues:
- Absence or changing sample rate — makes time-series analysis methods inapplicable; regular time grid is required
- Noisy data and inconsistent noise level
- Low uniqueness of measurements — due to rounding or high aperture value
- Outliers and impossible values

### Statistical model changes:
- Concept drift

### Machine learning perspective:
- Short data history
- Class imbalance
- Units of Measurements — "units of measurement are not the same for all signals or data sources, e.g. cm vs. inch"
- Time Synchronization — "timestamps of measurements incoming from different sources might slightly differ, e.g. UTC+0 vs UTC+3"
- Different signal scales
- Sparse data
- Multicollinearity

> "The data pre-processing stage in the ML project pipeline becomes urgent to provide high-quality results for the problem solving and even applicability of some machine learning methods."
