# Deepal S05 OBD-II PID Research

Community reverse-engineering project for the Deepal S05 (Changan Deepal S05).

The objective of this repository is to identify, validate and document BMS, powertrain, thermal management and battery-health related PIDs for use with Car Scanner, ABRP and custom telemetry solutions.

---

# Vehicle Under Test

- Deepal S05
- CATL LFP battery
- Gross battery: 68.8 kWh
- Usable battery: ~68 kWh
- Odometer during testing: ~5,000 km

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
- Stable*across*charging and driving sessions.

--*

### Battery Pack Voltage

PID:

*``text
22F228
```

Status:

```tex*
Validated
```

*--

###*Battery Pack Current

PID:

```tex*
22F229
```

Status:

```text
Vali*ated
```

---

### Battery Pack Po*er

PID:

```text
22F236
```

Stat*s:

```text
Validated
```

Notes:
*- Consistent with voltage × curren* calculations.

---

# ✅ SOH Candi*ate (Strong Evidence)

## PID

```*ext
22F264
```

## Formula

```tex*
SOH = 100 - A/68
*`*

## Validation

Recorded during:
*- Vehicle charging
- Vehicle parke*
- Vehicle driving
- SOC ranging f*om ~33% to 100%

Observed values:
*```text
98.32%
98.47%
98.49%
98.53*
98.54%
```

Statistical analysis:*
```text
Average SOH:
98.514%

Sta*dard deviation:
0.0516%
```

*urrent observed value:

```text
98*49%
``*

Interpretation:

- Highly stable*
-*Independent*of SOC.
- Consistent with:

```*ext
SOCE = Excellent
E*real*teo = 68 kWh
Range = 485 km
*``

Status:

```text
Strong SOH ca*didate
```

---

# ✅ Capacity Trac*ing

## PID

```text
E_real_teo
``*

Observed value:

```text
68 kWh
*``

Notes:

- Stable over all meas*rements.
- Consistent with a batte*y showing minimal degradation.
- S*pports the estimated SOH values.

*--

# 🟡 EFC Candidate

## PID

``*text
22F27F
```

Current formula u*der validation:

```text
(A*16777216+B*65536+C*256+D)
/
1000000
/
68
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
- Significantly more realistic than previously tested formulas.

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
- Battery continues to report:

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

- Normal LFP behavior.
- BMS-derived metric.
- Not a simple Vmax-Vmin calculation.

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
SOCE is likely a simplified battery-health classification rather than a direct SOH value.
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
| V_cells_delta | Unknown | ⭐⭐☆☆☆ |

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
