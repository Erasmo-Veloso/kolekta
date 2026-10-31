# KOLEKTA — Product Requirements

This document defines the full set of product requirements for KOLEKTA. Requirements are organised into **functional** (citizen-facing, operator-facing, and system) and **non-functional** categories. Each requirement has a unique identifier for traceability.

> [!NOTE]
> Not all requirements are in scope for the MVP. See [MVP Scope](MVP_SCOPE.md) for the initial delivery boundary.

---

## 1. Functional Requirements — Citizen-Facing

These requirements define what citizens can do through the KOLEKTA web application.

### FR-C01: View Map with Active Collection Vehicles

| Field | Detail |
|---|---|
| **ID** | FR-C01 |
| **Priority** | Must Have (MVP) |
| **Description** | Citizens can view an interactive map showing the real-time positions of active waste collection vehicles. |

**Acceptance Criteria:**
- The map displays the citizen's area of Luanda using OpenStreetMap tile data.
- Active collection vehicles are shown as distinct icons on the map.
- Vehicle positions update in near-real-time as new GPS data is received.
- The map supports pan, zoom, and geolocation controls.
- The map loads and renders correctly on mobile devices with limited screen space.

---

### FR-C02: Set or Share Location (GPS or Manual Pin)

| Field | Detail |
|---|---|
| **ID** | FR-C02 |
| **Priority** | Must Have (MVP) |
| **Description** | Citizens can set their location on the map to receive relevant proximity information and notifications. |

