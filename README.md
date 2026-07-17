# Deepal S05 / S07 BMS DID Mapping

> Working notes from OBD investigations on Changan Deepal S05.
> Header used for all identified DIDs: `7A1`

## Identified PID/DIDs

| DID | Value Observed | Meaning | Status |
|------|------:|----------|---------|
| F260 | 8000 | CC Resistance | Identified |
| F261 | 8000 | CC2 Resistance | Identified |
| F27C | 16880 | Shunt Initial Resistance (R0) | Identified |
| F264 | 27389 | Insulation Resistance | Identified | (dynanmic value)
| F22F |   100 | SOC direct | Identified |
| F228 | 399.9 | Voltage DC Pack | Identified |
| F229 | -0.5 | Current DC Pack | Identified |
| F250 | 3282 | Voltage Max Cell | Identified |
| F251 | 3280 | Voltage Min Cell | Identified |
| F252 | 39 | Temperature Max Cell | Identified |
| F253 | 38 | Temperature Min Cell | Identified |
| F3E0 |  | Voltage Cells Array | |
| F45B |  | SOC gross (255 @ 100%) | | 
| F4B9 |  | Voltage Cells (Max/Min)? | |

## Unidentified DIDs

| DID | Notes |
|------|-------|
| F230 | |
| F231 | |
| F233 | |
| F234 | |
| F239 | |
| F254 | |
| F255 | |
| F256 | |
| F257 | |
| F262 | |
| F27D | Constant value observed across captures |
| F27E | Constant value observed across captures |
| F27F | Constant value observed across captures |

These three DIDs were initially considered possible SOH-related parameters, but no evidence currently supports that assumption.

## Car Scanner Sensors Mapped

| Car Scanner Sensor | DID |
|-------------------|-----|
| CC Resistance | F260 |
| CC2 Resistance | F261 |
| Shunt Initial Resistance (R0) | F27C |
| Insulation Resistance | F264 

## Car Scanner Sensors NOT Mapped|

| Car Scanner Sensor | DID |
|-------------------|-----|
| [BMS] Battery Total Voltage | ? |
| [BMS] Battery Total Current | ? |
| [BMS] Battery Cell Max Voltage | F228? |
| [BMS] Battery Cell Min Voltage | F229? |
| [BMS] Battery Max Temperature | F252? |
| [BMS] Battery Min Temperature | F253? |
| ECU voltage | ? |
| Distance since last SOH update | ? |

## Notes

### F264

Originally investigated as a possible energy or SOH-related parameter.

Current evidence indicates:

```text
F264 = Insulation Resistance
```
Current Headers with data (service 22):

| Header (Service 22) | Meaning |
|-------------------|----------|
| 7A1 | BMS principal |
| 7A4 | BMS auxiliar |
| 7A7 | HV Module - battery or termical management? |
| 7F1 | Gateway |
| 731 | VCU - Vehicle Control Unit |
| 742 | MCU - Motor Control Unit |
| 744 | OBC - Charging Module? |

## 2DO
 - Obtain additional parameters like SOH, EFC, ...
 - Conclude the mapping
