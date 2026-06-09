# src-407 — MDPI Computers: A Data Quality Pipeline for Industrial Environments: Architecture and Implementation

**URL:** https://www.mdpi.com/2073-431X/14/7/241
**Title:** A Data Quality Pipeline for Industrial Environments: Architecture and Implementation
**AccessDate:** 2026-06-09
**Related section:** KPI (data quality pass rate, noise/outlier/gap)

---

## Key Excerpts

> Industrial data quality pipelines specifically adapted to industrial contexts include modular components responsible for data ingestion, profiling, validation, and continuous monitoring, guided by data quality dimensions including accuracy, completeness, consistency, and timeliness.

> Profiling results feed directly into metric calculations including null detection for completeness, outlier handling for accuracy, type checking for timeliness, and correlation analysis for consistency, allowing for a proactive approach and reducing manual intervention with automated, real-time responses to quality deviations.

> Cleansing noisy sensor data is one of the most fundamental and necessary steps for accurate data analysis. Without first removing the noise, anomaly detection techniques are likely to give a large number of false positives.

> IIoT sensor data is often highly corrupted with outliers and noise — caused by environmental disturbances, human interventions, and faulty sensors.

## Data Quality Pass Rate

A data quality pipeline computes a pass rate: the fraction of incoming sensor records that pass all validation checks (range, rate-of-change, null, type conformance).

Formula: `Quality Pass Rate = (records passing all checks) / (total records received) × 100%`

## Components of Industrial DQ Pipeline

1. Data ingestion (MQTT/OPC-UA/Modbus stream)
2. Profiling (statistical characterization)
3. Validation (rule-based checks: range, rate-of-change, null, type)
4. Cleansing (noise removal, outlier flagging, gap imputation)
5. Continuous monitoring (dashboards, alerts)

## Gap Handling

For missing values: context-aware interpolation based on sensor type and operational characteristics.