**Acceptance Criteria:**
- The citizen can use their device's GPS to set their current location automatically.
- The citizen can manually place a pin on the map to indicate their location.
- The selected location is used as the reference point for proximity calculations and notifications.
- The location can be updated at any time.
- Coordinates remain on the client unless the citizen explicitly enables background notifications. On opt-in, the selected location, radius, and push subscription are stored together in a notification zone for up to 30 days ([PD-009](PRODUCT_DECISIONS.md#pd-009-server-side-citizen-locations-for-background-notifications)).

---

### FR-C03: See Vehicles That May Pass Nearby

| Field | Detail |
|---|---|
| **ID** | FR-C03 |
| **Priority** | Must Have (MVP) |
| **Description** | Based on the citizen's set location, the system highlights vehicles that are active within a relevant radius. |

**Acceptance Criteria:**
- Vehicles within the citizen's spatial radius are highlighted on the map.
- The relevance radius is configurable on the client.
- If no vehicles are active nearby, the interface communicates this clearly.

---

### FR-C04: Receive Proximity Notification When Vehicle Approaches

| Field | Detail |
|---|---|
| **ID** | FR-C04 |
| **Priority** | Must Have (MVP — In-Session and Opt-In Background Push) |
| **Description** | Citizens receive an alert when an active collection vehicle enters a configurable proximity radius around their location. |

**Acceptance Criteria:**
- In-session alerts can be calculated in the browser without storing a location on the server.
- If the citizen enables background push, the server stores the selected coordinates, radius, and push subscription for up to 30 days and evaluates vehicle positions against that zone.
- The alert displays the vehicle identifier, approximate distance, and heading.
- The citizen can disable background notifications at any time; the associated notification zone and push subscription are then deleted.
- Background delivery is best-effort and depends on browser, device, network, and push-service availability.

---

### FR-C05: Receive Notification When Collection Stop Is Detected Nearby

| Field | Detail |
|---|---|
| **ID** | FR-C05 |
| **Priority** | Should Have (post-MVP) |
| **Description** | Citizens receive a notification when the system detects that a collection vehicle has made a probable collection stop (low speed or stationary period) near their location. |

**Acceptance Criteria:**
- The system identifies probable collection stops by analysing GPS speed, heading changes, and dwell time.
- A notification is sent to citizens whose set location is within the defined radius of the detected stop.
- The notification clearly indicates that this is a detected stop, not a confirmed collection event.
- False-positive stops (e.g., traffic, refuelling) should be minimised through heuristic tuning over time.

---

### FR-C06: View Estimated Passage Times Based on Historical Patterns

| Field | Detail |
|---|---|
| **ID** | FR-C06 |
| **Priority** | Should Have (Phase 2 — Route Intelligence) |
| **Description** | Citizens can view estimated times when a collection vehicle is likely to pass near their location, based on historical route patterns. |

**Acceptance Criteria:**
- Estimates are derived from recurring patterns in stored trajectory data (e.g., day-of-week, time-of-day).
- Estimates are clearly labelled as predictions, not guarantees.
- A confidence indicator reflects the reliability of the estimate (e.g., "usually passes between 08:00–09:30 on Tuesdays").
- If insufficient historical data exists, the system states this rather than guessing.

---

### FR-C07: View Vehicle Status

| Field | Detail |
|---|---|
| **ID** | FR-C07 |
| **Priority** | Should Have (MVP — basic version) |
| **Description** | Citizens can see the operational status of collection vehicles on the map. |

**Acceptance Criteria:**
- Each vehicle displays a status indicator: **Active** (currently on a collection route), **Returning** (heading back to depot/base), or **Idle** (stationary, not on route).
- Status is inferred from GPS data patterns (speed, location relative to known routes, time of day).
- The status updates as the vehicle's behaviour changes.

---

## 2. Functional Requirements — Operator-Facing (Future)

These requirements define capabilities for waste collection operators. They are **not in MVP scope** and are documented here for planning purposes.

> [!NOTE]
> Operator-facing features depend on an operator choosing to engage with the KOLEKTA platform. No operator participation has been confirmed.

### FR-O01: Register Vehicles and Assign to Operator

| Field | Detail |
|---|---|
| **ID** | FR-O01 |
| **Priority** | Future |
| **Description** | An operator can register their collection vehicles in the system, providing vehicle identifiers and associating them with the operator's organisation. |

**Acceptance Criteria:**
- Each vehicle has a unique system identifier.
- Vehicles are associated with exactly one operator.
- Vehicle metadata includes at minimum: identifier, operator, and vehicle type.
- Registered vehicles can be activated or deactivated.

---

### FR-O02: View Fleet Status Dashboard

| Field | Detail |
|---|---|
| **ID** | FR-O02 |
| **Priority** | Future |
| **Description** | Operators can view a dashboard showing the real-time status of their fleet: which vehicles are active, their positions, and basic operational metrics. |

**Acceptance Criteria:**
- The dashboard shows all vehicles belonging to the operator.
- Each vehicle displays its current status (active, returning, idle) and last known position.
- Basic metrics are displayed: number of active vehicles, total distance covered (current day), and estimated area coverage.
- The dashboard updates in near-real-time.

---

### FR-O03: View Historical Route Analytics

| Field | Detail |
|---|---|
| **ID** | FR-O03 |
| **Priority** | Future |
| **Description** | Operators can view historical data about their fleet's routes, including coverage maps, frequency of service per area, and route consistency. |

**Acceptance Criteria:**
- Historical trajectory data can be viewed on a map, filtered by date range and vehicle.
- Service frequency is visualised per area (e.g., heatmap or grid overlay).
- Route consistency metrics highlight areas with irregular service.

---

## 3. Functional Requirements — System

These requirements define backend and infrastructure capabilities that support the user-facing features.

### FR-S01: Ingest GPS Positions from Collection Vehicles

| Field | Detail |
|---|---|
| **ID** | FR-S01 |
| **Priority** | Must Have (MVP) |
| **Description** | The system accepts GPS position reports from collection vehicles via a defined API endpoint. |

**Acceptance Criteria:**
- The ingestion endpoint accepts position data including: vehicle identifier, latitude, longitude, timestamp, and optionally speed and heading.
- The endpoint validates incoming data for required fields, coordinate range, and timestamp plausibility.
- Invalid data points are rejected with appropriate error responses.
- Successfully ingested positions are available for real-time display within the defined latency target.
- The endpoint supports authentication to prevent unauthorised data submission. ⏳ *Authentication mechanism [PENDING]*

---

### FR-S02: Store and Index Historical Trajectories

| Field | Detail |
|---|---|
| **ID** | FR-S02 |
| **Priority** | Must Have (MVP) |
| **Description** | The system persistently stores GPS trajectories and indexes them for efficient spatial and temporal queries. |

**Acceptance Criteria:**
- Trajectories are stored in PostgreSQL with PostGIS spatial indexing.
- Data can be queried efficiently by vehicle, time range, and geographic bounding box.
- Storage schema supports trajectory reconstruction (ordered sequence of timestamped points).
- A data retention policy is defined and enforced. ⏳ *Retention period [PENDING]*

---

### FR-S03: Identify Recurring Route Patterns Automatically

| Field | Detail |
|---|---|
| **ID** | FR-S03 |
| **Priority** | Must Have (MVP — basic version) |
| **Description** | The system analyses stored trajectories to identify recurring route patterns — routes that are followed repeatedly on specific days or times. |

**Acceptance Criteria:**
- The system detects routes that recur on consistent days of the week.
- The system identifies typical time windows for each recurring route.
- Pattern detection runs as a batch process (not real-time) on a configurable schedule.
- Detected patterns are stored and available for the prediction engine.

---

### FR-S04: Generate Passage Predictions Based on Patterns

| Field | Detail |
|---|---|
| **ID** | FR-S04 |
| **Priority** | Must Have (MVP — basic version) |
| **Description** | The system generates predictions for when a collection vehicle is likely to pass a given geographic point, based on identified route patterns. |

**Acceptance Criteria:**
- Predictions include an estimated time window (not a single point in time).
- Predictions include a confidence level based on the consistency of the underlying pattern.
- Predictions are generated for the current day and near future (e.g., next 7 days).
- Invalidating a prediction when live movement diverges is [FR-S07](#fr-s07-invalidate-predictions-when-actual-movement-diverges), which is post-MVP. It is not an acceptance test for the basic version of FR-S04.

---

### FR-S05: Compare Live GPS with Predicted Routes

| Field | Detail |
|---|---|
| **ID** | FR-S05 |
| **Priority** | Should Have (post-MVP) |
| **Description** | The system compares incoming live GPS positions against predicted routes to assess whether a vehicle is following its expected pattern. |

**Acceptance Criteria:**
- Live positions are compared against the day's predicted route geometry.
- A deviation threshold triggers a route divergence event.
- Divergence events are logged and can trigger prediction invalidation (FR-S07).

---

### FR-S06: Detect Possible Collection Stops

| Field | Detail |
|---|---|
| **ID** | FR-S06 |
| **Priority** | Should Have (post-MVP) |
| **Description** | The system analyses GPS data to detect when a vehicle has made a probable collection stop — a location where the vehicle slowed, paused, or stopped in a manner consistent with waste collection activity. |

**Acceptance Criteria:**
- Stop detection uses speed, dwell time, and location context as heuristics.
- Detected stops are stored with location, time, and duration.
- The system distinguishes (where possible) collection stops from non-collection stops (e.g., traffic, fuel, depot).
- Detected stops feed into pattern analysis for future prediction improvement.

---

### FR-S07: Invalidate Predictions When Actual Movement Diverges

| Field | Detail |
|---|---|
| **ID** | FR-S07 |
| **Priority** | Should Have (post-MVP) |
| **Description** | When a vehicle's actual movement significantly deviates from predicted routes, the system marks affected predictions as invalid and communicates this to users. |

**Acceptance Criteria:**
- Predictions for areas the vehicle has not yet reached are marked as uncertain or cancelled.
- Citizens who were expecting a vehicle based on a now-invalidated prediction are notified (if they have opted in to notifications).
- The invalidation logic has configurable sensitivity thresholds.

---

### FR-S08: Support Multiple Operators

| Field | Detail |
|---|---|
| **ID** | FR-S08 |
| **Priority** | Must Have (post-MVP) |
| **Description** | The system supports multiple waste collection operators, with vehicles, routes, and data segregated by operator. |

**Acceptance Criteria:**
- Each operator has a distinct identity within the system.
- Vehicles are associated with exactly one operator.
- Operator data is logically separated.
- Citizens see vehicles from all active operators on the map (unified citizen experience).
- Operators see only their own data in operator-facing features.

---

## 4. Non-Functional Requirements

### NFR-01: Performance

| Aspect | Target |
|---|---|
| GPS ingestion-to-display latency | < 5 seconds (⏳ *target under validation*) |
| Map initial load time | < 3 seconds on 3G connection |
| API response time (95th percentile) | < 500 ms |
| Concurrent WebSocket connections | Support at minimum 1,000 simultaneous users (⏳ *target under validation*) |

---

### NFR-02: Scalability

- The system must support a growing number of tracked vehicles without architectural changes.
- The system must support a growing number of concurrent citizen users.
- The database schema and query patterns must remain performant as trajectory data accumulates over months.
- Horizontal scaling of the backend should be possible via container orchestration.

---

### NFR-03: Privacy

> [!IMPORTANT]
> Privacy is a core principle of KOLEKTA, not just a compliance checkbox.

- **Citizen location:** Location data is used for proximity calculations. It remains client-side unless the citizen explicitly enables background notifications; opted-in coordinates and push subscription are stored for up to 30 days and deleted on unsubscribe. The interface must explain this before opt-in. See [PD-009](PRODUCT_DECISIONS.md#pd-009-server-side-citizen-locations-for-background-notifications).
- **Vehicle tracking:** Vehicle GPS data is operational data provided by operators. It is stored for pattern analysis. Operators must consent to their vehicle data being displayed publicly.
- **No personal profiles:** The MVP does not require user accounts. No names, emails, or phone numbers are collected.
- **Data minimisation:** The system collects only the data necessary for its stated functions.
- **Transparency:** The privacy approach is documented publicly and communicated to users within the application.

⏳ *A formal Privacy Policy document is [PENDING].*

---

### NFR-04: Reliability

- The system must degrade gracefully when GPS data from vehicles is unavailable or delayed.
- If real-time vehicle positions are unavailable, the system should display the last known position with a clear timestamp and staleness indicator.
- Predictions should remain visible even when live tracking is temporarily down.
- The system should recover automatically from transient failures (e.g., database connection drops, WebSocket disconnections).

---

### NFR-05: Accessibility

- The application targets **WCAG 2.1 Level AA** compliance.
- The map interface must provide alternative text-based information for users who cannot interact with the visual map.
- Colour choices must meet contrast ratio requirements.
- The interface must be navigable via keyboard.
- Touch targets must meet minimum size requirements for mobile use.

---

### NFR-06: Internationalisation (i18n)

- The primary interface language is **Portuguese** (as spoken in Angola).
- **English** is supported as a secondary language.
- The application architecture must support adding additional languages without code changes (string externalisation).
- Date, time, and number formatting must respect locale conventions.
- All user-visible strings must be externalised — no hardcoded text in UI components.

---

### NFR-07: Offline Support

- The application is delivered as a **Progressive Web App (PWA)** and can be installed on the user's home screen.
- When offline, the application displays the last known state: cached map tiles, last known vehicle positions (with clear staleness indicator), and any cached predictions.
- The application clearly indicates when it is operating in offline mode.
- When connectivity is restored, the application resynchronises automatically.

---

## Requirement Traceability

| Requirement | MVP (Phase 1) | Route Intelligence (Phase 2) | Future (Phase 3+) |
|---|---|---|---|
| FR-C01 | ✅ | | |
| FR-C02 | ✅ (Client-side, or stored on opt-in) | | |
| FR-C03 | ✅ | | |
| FR-C04 | ✅ (In-Session and Background Push) | | |
| FR-C05 | | ✅ | |
| FR-C06 | | ✅ | |
| FR-C07 | ✅ (basic) | ✅ (enhanced) | |
| FR-O01 | | | ✅ |
| FR-O02 | | | ✅ |
| FR-O03 | | | ✅ |
| FR-S01 | ✅ | | |
| FR-S02 | ✅ (raw ingestion) | ✅ (trajectory pipeline) | |
| FR-S03 | | ✅ | |
| FR-S04 | | ✅ | |
| FR-S05 | | ✅ | |
| FR-S06 | | ✅ | |
| FR-S07 | | ✅ | |
| FR-S08 | | | ✅ |

---

*Cross-reference: [MVP Scope](MVP_SCOPE.md) · [System Architecture](SYSTEM_ARCHITECTURE.md) · [Product Decisions](PRODUCT_DECISIONS.md)*
