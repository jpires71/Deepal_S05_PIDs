# Deepal S05 OBD Telemetry Reference, DID/PID Validation and Battery Analysis

## Abstract

This repository documents the reverse engineering, validation and practical use of Deepal S05 OBD telemetry.

The project focuses on:

- DID/PID discovery
- Formula validation
- Dashboard correlation
- CSV export validation
- Battery health assessment
- Cell balancing analysis
- Thermal analysis
- Energy analysis
- BMS behaviour characterization

All measurements were collected using:

- Deepal S05
- Vgate iCar Pro 2S
- Car Scanner ELM OBD2
- Custom Deepal S05 profile

The goal of this repository is not limited to battery health estimation. It aims to provide a reproducible technical reference for Deepal S05 owners interested in understanding and validating vehicle telemetry.

## Current Findings

| Metric | Value |
|----------|----------|
| SOH Candidate | 98.4–98.5% |
| EFC Candidate | 14.67 |
| Estimated Capacity | 68 kWh |
| Real Cell Delta (CSV) | ~2-3 mV |
| Pack Voltage | ~399 V |
| Thermal Delta | ~1°C |
| Battery Condition | Excellent |

Recent analysis suggests:

- SOH remains stable near 98.5%
- Battery ageing is minimal
- Cell voltage imbalance is extremely low
- No evidence of weak cells
- No evidence of abnormal degradation
- No evidence of thermal imbalance
- No evidence of balancing issues

---

# Project Scope

This repository aims to:

- Discover undocumented Deepal S05 DIDs/PIDs
- Validate telemetry formulas
- Correlate dashboard values with raw BMS data
- Validate CSV exports
- Estimate battery health
- Characterize thermal management behaviour
- Identify useful metrics for ABRP and telemetry integrations
- Build a community-maintained Deepal S05 telemetry reference

---

# Test Environment

## Vehicle

- Deepal S05
- CATL LFP battery
- Gross battery: 68.8 kWh
- Usable battery: ~68 kWh
- Odometer during testing: ~5,000 km

## OBD Interface

- Vgate iCar Pro 2S
- Bluetooth connection

## Software

- Car Scanner ELM OBD2
- Custom Deepal S05 profile

## Data Sources

Validation is based on:

- Dashboard telemetry
- Live OBD telemetry
- CSV exports
- Charging sessions
- Driving sessions
- Overnight parking
- Post-drive observations
- Repeated measurements

---

# Validation Methodology

Each DID/PID is assigned a validation level.

Validation techniques include:

- Dashboard correlation
- CSV validation
- Charge/discharge consistency
- Session repeatability
- Thermal consistency
- BMS value correlation

Validation levels:

| Status | Definition |
|----------|----------|
| Observed | Discovered but not validated |
| Candidate | Strong evidence available |
| Validated | Repeatedly confirmed |
| Fully Validated | Confirmed through multiple independent methods |

---

# PID Validation Status

## ✅ Fully Validated

### SOC Direct

PID:

```text
22F231
```

Formula:

```text
(A*256+B)/10
```

Status:

```text
Validated
```

Notes:

- Matches BMS reported SoC.
- Stable across charging sessions.
- Stable across driving sessions.
- Matches dashboard values.

---

### Battery Pack Voltage

PID:

```text
22F228
```

Status:

```text
Validated
```

Notes:

- Consistent across charging and driving conditions.
- Matches expected pack behaviour.

---

### Battery Pack Current

PID:

```text
22F229
```

Status:

```text
Validated
```

Notes:

- Consistent with observed charging/discharging activity.

---

### Battery Pack Power

PID:

```text
22F236
```

Status:

```text
Validated
```

Notes:

- Consistent with voltage × current calculations.
- Matches observed vehicle power behaviour.

---

# ✅ SOH Candidate (Strong Evidence)

## PID

```text
22F264
```

## Formula

```text
SOH = 100 - A/68
```

## Validation

Recorded during:

- Vehicle charging
- Vehicle parked
- Vehicle driving
- SOC ranging from ~33% to 100%

Observed values:

```text
98.32%
98.47%
98.49%
98.53%
98.54%
```

Statistical analysis:

```text
Average SOH:
98.514%

Standard deviation:
0.0516%
```

Current observed value:

```text
98.49%
```

Interpretation:

- Highly stable
- Independent of SOC
- Consistent with vehicle behaviour

Supporting observations:

```text
SOCE = Excellent
E_real_teo = 68 kWh
Range = 485 km
```

Status:

```text
Strong SOH Candidate
```

---

# ✅ Capacity Tracking

## PID

```text
E_real_teo
```

Observed value:

```text
68 kWh
```

Notes:

- Stable across all measurements.
- Consistent with minimal battery degradation.
- Supports SOH estimates.
- Consistent with observed range figures.

---

# 🟡 EFC Candidate

## PID

```text
22F27F
```

Current formula under validation:

```text
(A*16777216+B*65536+C*256+D)
/1000000
/68
```

Current result:

```text
14.67 EFC
```

Vehicle mileage:

```text
4980 km
```

Independent manual estimate:

```text
~13.xx EFC
```

Status:

```text
Promising
Requires additional validation
```

Notes:

- Provides values consistent with manually calculated EFC.
- Significantly more realistic than earlier formulas.
- Appears correlated with vehicle usage.

---

