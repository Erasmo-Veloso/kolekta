# AGENTS.md — Instructions for AI Coding Agents

This file provides context and guidance for AI coding agents (e.g., GitHub Copilot, Cursor, Claude, Gemini, and similar tools) that may work on the KOLEKTA codebase.

## Project Overview

**KOLEKTA** is an open-source civic technology platform for urban waste collection tracking in **Luanda, Angola**. The system aims to help citizens know when waste collection vehicles may pass through their neighbourhoods by:

1. Ingesting GPS position data from collection vehicles.
2. Storing and analysing historical vehicle trajectories.
3. Identifying recurring route patterns using geospatial analysis.
4. Generating estimated passage time predictions for specific areas.
5. Delivering real-time tracking and notifications to citizens via a web application.

**Tagline:** "Sabe quando. Vive melhor." — *Know when. Live better.*

## Current Project Status

> [!IMPORTANT]
> **Implementation has not started yet.** The project is in the planning and documentation phase. Before proposing any code changes, read the documentation in `docs/` thoroughly to understand requirements, architecture decisions, and constraints.

## Repository Structure

```
repo/
├── assets/
│   └── branding/           # Logo files and brand assets
├── docs/
│   ├── PRODUCT_BRIEF.md        # Vision, goals, and context
│   ├── PRODUCT_REQUIREMENTS.md # Functional & non-functional requirements
│   ├── MVP_SCOPE.md            # MVP boundaries and priorities
│   ├── PRODUCT_DECISIONS.md    # Decisions log with rationale
│   ├── SYSTEM_ARCHITECTURE.md  # Technical architecture design
│   ├── DATA_MODEL.md           # Database schema and entities
│   ├── GPS_INTEGRATION.md      # GPS data ingestion strategy
│   ├── ROUTE_PREDICTION.md     # Prediction algorithms
│   ├── API_SPECIFICATION.md    # API contracts
│   ├── ROADMAP.md              # Development phases
│   ├── BRAND_GUIDELINES.md     # Visual identity
│   └── OPERATOR_DATA_AGREEMENT_TEMPLATE.md # Lean operator data sharing agreement
├── LICENSE                     # GNU Affero General Public License v3.0 (AGPL-3.0)
├── README.md
├── AGENTS.md                   # This file
├── CONTRIBUTING.md             # Contribution guidelines
├── CODE_OF_CONDUCT.md          # Community standards
└── SECURITY.md                 # Security policy
```

> [!NOTE]
> Application source code directories (e.g., `frontend/`, `backend/`, `services/`) do not exist yet and will be created as development begins.

## Key Technology Decisions

The proposed technology stack is documented in the [README](README.md) and evaluated in detail in [Product Decisions](docs/PRODUCT_DECISIONS.md):

| Layer | Proposed Technology |
|---|---|
| Frontend | React / Next.js (PWA) |
| Backend | NestJS (TypeScript) |
| Database | PostgreSQL + PostGIS |
| Cache | Redis |
| Geospatial Engine | Python |
| Maps | MapLibre + OpenStreetMap |
| Real-time | WebSockets |
| Notifications | Web Push / FCM |
| Infrastructure | Docker |

These choices are **proposed, not final**, and are subject to community discussion.

## Essential Constraints

When working on KOLEKTA, adhere to these constraints strictly:

### 1. No Premature Complex ML

Do **not** introduce heavy machine learning frameworks (TensorFlow, PyTorch, etc.) for route prediction. The initial approach should use statistical pattern recognition — time-window analysis, frequency counting, and basic geospatial heuristics. Complexity should only increase when simple methods are proven insufficient. See [Route Prediction](docs/ROUTE_PREDICTION.md).

### 2. Predictions Are Estimates, Not Guarantees

All passage time predictions must be communicated as **estimates** with confidence levels. Never present a prediction as a guaranteed arrival time. UI copy, API responses, and documentation must reflect this. See [Product Requirements](docs/PRODUCT_REQUIREMENTS.md).

### 3. Citizen Location and Privacy

- GPS data pertains to **vehicles**, not individual citizens.
- Citizen location stays client-side unless the person explicitly enables background notifications. Opted-in coordinates and the linked push subscription are stored for up to 30 days and deleted on unsubscribe or expiry, as defined in [PD-009](docs/PRODUCT_DECISIONS.md#pd-009-server-side-citizen-locations-for-background-notifications).
- Treat notification zones as sensitive even though the MVP does not require names, emails, phone numbers, or accounts. Do not expose precise locations through public APIs or logs.
- Minimise data collection — only collect what is necessary.
- Users must be able to use the platform without creating an account for basic features.
- All location data processing should be transparent and documented.
- See [Security Policy](SECURITY.md) for additional guidance.

### 4. Multi-Operator Architecture

The system must be designed from the outset to support **multiple waste collection operators**, not just a single company. Data models, APIs, and the UI must accommodate this. Do not hard-code operator-specific logic. See [Data Model](docs/DATA_MODEL.md) and [System Architecture](docs/SYSTEM_ARCHITECTURE.md).

### 5. Luanda-Specific Context

- The primary user base is in Luanda, Angola.
- Connectivity may be intermittent or low-bandwidth — design for resilience.
- The UI should support Portuguese (primary) and potentially English.
- Geographic data and examples should reference Luanda's municipalities and neighbourhoods.

## Coding Standards

⏳ **[PENDING]** Formal coding standards have not yet been defined. When they are established, they will be documented here and in `CONTRIBUTING.md`. In the meantime, follow these general principles:

- Write clear, self-documenting code with meaningful names.
- Add JSDoc/TSDoc comments for public APIs and complex logic.
- Use TypeScript strict mode for all TypeScript code.
- Follow the existing file and directory naming conventions once established.
- Write unit tests for business logic and integration tests for API endpoints.
- Use Conventional Commits for commit messages (see [CONTRIBUTING.md](CONTRIBUTING.md)).

## Where to Find Key Information

| Topic | Document |
|---|---|
| What is KOLEKTA and why? | [Product Brief](docs/PRODUCT_BRIEF.md) |
| What must it do? | [Product Requirements](docs/PRODUCT_REQUIREMENTS.md) |
| What is the MVP? | [MVP Scope](docs/MVP_SCOPE.md) |
| What has been decided? | [Product Decisions](docs/PRODUCT_DECISIONS.md) |
| How is it built? | [System Architecture](docs/SYSTEM_ARCHITECTURE.md) |
| What does the data look like? | [Data Model](docs/DATA_MODEL.md) |
| How does GPS data get in? | [GPS Integration](docs/GPS_INTEGRATION.md) |
| How do predictions work? | [Route Prediction](docs/ROUTE_PREDICTION.md) |
| What are the API contracts? | [API Specification](docs/API_SPECIFICATION.md) |
| What is the timeline? | [Roadmap](docs/ROADMAP.md) |
| How should it look? | [Brand Guidelines](docs/BRAND_GUIDELINES.md) |

## Before You Start

1. **Read `docs/` thoroughly.** Every document in the `docs/` directory provides essential context. Do not propose changes that contradict documented decisions without explicitly flagging the conflict.
2. **Check `PRODUCT_DECISIONS.md`** for pending decisions marked with ⏳. Do not assume a resolution for pending items — flag them or ask.
3. **Respect the MVP scope.** Features outside the MVP boundary should not be implemented until the MVP is complete, unless explicitly discussed and agreed upon.
4. **Follow the contribution workflow** described in [CONTRIBUTING.md](CONTRIBUTING.md).
