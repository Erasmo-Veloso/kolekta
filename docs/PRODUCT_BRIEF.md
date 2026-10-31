# KOLEKTA — Product Brief

> **"Sabe quando. Vive melhor."** — *"Know when. Live better."*

---

## Overview

| Field | Detail |
|---|---|
| **Product Name** | KOLEKTA |
| **Domain** | Civic technology — urban waste collection tracking |
| **Origin** | Angola |
| **Target Territory** | Luanda (initial) |
| **Platform** | Web application / Progressive Web App (PWA) |
| **Operating Model** | Independent open-source initiative |
| **Current Status** | Planning and foundation phase |

## The Problem

Citizens of Luanda lack reliable information about when waste collection vehicles will service their neighbourhood. Collection schedules are inconsistent, unpublished, or frequently disrupted. Residents have no way to know:

- Whether a collection vehicle is active and heading toward their area.
- What time collection typically occurs on their street.
- Whether today's collection has already passed or is still expected.

This uncertainty forces residents to leave waste on the street for extended periods — creating public health hazards, environmental degradation, and quality-of-life impacts — or to miss collection windows entirely.

## The Solution

KOLEKTA is a GPS-based tracking and prediction platform for urban waste collection. It combines three capabilities:

1. **Real-time vehicle tracking** — Citizens see the live positions of collection vehicles on a map and receive alerts when a vehicle approaches their area.
2. **Historical trajectory analysis** — The system records vehicle routes over time, building a picture of actual collection patterns.
3. **Geospatial prediction** — By analysing historical patterns, the system estimates when a vehicle is likely to pass a given location, giving citizens actionable timing information.

## Target Users

| User Group | Needs |
|---|---|
| **Citizens of Luanda** | Know when to put waste out; avoid missed collections; reduce street-level waste accumulation. |
| **Waste collection operators** | Understand fleet coverage; identify route inefficiencies; improve service transparency. |

## Potential Operators

KOLEKTA is designed to work with any urban waste collection operator. In Luanda, **ELISAL** is the primary public waste collection entity, and other operators also serve different areas of the city. KOLEKTA does not currently have a partnership or formal agreement with any operator. The platform is being built as an independent initiative that could benefit operators who choose to participate.

> [!IMPORTANT]
> No partnerships, endorsements, or data-sharing agreements with any operator have been established. KOLEKTA is an independent open-source project.

## Core Principles

KOLEKTA is guided by eight foundational principles:

1. **Civic Purpose** — The project exists to serve citizens and improve urban life, not to generate profit. Every feature decision is evaluated against its civic value.

2. **Open Source** — All code, documentation, and design artefacts are open. Transparency is not optional — it is the foundation of public trust.

3. **Transparent Location Use** — The system explains when citizen locations are stored for background notifications. Storage is opt-in and time-limited; location data is not monetised or used for profiling, and is sent to a push provider only to deliver the requested alert.

4. **Accessible by Default** — The platform must work for people across a wide range of devices, network conditions, and abilities. Performance on low-end smartphones over slow connections is a first-class requirement.

5. **Honest Data** — The system shows what it knows, acknowledges what it doesn't, and never fabricates certainty. Predictions are clearly labelled as estimates with confidence indicators.

6. **Local Context** — KOLEKTA is built for Luanda first. Design decisions respect the local infrastructure reality: intermittent connectivity, device diversity, and Portuguese as the primary language.

7. **Operator Agnostic** — The architecture supports multiple collection operators without favouring any single entity. The system is a platform, not a tool for one company.

8. **Incremental Delivery** — The project ships small, working increments rather than waiting for a complete system. Each release must deliver standalone value.

## Tagline

**"Sabe quando. Vive melhor."** — Portuguese for **"Know when. Live better."**

The tagline captures the core value proposition: by giving citizens reliable timing information about waste collection, KOLEKTA enables a small but meaningful improvement in daily life.

## Current Status

KOLEKTA is in the **planning and foundation phase**. This includes:

- Defining product requirements and scope
- Establishing architecture and technology decisions
- Setting up the open-source project infrastructure
- Writing foundational documentation

No code has been deployed. No data is being collected. No users are being served.

---

*Cross-reference: [Product Requirements](PRODUCT_REQUIREMENTS.md) · [MVP Scope](MVP_SCOPE.md) · [Product Decisions](PRODUCT_DECISIONS.md)*
