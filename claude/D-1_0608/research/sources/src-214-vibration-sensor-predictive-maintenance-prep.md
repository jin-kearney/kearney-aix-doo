# src-214: Vibration Sensor Data Prep for Predictive Maintenance

**URL:** https://ncd.io/blog/predictive-maintenance-with-vibration-sensors-the-complete-starter-guide/
**URL-2:** https://oxmaint.com/article/vibration-analysis-predictive-maintenance
**URL-3:** https://tractian.com/en/blog/vibration-sensor-predictive-maintenance
**URL-4:** https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12693812/
**Title:** Predictive Maintenance with Vibration Sensors: The Complete Starter Guide; Vibration Analysis for Predictive Maintenance
**AccessDate:** 2026-06-09
**Related section:** Easy-to-understand narrative (vibration sensor use case), sensor cleansing techniques

---

## Key Excerpts

**Vibration Data Preparation Pipeline:**
1. Signal acquisition (accelerometer, MEMS sensors, or piezoelectric sensors)
2. Oversampled acquisition with on-node denoising and enveloping
3. Denoising: wavelet-based denoising using adaptive soft-thresholding + detail coefficients
4. Savitzky-Golay filtering: polynomial smoothing that maintains transient features while reducing abrupt fluctuations
5. Sliding window segmentation: captures temporal correlations
6. Feature extraction: time-domain (RMS, peak, kurtosis), frequency-domain (FFT), envelope analysis

**Signal Processing Details:**
- PDRNet method (2025 paper): physical feature-driven residual network for motor vibration signal denoising
- Wavelet denoising: adaptive soft-thresholding isolates fault signatures and suppresses noise
- Savitzky-Golay filtering (polynomial smoothing): applied after wavelet denoising; maintains transient features

**Fault Frequency Analysis:**
- Each mechanical fault produces energy at predictable frequencies
- Bearing outer-race defects: BPFO (Ball Pass Frequency, Outer Race)
- Inner-race faults: BPFI (Ball Pass Frequency, Inner Race)
- Gearmesh faults: multiples of tooth-pass frequency

**Context-Aware Analytics:**
- "Operating region maps" and "mode-segmented features" aligned with real operating conditions
- Telematics data (shaft speed, load) provides context for interpreting vibration signatures
- Baseline informed by operating conditions — not just raw signal thresholds

**Manufacturing Example Pipeline (Step by Step):**
1. Mount accelerometer on bearing housing (or motor end cap)
2. Sample at high frequency (typically 5–25 kHz for bearing fault detection)
3. Apply anti-aliasing filter (hardware low-pass filter before ADC)
4. Edge node: wavelet denoising, envelope detection, calculate RMS/kurtosis over rolling window
5. Transmit aggregated features + periodic raw snapshots via MQTT to cloud historian
6. Cloud: build baseline during "known good" operation; train anomaly detection model
7. Deployment: compare live features to baseline; alert when deviation exceeds threshold

**Key Insight:**
"Early work in condition-based maintenance established a pipeline: signal acquisition → health indicator extraction → condition assessment → degradation forecasting. A combination of condition monitoring, vibration monitoring, machine learning, and analytics is paramount for a successful predictive strategy."
