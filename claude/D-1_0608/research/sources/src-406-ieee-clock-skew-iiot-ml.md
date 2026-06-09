# src-406 — IEEE Xplore: Real Time Empirical Synchronization of IoT Signals for Improved AI Prognostics

**URL:** https://ieeexplore.ieee.org/document/8947672
**Title:** Real Time Empirical Synchronization of IoT Signals for Improved AI Prognostics
**AccessDate:** 2026-06-09
**Related section:** KPI (time-sync / clock skew metric)

---

## Key Excerpts

> A significant challenge for Machine Learning prognostic analyses is variable clock skew between multiple data acquisition systems, where large numbers of sensors with their own internal clocks can create significant clock-mismatch issues.

> Clock skew issues in timestamps for time series telemetry signatures cause poor performance for ML prognostics, resulting in high false-alarm and missed-alarm probabilities.

> Due to manufacturing variances and environmental influences, no two clocks generate pulses at identical frequencies, which gives rise to timing discrepancies.

## Relevance to KPI

- Clock skew (time offset between sensor clocks) directly degrades ML model performance.
- Timestamp synchronization is a prerequisite for multi-sensor correlation and AI training.
- Measured as: max clock drift (ms or µs) across sensor network relative to a master clock (e.g., NTP/PTP reference).
- Target: clock skew < 1 ms for correlated time-series AI training; < 100 µs for real-time control.

## Solution Approach

CRONOS post-hoc approach estimates the skew and offset between sensor clocks and adjusts timing accordingly using ML-predicted slope and intercept parameters.
