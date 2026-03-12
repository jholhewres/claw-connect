# Google Analytics Data Action Reference

**Provider:** `google`
**Service:** `analytics-data`
**App name:** `google-analytics-data`

## Actions

### run_report

Run a standard GA4 report with dimensions and metrics. Dimensions and metrics are passed as simple string arrays — the handler converts them to the GA4 API format internally.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "analytics-data",
    "action": "run_report",
    "params": {
      "property_id": "123456789",
      "start_date": "2024-01-01",
      "end_date": "2024-01-31",
      "dimensions": ["country", "pagePath"],
      "metrics": ["sessions", "activeUsers"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `property_id` | string | yes | GA4 property ID |
| `start_date` | string | yes | Report start date (YYYY-MM-DD) |
| `end_date` | string | yes | Report end date (YYYY-MM-DD) |
| `dimensions` | array | no | Dimension names as strings (e.g. `["country", "pagePath", "date"]`) |
| `metrics` | array | yes | Metric names as strings (e.g. `["sessions", "activeUsers"]`) |
| `limit` | integer | no | Max rows to return (default 100000) |
| `offset` | integer | no | Row offset for pagination (default 0) |

### run_realtime_report

Run a realtime report for live traffic data. Dimensions and metrics are passed as simple string arrays.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "analytics-data",
    "action": "run_realtime_report",
    "params": {
      "property_id": "123456789",
      "dimensions": ["country"],
      "metrics": ["activeUsers"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `property_id` | string | yes | GA4 property ID |
| `dimensions` | array | no | Dimension names as strings (e.g. `["country"]`) |
| `metrics` | array | no | Metric names as strings (e.g. `["activeUsers"]`) |
| `limit` | integer | no | Max rows to return (default 100000) |

### get_metadata

Retrieve available dimensions and metrics for a property.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "analytics-data",
    "action": "get_metadata",
    "params": {"property_id": "123456789"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `property_id` | string | yes | GA4 property ID |

## Common Dimensions & Metrics

- **Dimensions:** `country`, `date`, `pagePath`, `sessionSource`, `deviceCategory`
- **Metrics:** `sessions`, `activeUsers`, `screenPageViews`, `conversions`
