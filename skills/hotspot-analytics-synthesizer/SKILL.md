---
name: hotspot-analytics-synthesizer
description: Computes real-time campus vitality indices, active participant densities, and trending event hotspots for map rendering.
---

# Hotspot Analytics Synthesizer

## Overview
The `hotspot-analytics-synthesizer` skill analyzes aggregated campus engagement metrics to identify trending campus gatherings. It computes vitality indices, evaluates contributor diversity, and manages real-time marker broadcasting to client applications.

## Core Capabilities
- **Vitality Index Calculation**: Evaluates photo volume, contributor diversity, and temporal recency to score active event bubbles.
- **Hotspot Halo Generation**: Dynamically styles high-vitality clusters ($V_{\text{hotspot}} \ge 0.70$) with distinctive pulsating visual indicators.
- **Anti-Spam Verification**: Prevents single-user spam from artificially inflating cluster vitality scores.
- **Event Lifecycle Tracking**: Tracks rising, peak, and receding phases of campus events, automatically fading inactive bubbles.

## Inputs
- `cluster_id`: Unique identifier for the event cluster.
- `contributor_ids`: Set of distinct student contributor IDs.
- `photo_timestamps`: Array of upload timestamps.

## Outputs
- `vitality_score`: Normalized numerical score ($0.0 - 1.0$).
- `is_hotspot`: Boolean flag triggering prominent map styling.
- `event_phase`: Current lifecycle phase (`forming`, `peak`, `receding`, `concluded`).
