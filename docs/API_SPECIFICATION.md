# API Specification

> **Status**: Conceptual design — not yet implemented. Detailed OpenAPI specifications will be created during implementation.

## Overview

KOLEKTA exposes a **REST API** (NestJS) with **WebSocket** support for real-time updates. The API serves three primary consumers:

1. **GPS data sources** (operators / devices) — push vehicle positions into the system.
2. **Citizen-facing frontend** — read vehicle positions, predictions, and subscribe to notifications.
3. **Operator dashboards** (future) — view fleet analytics and route performance.

---

## API Design Principles

| Principle | Detail |
|-----------|--------|
| Resource-oriented REST | Endpoints represent resources (`/vehicles`, `/predictions`), not actions |
| JSON everywhere | All request and response bodies use `application/json` |
| Consistent errors | Every error follows the standard format (see [Error Format](#error-format)) |
| URL-based versioning | All endpoints are prefixed with `/api/v1/` |
| Cursor-based pagination | For time-series data (GPS positions, trajectories) |
| Offset-based pagination | For entity lists (vehicles, operators) |
| Rate limiting | Applied to all public endpoints (see [Rate Limiting](#rate-limiting)) |
| HTTPS only | All production traffic must be encrypted |

---

## Authentication & Authorization

> [!IMPORTANT]
> ⏳ The authentication strategy is **PENDING** — specific implementation details (JWT, OAuth2, API key format) have not been finalized.

### Current Design Intent

| Consumer | Auth Method | Notes |
|----------|-------------|-------|
| GPS ingestion (operator devices) | API key per operator | Included in `Authorization` header or custom `X-API-Key` header |
| Citizen-facing endpoints | None (anonymous) | No authentication required in MVP |
| Operator dashboard (future) | Token-based authentication | ⏳ Strategy TBD |
| Admin/system endpoints | Internal only | Not exposed publicly; secured by network policy |

---

## Error Format

All API errors return a consistent JSON structure:

```json
{
  "error": "BadRequest",
  "message": "Parameter 'lat' is required and must be a valid latitude (-90 to 90).",
  "statusCode": 400
}
```

### Standard HTTP Error Codes

| Code | Meaning | When Used |
|------|---------|-----------|
| `400` | Bad Request | Invalid parameters, malformed request body |
| `401` | Unauthorized | Missing or invalid API key / token |
| `403` | Forbidden | Valid credentials but insufficient permissions |
| `404` | Not Found | Resource does not exist |
| `409` | Conflict | Duplicate resource creation |
| `422` | Unprocessable Entity | Valid JSON but semantically invalid (e.g., latitude out of range) |
| `429` | Too Many Requests | Rate limit exceeded |
| `500` | Internal Server Error | Unexpected server failure |
| `503` | Service Unavailable | System is temporarily overloaded or in maintenance |

### KOLEKTA-Specific Error Scenarios

| Scenario | Code | Error | Message (example) |
|----------|------|-------|-------------------|
| GPS position outside Luanda bounding box | `422` | `OutOfBounds` | "GPS position is outside the supported geographic area." |
| Unknown vehicle ID in GPS submission | `404` | `VehicleNotFound` | "Vehicle with the provided identifier is not registered." |
| Invalid API key | `401` | `InvalidApiKey` | "The provided API key is not valid or has been revoked." |
| Prediction not available | `404` | `PredictionNotFound` | "No prediction exists for the given identifier." |
| Batch too large | `400` | `BatchTooLarge` | "Batch exceeds the maximum of 1000 positions per request." |
| Subscription limit reached | `429` | `SubscriptionLimitReached` | "Maximum number of active subscriptions reached for this client." |

---

## API Groups

### GPS Ingestion (Operator → KOLEKTA)

These endpoints receive GPS data from operator devices or third-party tracking platforms.

---

#### `POST /api/v1/gps/positions`

Submit a single GPS position for a vehicle.

**Authentication**: API key (required)

**Request Body**:

```json
{
  "vehicle_id": "string",
  "latitude": -8.8390,
  "longitude": 13.2894,
  "timestamp": "2025-01-15T08:30:00Z",
  "speed_kmh": 22.5,
  "heading_degrees": 145.0,
  "metadata": {
    "odometer_km": 45230,
    "ignition": true
  }
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `vehicle_id` | string | Yes | Unique vehicle identifier (assigned during registration) |
| `latitude` | number | Yes | WGS 84 latitude |
| `longitude` | number | Yes | WGS 84 longitude |
| `timestamp` | string (ISO 8601) | Yes | Time of position fix (UTC) |
| `speed_kmh` | number | No | Speed in km/h |
| `heading_degrees` | number | No | Heading in degrees (0–360, north = 0) |
| `metadata` | object | No | Additional device-specific data |

**Response**: `201 Created`

```json
{
  "id": "pos_abc123",
  "received_at": "2025-01-15T08:30:02Z"
}
```

---

#### `POST /api/v1/gps/positions/batch`

Submit multiple GPS positions in a single request. Designed for devices that buffer positions and transmit periodically.

**Authentication**: API key (required)

**Request Body**:

```json
{
  "positions": [
    {
      "vehicle_id": "string",
      "latitude": -8.8390,
      "longitude": 13.2894,
      "timestamp": "2025-01-15T08:30:00Z",
      "speed_kmh": 22.5,
      "heading_degrees": 145.0
    }
  ]
}
```

| Constraint | Value |
|------------|-------|
| Maximum positions per batch | 1000 |
| Positions may span multiple vehicles | Yes |
| Duplicate timestamps for same vehicle | Ignored (idempotent) |

**Response**: `201 Created`

```json
{
  "accepted": 47,
  "rejected": 3,
  "errors": [
    {
      "index": 12,
      "message": "Latitude out of range"
    }
  ]
}
```

---

### Vehicles (Public, Read-Only)

Public endpoints for retrieving vehicle information and current status.

---

#### `GET /api/v1/vehicles/active`

List all currently active vehicles with their most recent position.

**Authentication**: None

**Query Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `lat` | number | No | Center latitude for geographic filtering |
| `lng` | number | No | Center longitude for geographic filtering |
| `radius_meters` | number | No | Radius for geographic filtering (requires `lat`, `lng`) |
| `operator_id` | string | No | Filter by operator |
| `limit` | number | No | Max results (default: 50, max: 200) |
| `offset` | number | No | Pagination offset |

**Response**: `200 OK`

```json
{
  "vehicles": [
    {
      "id": "veh_001",
      "operator_id": "op_abc",
      "position": {
        "latitude": -8.8390,
        "longitude": 13.2894,
        "speed_kmh": 18.3,
        "heading_degrees": 220.0,
        "timestamp": "2025-01-15T08:30:00Z"
      },
      "status": "collecting",
      "last_update_seconds_ago": 15
    }
  ],
  "total": 12,
  "limit": 50,
  "offset": 0
}
```

---

#### `GET /api/v1/vehicles/:id`

Get details and current status for a specific vehicle.

**Authentication**: None

**Response**: `200 OK`

```json
{
  "id": "veh_001",
  "operator_id": "op_abc",
  "type": "compactor",
  "position": {
    "latitude": -8.8390,
    "longitude": 13.2894,
    "speed_kmh": 18.3,
    "heading_degrees": 220.0,
    "timestamp": "2025-01-15T08:30:00Z"
  },
  "status": "collecting",
  "current_trip_id": "trip_xyz",
  "last_update_seconds_ago": 15
}
```

---

#### `GET /api/v1/vehicles/:id/trajectory`

Get a vehicle's current (ongoing) trajectory as a GeoJSON LineString.

**Authentication**: None

**Query Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `since` | string (ISO 8601) | No | Return trajectory from this time (default: start of current trip) |
| `simplify` | boolean | No | Apply Douglas-Peucker simplification (default: `true`) |

**Response**: `200 OK`

```json
{
  "vehicle_id": "veh_001",
  "trip_id": "trip_xyz",
  "started_at": "2025-01-15T06:00:00Z",
  "trajectory": {
    "type": "Feature",
    "geometry": {
      "type": "LineString",
      "coordinates": [
        [13.2894, -8.8390],
        [13.2900, -8.8385]
      ]
    },
    "properties": {
      "point_count": 245,
      "distance_meters": 12340
    }
  }
}
```

---

### Predictions (Public, Read-Only)

Endpoints for retrieving route predictions and estimated passage times.

---

#### `GET /api/v1/predictions/nearby`

Get predicted collection vehicle passages near a given location.

**Authentication**: None

**Query Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `lat` | number | Yes | Latitude of the location |
| `lng` | number | Yes | Longitude of the location |
| `radius_meters` | number | No | Search radius (default: 500, max: 2000) |
| `time_window_hours` | number | No | Look-ahead window (default: 4, max: 24) |

**Response**: `200 OK`

```json
{
  "predictions": [
    {
      "id": "pred_001",
      "vehicle_id": "veh_001",
      "operator_id": "op_abc",
      "estimated_arrival": "2025-01-15T07:30:00Z",
      "estimated_arrival_window": {
        "earliest": "2025-01-15T07:15:00Z",
        "latest": "2025-01-15T07:45:00Z"
      },
      "confidence": "high",
      "distance_meters": 320,
      "route_template_id": "tmpl_abc",
      "source": "historical_pattern"
    }
  ],
  "location": {
    "latitude": -8.8390,
    "longitude": 13.2894
  },
  "generated_at": "2025-01-15T06:00:00Z"
}
```

---

#### `GET /api/v1/predictions/:id`

Get details for a specific prediction.

**Authentication**: None

**Response**: `200 OK`

```json
{
  "id": "pred_001",
  "vehicle_id": "veh_001",
  "operator_id": "op_abc",
  "estimated_arrival": "2025-01-15T07:30:00Z",
  "estimated_arrival_window": {
    "earliest": "2025-01-15T07:15:00Z",
    "latest": "2025-01-15T07:45:00Z"
  },
  "confidence": "high",
  "route_template_id": "tmpl_abc",
  "predicted_path": {
    "type": "Feature",
    "geometry": {
      "type": "LineString",
      "coordinates": [
        [13.2900, -8.8385],
        [13.2894, -8.8390]
      ]
    }
  },
  "status": "active",
  "last_updated": "2025-01-15T06:45:00Z"
}
```

---

### Notifications (Public)

Endpoints for citizens to subscribe to proximity-based push notifications.

---

#### `POST /api/v1/notifications/subscribe`

Subscribe to notifications when a collection vehicle approaches a given location.

**Authentication**: None

**Request Body**:

```json
{
  "latitude": -8.8390,
  "longitude": 13.2894,
  "radius_meters": 500,
  "push_subscription": {
    "endpoint": "https://fcm.googleapis.com/fcm/send/...",
    "keys": {
      "p256dh": "...",
      "auth": "..."
    }
  }
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `latitude` | number | Yes | Center of notification area |
| `longitude` | number | Yes | Center of notification area |
| `radius_meters` | number | Yes | Notification trigger radius (min: 100, max: 2000) |
| `push_subscription` | object | Yes | W3C Push API subscription object |

**Response**: `201 Created`

```json
{
  "id": "2f7d8b90-4a51-4b3b-9c04-5312f8798d10",
  "created_at": "2025-01-15T06:00:00Z",
  "expires_at": "2025-02-14T06:00:00Z"
}
```

> [!NOTE]
> The selected location, radius, and push subscription are stored together for up to 30 days under [PD-009](PRODUCT_DECISIONS.md#pd-009-server-side-citizen-locations-for-background-notifications). Unsubscribing deletes the notification zone immediately. Push delivery is best-effort and depends on the browser, device, network, and push service.

---

#### `DELETE /api/v1/notifications/:id`

Unsubscribe from notifications.

**Authentication**: None (the cryptographically random subscription ID is an unguessable deletion token; it must not be exposed in public listings or logs)

**Response**: `204 No Content`

---

### Operators (Future, Authenticated)

> [!WARNING]
> These endpoints are planned for future phases and are **not part of the MVP**. The authentication strategy for operator access is ⏳ PENDING.

---

#### `GET /api/v1/operators/:id/dashboard`

Operator fleet overview including active vehicles, today's trips, and key metrics.

#### `GET /api/v1/operators/:id/vehicles`

List all vehicles belonging to an operator with their current status.

#### `GET /api/v1/operators/:id/analytics`

Route analytics: coverage, trip counts, average durations, prediction accuracy.

---

### System (Internal / Admin)

---

#### `GET /api/v1/health`

Basic health check. Returns `200 OK` if the API process is running.

```json
{
  "status": "ok",
  "timestamp": "2025-01-15T06:00:00Z"
}
```

---

#### `GET /api/v1/health/ready`

Readiness check. Returns `200 OK` only when all dependencies (database, Redis, etc.) are reachable.

```json
{
  "status": "ready",
  "checks": {
    "database": "ok",
    "redis": "ok",
    "postgis": "ok"
  },
  "timestamp": "2025-01-15T06:00:00Z"
}
```

Returns `503 Service Unavailable` if any dependency is unreachable.

---

## WebSocket API

Real-time vehicle tracking is provided via WebSocket connections. This allows the frontend to display live vehicle positions without polling.

### Connection

```
ws://host/ws/v1/vehicles
```

> [!NOTE]
> In production, use `wss://` (WebSocket Secure) behind a reverse proxy.

### Client → Server Messages

#### `subscribe_area`

Subscribe to vehicle events within a geographic area.

```json
{
  "event": "subscribe_area",
  "data": {
    "lat": -8.8390,
    "lng": 13.2894,
    "radius_meters": 1000
  }
}
```

**Response**:

```json
{
  "event": "subscription_confirmed",
  "data": {
    "subscription_id": "ws_sub_001"
  }
}
```

#### `unsubscribe_area`

```json
{
  "event": "unsubscribe_area",
  "data": {
    "subscription_id": "ws_sub_001"
  }
}
```

---

### Server → Client Messages

#### `vehicle_position`

Emitted when a tracked vehicle sends a new GPS position within a subscribed area.

```json
{
  "event": "vehicle_position",
  "data": {
    "vehicle_id": "veh_001",
    "lat": -8.8390,
    "lng": 13.2894,
    "speed_kmh": 18.3,
    "heading_degrees": 220.0,
    "timestamp": "2025-01-15T08:30:00Z"
  }
}
```

#### `vehicle_entered`

Emitted when a vehicle enters a subscribed area.

```json
{
  "event": "vehicle_entered",
  "data": {
    "vehicle_id": "veh_001",
    "distance_meters": 980
  }
}
```

#### `vehicle_exited`

Emitted when a vehicle leaves a subscribed area.

```json
{
  "event": "vehicle_exited",
  "data": {
    "vehicle_id": "veh_001"
  }
}
```

#### `prediction_update`

Emitted when a prediction relevant to the subscribed area is updated.

```json
{
  "event": "prediction_update",
  "data": {
    "prediction_id": "pred_001",
    "estimated_arrival": "2025-01-15T07:30:00Z",
    "confidence": "high"
  }
}
```

#### `stop_detected`

Emitted when a vehicle stop is detected within a subscribed area.

```json
{
  "event": "stop_detected",
  "data": {
    "vehicle_id": "veh_001",
    "lat": -8.8392,
    "lng": 13.2890,
    "duration_seconds": 180,
    "classification": "likely_collection"
  }
}
```

---

## Rate Limiting

| Consumer | Limit | Notes |
|----------|-------|-------|
| GPS ingestion | Per API key, configurable per operator | ⏳ Default rate TBD; will depend on device reporting frequency |
| Public REST endpoints | Per IP | ⏳ Specific limits TBD; reasonable defaults to prevent abuse |
| WebSocket connections | Max connections per IP | ⏳ Specific limit TBD |

Rate-limited responses return `429 Too Many Requests` with a `Retry-After` header:

```
HTTP/1.1 429 Too Many Requests
Retry-After: 60
Content-Type: application/json

{
  "error": "TooManyRequests",
  "message": "Rate limit exceeded. Try again in 60 seconds.",
  "statusCode": 429
}
```

---

## Pagination

### Cursor-Based (Time-Series Data)

Used for GPS positions, trajectory history, and other time-ordered data.

```
GET /api/v1/vehicles/veh_001/trajectory?cursor=eyJ0cyI6IjIwMjUtMDEtMTUifQ&limit=100
```

Response includes a `next_cursor` field:

```json
{
  "data": [...],
  "next_cursor": "eyJ0cyI6IjIwMjUtMDEtMTYifQ",
  "has_more": true
}
```

### Offset-Based (Entity Lists)

Used for vehicles, operators, and other entity collections.

```
GET /api/v1/vehicles/active?limit=50&offset=100
```

Response includes pagination metadata:

```json
{
  "data": [...],
  "total": 234,
  "limit": 50,
  "offset": 100
}
```

---

## Cross-References

- [System Architecture](SYSTEM_ARCHITECTURE.md) — how the API layer fits into the overall system
- [Data Model](DATA_MODEL.md) — database schema backing these endpoints
- [GPS Integration](GPS_INTEGRATION.md) — details on GPS data ingestion
- [Route Prediction](ROUTE_PREDICTION.md) — how predictions are generated