# 🟡 Cell Delta

## Parameter

```text
V_cells_delta
```

Observed values:

```text
0.09 V
0.092 V
0.10 V
0.10 V
```

Observed during:

- Charging
- Full charge
- Vehicle parked
- Vehicle driving

Observations:

- Surprisingly stable.
- Does not correlate with battery degradation.
- Does not appear to follow expected Vmax-Vmin behaviour.

Battery simultaneously reports:

```text
SOCE = Excellent
SOH ≈ 98.5%
Range = 485 km
E_real_teo = 68 kWh
```

Current interpretation:

```text
Unknown
Requires further investigation
```

Possible explanations:

- Normal LFP behaviour
- BMS-derived metric
- Aggregated balancing indicator
- Not a direct Vmax-Vmin calculation

---

## Recent CSV Validation

Additional overnight-rest CSV analysis suggests:

```text
Vmax ≈ 3.331-3.332 V
Vmin ≈ 3.328-3.329 V
Actual delta ≈ 0.002-0.003 V
```

This measured delta is significantly lower than the value exposed by the current V_cells_delta parameter.

Current hypothesis:

```text
V_cells_delta is likely not a direct
Vmax-Vmin calculation.
```

Status:

```text
Under Investigation
```

---

# ✅ Thermal Management Observations

Typical values observed:

Battery:

```text
24°C - 38°C
```

Coolant:

```text
25°C
```

Temperature delta:

```text
1°C - 2°C
```

Observations:

- Coolant temperature closely tracks battery temperature.
- Charging performance matches expected DC charging curves.
- No evidence of thermal throttling.
- Suggests highly effective active thermal management.

---

# SOCE

Current vehicle software:

```text
3.0.3
```

SOH display removed.

BMS instead reports:

```text
SOCE = Excellent
```

Current evidence suggests:

```text
SOCE is likely a simplified battery-health
classification rather than a direct SOH value.
```

---

# Engineering Findings

## Battery Health

Current evidence indicates:

```text
SOH ≈ 98.5%
```

Estimated degradation:

```text
≈1.5%
```

Estimated usable capacity:

```text
≈67-68 kWh
```

Observed battery behaviour is consistent with a battery exhibiting minimal ageing.

---

## Cell Balancing

Recent CSV analysis indicates:

```text
Vmax ≈ 3.331-3.332 V
Vmin ≈ 3.328-3.329 V
Real cell delta ≈ 2-3 mV
```

Assessment:

- Excellent balancing performance
- No signs of weak cells
- No evidence of abnormal divergence
- Behaviour consistent with a healthy pack

---

## Thermal Behaviour

Observed:

```text
Battery: 24-38°C
Coolant: ~25°C
Delta: ~1-2°C
```

Assessment:

- Excellent thermal uniformity
- No thermal anomalies detected
- No evidence of thermal throttling

---

## Operational Stability

Measurements have been collected during:

- Charging
- Driving
- Overnight parking
- Immediately after parking

Assessment:

```text
Stable across all observed conditions.
```

---

# Current Conclusions

## Battery Health

Estimated:

```text
SOH ≈ 98.5%
```

Estimated usable energy:

```text
≈67-68 kWh
```

Estimated degradation:

```text
≈1.5%
```

---

## Confidence Table

| PID | Description | Confidence |
|------|------|------|
| F231 | SOC | ⭐⭐⭐⭐⭐ |
| F228 | Pack Voltage | ⭐⭐⭐⭐⭐ |
| F229 | Pack Current | ⭐⭐⭐⭐⭐ |
| F236 | Pack Power | ⭐⭐⭐⭐⭐ |
| F264 | SOH Candidate | ⭐⭐⭐⭐☆ |
| F27F | EFC Candidate | ⭐⭐⭐☆☆ |
| V_cells_delta | Under Investigation | ⭐⭐☆☆☆ |

---

# Overall Assessment

Current evidence indicates:

✅ SOH ≈ 98.5%

✅ Capacity ≈ 68 kWh

✅ Estimated degradation ≈ 1.5%

✅ EFC candidate ≈ 14.67

✅ Real cell delta ≈ 2-3 mV

✅ SOCE = Excellent

✅ Temperature delta ≈ 1-2°C

No evidence currently suggests:

- Abnormal degradation
- Weak cells
- Balancing faults
- Thermal management issues

## Battery Condition

```text
EXCELLENT
```

Confidence level:

```text
HIGH
```

---

# Contributions

Additional logs, captures and validation data are welcome.

Please provide:

- Vehicle model
- Software version
- Battery type
- Odometer
- Raw PID values
- Calculated values
- CSV exports when possible

---

# Future Work

Planned validation activities:

- Additional DID/PID discovery
- Additional formula validation
- High-load discharge analysis
- Regenerative braking analysis
- Long-term SOH tracking
- Long-term EFC validation
- Additional BMS telemetry mapping
- Additional CSV-based studies
- Community-contributed validation datasets

---

# Changelog

## v1.0

- Initial PID discovery
- Formula validation
- SOH candidate identification
- EFC candidate identification
- Battery analysis
- Thermal analysis
- CSV validation
- Cell balancing investigation

## Planned

- Expanded PID coverage
- Additional BMS telemetry mapping
- Regenerative braking analysis
- High-load testing
- Long-term degradation tracking
- Community dataset integration
