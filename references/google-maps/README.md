# Google Maps Action Reference

**Provider:** `google`
**Service:** `maps`
**App name:** `google-maps`

## Actions

### search_places

Search for places by text query.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "maps",
    "action": "search_places",
    "params": {
      "query": "restaurants in São Paulo",
      "max_results": 5,
      "language_code": "pt-BR"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | yes | Text query (e.g. "restaurants in São Paulo", "123 Main Street") |
| `max_results` | integer | no | Maximum results (1-20, default 10) |
| `language_code` | string | no | Language for results (e.g. `pt-BR`, `en`) |
| `region_code` | string | no | Region bias (e.g. `BR`, `US`) |
| `latitude` | number | no | Center latitude for location bias |
| `longitude` | number | no | Center longitude for location bias |
| `radius` | number | no | Radius in meters for location bias (default 50000) |

### search_nearby

Search for places near a location.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "maps",
    "action": "search_nearby",
    "params": {
      "latitude": -23.5505,
      "longitude": -46.6333,
      "radius": 1000,
      "included_types": ["restaurant"],
      "max_results": 10
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `latitude` | number | yes | Center latitude |
| `longitude` | number | yes | Center longitude |
| `radius` | number | yes | Search radius in meters (max 50000) |
| `included_types` | array | no | Place types to include (e.g. `restaurant`, `gas_station`, `hospital`) |
| `excluded_types` | array | no | Place types to exclude |
| `max_results` | integer | no | Maximum results (1-20, default 10) |
| `language_code` | string | no | Language for results |

### get_place

Get details about a specific place.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "maps",
    "action": "get_place",
    "params": {
      "place_id": "ChIJN1t_tDeuEmsRUsoyG83frY4",
      "language_code": "en"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `place_id` | string | yes | Place ID (with or without `places/` prefix) |
| `language_code` | string | no | Language for results |

### autocomplete

Get place predictions as the user types.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "maps",
    "action": "autocomplete",
    "params": {
      "input": "restaurante japonês",
      "language_code": "pt-BR",
      "included_region_codes": ["BR"]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `input` | string | yes | Text input for predictions |
| `language_code` | string | no | Language for results |
| `region_code` | string | no | Region bias (e.g. `BR`) |
| `included_region_codes` | array | no | Restrict to these regions |
| `latitude` | number | no | Center latitude for location bias |
| `longitude` | number | no | Center longitude for location bias |
| `radius` | number | no | Radius in meters for location bias |

### compute_routes

Compute directions between origin and destination.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "maps",
    "action": "compute_routes",
    "params": {
      "origin_latitude": -23.5505,
      "origin_longitude": -46.6333,
      "destination_latitude": -22.9068,
      "destination_longitude": -43.1729,
      "travel_mode": "DRIVE"
    }
  }'
```

Using place IDs:

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "maps",
    "action": "compute_routes",
    "params": {
      "origin_place_id": "ChIJN1t_tDeuEmsRUsoyG83frY4",
      "destination_place_id": "ChIJW6AIkVXemwARTtIvZ2xC3FA",
      "travel_mode": "DRIVE"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `origin_latitude` | number | no* | Origin latitude |
| `origin_longitude` | number | no* | Origin longitude |
| `origin_place_id` | string | no* | Origin place ID (alternative to lat/lng) |
| `destination_latitude` | number | no* | Destination latitude |
| `destination_longitude` | number | no* | Destination longitude |
| `destination_place_id` | string | no* | Destination place ID (alternative to lat/lng) |
| `travel_mode` | string | no | `DRIVE`, `BICYCLE`, `WALK`, `TRANSIT` (default `DRIVE`) |
| `compute_alternative_routes` | boolean | no | Compute alternative routes (default false) |
| `language_code` | string | no | Language for instructions |
| `units` | string | no | `METRIC` or `IMPERIAL` (default `METRIC`) |

*Provide either lat/lng or place_id for origin and destination.

### compute_route_matrix

Compute distances between multiple origins and destinations.

```bash
curl -s -X POST "https://integraclaw.dev/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "maps",
    "action": "compute_route_matrix",
    "params": {
      "origins": [
        {"latitude": -23.5505, "longitude": -46.6333},
        {"latitude": -22.9068, "longitude": -43.1729}
      ],
      "destinations": [
        {"latitude": -25.4284, "longitude": -49.2733},
        {"latitude": -19.9167, "longitude": -43.9345}
      ],
      "travel_mode": "DRIVE"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `origins` | array | yes | List of `{"latitude": N, "longitude": N}` objects |
| `destinations` | array | yes | List of `{"latitude": N, "longitude": N}` objects |
| `travel_mode` | string | no | `DRIVE`, `BICYCLE`, `WALK`, `TRANSIT` (default `DRIVE`) |

## Place Types

Common place types for `included_types` / `excluded_types`:

`restaurant`, `cafe`, `bar`, `hotel`, `gas_station`, `parking`, `hospital`, `pharmacy`, `school`, `university`, `bank`, `atm`, `supermarket`, `shopping_mall`, `gym`, `park`, `museum`, `airport`, `train_station`, `bus_station`, `church`, `police`, `fire_station`, `post_office`, `library`, `movie_theater`, `dentist`, `doctor`, `veterinary_care`

Full list: [Google Maps Place Types](https://developers.google.com/maps/documentation/places/web-service/place-types)

## Notes

- This integration uses the **Places API (New)** and **Routes API** — the latest Google Maps Platform APIs
- Place IDs from search results can be used directly with `get_place` for full details
- The `search_places` action can also be used for geocoding (searching by address returns coordinates)
- The OAuth scope `cloud-platform` is required — users must enable Places API and Routes API in their Google Cloud Console
- The user's Google Cloud project must have billing enabled for Maps Platform APIs

## Resources

- [Places API (New) Reference](https://developers.google.com/maps/documentation/places/web-service/op-overview)
- [Routes API Reference](https://developers.google.com/maps/documentation/routes)
- [Place Types](https://developers.google.com/maps/documentation/places/web-service/place-types)
