# Deepal S05 – community candidate for ABRP OBD

This is a cleaned starter profile intended for community testing.

## Included signals
- SOC direct (`22F22F`, header `7A1`)
- Pack voltage (`22F228`, header `7A1`)
- Pack current (`22F229`, header `7A1`)
- Derived pack power (`voltage * current / 1000`)
- SOH candidate (`22F27D`, header `7A1`)
- Cell max/min voltage (`22F250`, `22F251`)
- Battery max/min temperature (`22F252`, `22F253`)
- Charge temperature candidate (`22F255`)

## Explicitly experimental
- `22F264` as available energy / Et sum
- Any lifetime charged-energy counter
- Any odometer signal
- Any real EFC based on OBD-only lifetime totals

## Practical note
Keep the legacy `22F264` entries only for comparison/debugging. They should not be treated as validated lifetime counters.