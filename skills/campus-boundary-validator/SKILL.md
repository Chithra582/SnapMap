---
name: campus-boundary-validator
description: Validates GPS coordinates against institutional campus GeoJSON perimeter polygons and checks for restricted zones.
---

# Campus Boundary Validator

## Overview
The `campus-boundary-validator` skill performs point-in-polygon ray-casting checks to ensure uploaded photos originate within authorized university campus boundaries, while detecting uploads originating from privacy-sensitive zones.

## Core Capabilities
- **Perimeter Geo-Fencing**: Evaluates GPS points against multi-polygon GeoJSON geometries defining campus physical boundaries.
- **Restricted Zone Detection**: Identifies coordinates falling inside residential dormitories, exam centers, or health service buildings.
- **Micro-Location Obfuscation**: Enforces privacy coordinate snapping to prevent pinpointing individual student residential rooms.
- **Boundary Violation Reporting**: Returns deterministic status codes (`IN_BOUNDS`, `OUT_OF_BOUNDS`, `RESTRICTED_ZONE`).

## Inputs
- `latitude`: Latitude of the captured media.
- `longitude`: Longitude of the captured media.
- `campus_id`: Identifier for the target college campus.

## Outputs
- `is_valid`: Boolean flag indicating if coordinate is approved for public map display.
- `zone_classification`: Classification tag (`public_quad`, `athletic_facility`, `academic_hall`, `restricted_dorm`, `out_of_bounds`).
- `snapped_coordinates`: Obfuscated centroid coordinates safe for public map rendering.
