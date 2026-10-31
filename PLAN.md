# CarbonSutra — Round 1 plan

## What is built now

The Round 1 frontend is an interactive **Neighbourhood Pollution Digital Twin concept**. It uses four hand-authored synthetic locality scenarios, a time slider, clickable zones, an AQI curve, peak timing, sample PM2.5/PM10 proxies, and coverage/confidence states.

## Truthful scope

- No live pollution feeds, station mapping, trained ML model, real confidence scores, or alert delivery exist.
- The locality map is illustrative, not a geographic claim or proof of hyperlocal monitoring coverage.
- AQI bands follow the Indian CPCB category boundaries; PM2.5 and PM10 values are interface-only proxies.

## Next implementation milestones

1. Confirm a permitted air-quality dataset, weather source, and station coverage.
2. Build a station-level persistence baseline and evaluate it chronologically.
3. Record source timestamps, missingness, station distance, and data quality.
4. Add candidate event-aware forecasting only after its error improves on the baseline.
5. Calibrate prediction intervals before displaying confidence, then build opt-in alerts.

## Demo script

Select a neighbourhood, drag from 6 PM into the next morning, then click a second zone. Explain that the map exposes **time + location + uncertainty**, rather than just reporting one city-wide AQI after pollution has already peaked.
