---
name: geospatial-event-clusterer
description: Clusters geotagged campus photo coordinates into cohesive event bubbles using spatial distance and temporal proximity algorithms.
---

# Geospatial Event Clusterer

## Overview
The `geospatial-event-clusterer` skill groups incoming campus photo uploads into interactive map event bubbles. It calculates pairwise Haversine distances, identifies spatial density concentrations, and computes bounding radii and centroid locations.

## Core Capabilities
- **Haversine Distance Evaluation**: Determines geographical proximity between photo coordinates across campus with meter-level accuracy.
- **DBSCAN Density Clustering**: Groups photos into event clusters when at least $k \ge 3$ uploads fall within radius $R_{\text{eps}} \le 50\text{ meters}$.
- **Centroid Calculation**: Calculates the weighted center of mass for each event cluster to anchor map bubble markers.
- **Temporal Windowing**: Filters coordinates by temporal freshness ($T_{\text{window}} = 2\text{ to }6\text{ hours}$) to reflect active happenings.

## Inputs
- `coordinates_batch`: Array of objects containing `latitude`, `longitude`, `timestamp`, and `photo_id`.
- `clustering_radius_meters`: Maximum distance threshold for spatial grouping (default: 50m).
- `temporal_window_hours`: Sliding window in hours (default: 4 hours).

## Outputs
- `clusters`: Array of clustered event objects with `centroid`, `photo_ids`, `bounding_radius`, and `venue_name`.
- `unclustered_pins`: Isolated photo uploads displayed as single map points.
