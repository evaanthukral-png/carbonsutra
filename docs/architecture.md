# CarbonSutra technical architecture

## System boundary

CarbonSutra is a neighbourhood-level forecasting and decision-support layer. It does not claim to replace certified monitoring equipment; it forecasts from verified observations and clearly labels uncertainty.

## Core services

| Layer | Responsibility | MVP choice |
| --- | --- | --- |
| Ingestion | Pull, validate, and standardize hourly source data | Python scheduled jobs |
| Feature pipeline | Join location/time, generate lags and event signals | pandas + PostgreSQL |
| Forecast service | Produce pollutant and AQI forecasts | FastAPI + XGBoost |
| Explainability | Capture drivers for each forecast | feature importance / SHAP |
| API | Serve locality forecast, drivers, health advice | REST JSON |
| Client | Visualize current readings and 72-hour outlook | static web app; later React |

## Primary API contract

`GET /v1/forecast?locality=rohini&hours=72`

```json
{
  "locality": "Rohini, Delhi",
  "generated_at": "2026-10-31T18:00:00Z",
  "current": { "aqi": 186, "pm25": 98, "band": "Unhealthy" },
  "forecast": [{ "time": "2026-10-31T19:00:00Z", "aqi": 194, "pm25": 104, "confidence": 0.82 }],
  "drivers": [{ "name": "Low wind speed", "impact": "high" }]
}
```

## Data-quality guardrails

- Discard impossible negative pollutant values and duplicates.
- Flag readings stale for more than two hours.
- Fall back to nearby-station aggregation when a locality sensor is missing.
- Display a lower confidence score whenever key inputs are missing.
- Store source timestamp, station ID, and model version with each forecast.

## Scaling path

Begin with a scheduled batch forecast every hour for a limited set of localities. Move the feature pipeline to a queue-backed worker and partition time-series data by locality/date as coverage grows. Cache the latest forecast at the API edge.
