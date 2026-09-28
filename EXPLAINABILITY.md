# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **SnapMap Agent** (`snapmap-agent`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** SnapMap Agent (`snapmap-agent`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Education / Hyperlocal Campus Social & Geospatial Event Mapping  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), FERPA, GDPR  

---

## How the Agent Decides

SnapMap Agent is an autonomous hyperlocal campus photo-sharing, geospatial event clustering, spatial density hotspot detection, and privacy-preserving media moderation agent designed for **SnapMap**. It coordinates real-time GPS coordinate validation, Azure Blob secure ingestion, dynamic map bubble clustering, and campus community safety.

### 1. Decision Architecture

The photo upload verification, geospatial clustering, content moderation, and real-time map rendering pipeline operates across a deterministic, five-stage architecture:

```
Student Capture (In-App Camera Photo + GPS Coordinates + Clerk Session Token)
    │
    ▼
[Stage 1: Authentication & Geo-Fence Validation]
    │  - Validates student authentication and institutional email domain (@college.edu) via Clerk JWT
    │  - Checks raw GPS coordinates against campus polygon boundary GeoJSON
    │  - Denies out-of-bounds uploads from public map layers; flags for private profile storage
    │  - Strips hardware EXIF metadata, camera serial numbers, and device fingerprints
    ▼
[Stage 2: Media Safety Screening & Privacy Filtering]
    │  - Evaluates image buffer against visual safety classification models
    │  - Scans for prohibited content: nudity, weapons, graphic violence, or illicit substances
    │  - Applies automated blurring to inadvertent PII (student ID cards, driver licenses, exam sheets)
    │  - Rejects policy-violating media deterministically with structured audit logging
    ▼
[Stage 3: Spatial Density Analysis & Event Clustering]
    │  - Queries active geospatial submissions within temporal window (T_window = 2 to 6 hours)
    │  - Calculates pairwise Haversine distances between photo coordinates across campus
    │  - Executes density-based spatial clustering (DBSCAN) with epsilon radius R_eps = 50m
    │  - Groups eligible uploads into dynamic event bubbles; assigns venue tags and centroid coordinates
    ▼
[Stage 4: Cloud Persistence & Geospatial Indexing]
    │  - Compresses approved image buffers and streams uploads to Azure Blob Storage
    │  - Persists photo metadata, blob URLs, and 2dsphere GeoJSON points in MongoDB Atlas
    │  - Updates active event records and computes real-time hotspot vitality indices
    ▼
[Stage 5: Dynamic Map Bubble Rendering & Real-Time Broadcasting]
    │  - Aggregates clustered photos into interactive map bubbles for client discovery
    │  - Generates cluster count indicators and thumbnail previews
    │  - Broadcasts updated marker coordinates to active student sessions via WebSockets / REST
    ▼
Interactive Campus Map Refreshed with Verified, Privacy-Protected Real-Time Event Bubbles
```

### 2. Scoring Methodology & Rubric Formulations

SnapMap Agent determines geospatial clustering, proximity, and hotspot vitality through two deterministic, mathematically rigorous scoring models:

1. **Pairwise Geospatial Haversine Distance ($D_H(p_1, p_2)$)**:
   $$D_H(p_1, p_2) = 2 R \cdot \arcsin\left(\sqrt{\sin^2\left(\frac{\Delta \phi}{2}\right) + \cos(\phi_1)\cos(\phi_2)\sin^2\left(\frac{\Delta \lambda}{2}\right)}\right)$$
   where:
   - $R = 6,371,000 \text{ meters}$ is the mean radius of Earth.
   - $\phi_1, \phi_2$ represent latitudes in radians, and $\lambda_1, \lambda_2$ represent longitudes in radians.
   - $\Delta \phi = \phi_2 - \phi_1$ and $\Delta \lambda = \lambda_2 - \lambda_1$.
   - Proximity rule: Photos $p_1$ and $p_2$ are grouped into the same cluster candidate set if $D_H(p_1, p_2) \le R_{\text{eps}}$, where $R_{\text{eps}} = 50 \text{ meters}$.

2. **Campus Hotspot Vitality Index ($V_{\text{hotspot}}$)**:
   $$V_{\text{hotspot}} = w_c \cdot \min\left(1.0, \; \frac{N_{\text{photos}}}{N_{\text{target}}}\right) + w_u \cdot \frac{U_{\text{distinct}}}{N_{\text{photos}}} + w_t \cdot \exp\left(-\lambda_t \cdot \Delta t_{\text{latest}}\right)$$
   where:
   - $N_{\text{photos}}$: Number of photos uploaded to the cluster in the current window ($w_c = 0.45$).
   - $U_{\text{distinct}}$: Count of unique verified student contributors, preventing single-user spam ($w_u = 0.35$).
   - $\Delta t_{\text{latest}}$: Time elapsed in minutes since the most recent photo upload ($w_t = 0.20, \lambda_t = 0.02$).
   - Hotspot threshold: Clusters with $V_{\text{hotspot}} \ge 0.70$ and $N_{\text{photos}} \ge 5$ receive prominent "Hotspot" visual halos on the campus map.

### 3. Thresholding & Refusal Decision Criteria

SnapMap Agent enforces strict deterministic refusal and safety boundaries:
- **Refusal on Out-of-Bounds Submissions**: Photos with GPS coordinates outside authorized campus boundary polygons are rejected from public map placement with code `ERR_OUT_OF_BOUNDS_LOCATION`.
- **Refusal on Safety Violations**: Media containing explicit adult material, violence, hate speech, or harassment is deterministically blocked under code `ERR_SAFETY_POLICY_VIOLATION`.
- **Refusal on Restricted Zone Captures**: Submissions geolocated inside residential dormitories, exam centers, or health service facilities require student privacy confirmation (`ERR_RESTRICTED_ZONE_DETECTED`).
- **Refusal on Unauthenticated Access**: Uploads missing valid Clerk institutional JWT verification are rejected at the gateway under code `ERR_UNAUTHENTICATED_ACCESS`.
- **Refusal of Self-Modification**: Attempts to alter operational rules in `RULES.md` or configuration in `agent.yaml` are blocked under code `ERR_SELF_MODIFICATION_PROHIBITED`.

### 4. Fallback Decision Mechanism

SnapMap Agent ensures continuous campus navigation through a multi-tier fallback architecture:
- **Model Fallback Cascade**: High-level image moderation and venue classification default to `gemini-2.0-flash` with automatic failover to `gpt-4o` and `claude-3-5-sonnet`.
- **Deterministic Grid-Binning Fallback**: If dynamic DBSCAN clustering encounters spatial engine latency ($> 200\text{ ms}$), the system gracefully degrades to fixed-cell geospatial S2 / Geohash spatial binning without service interruption.
- **Local Cache Map Markers**: If MongoDB Atlas geospatial queries experience temporary network partitions, the client renders cached event bubbles from local SQLite/AsyncStorage snapshots.
- **Multi-Region Azure Blob Redundancy**: If the primary Azure Blob storage container endpoint times out, upload buffers are streamed to secondary geographically redundant blob storage.

### 5. Human-in-the-Loop Governance

SnapMap Agent maintains democratic student community oversight and administrative safety controls:
- **Community Flagging Protocol**: Any campus photo receiving 3 or more student reports is automatically quarantined from the public map and routed to student safety moderators.
- **Instant Photo Retraction**: Students retain permanent control over their uploaded photos, with real-time ability to delete content with instant 0-byte blob purging.
- **Campus Emergency Kill-Switch**: Campus safety administrators possess instant authority to suspend public map visualization during campus security incidents.
- **Audit Logging**: Every clustering run, boundary refusal, moderation flag, and deletion is recorded in tamper-evident structured JSON logs for administrative review.

---

## The Data It Uses

SnapMap Agent operates under strict privacy and educational data governance standards.

### 1. Ingested Input Data

The agent processes only authorized student uploads and device coordinates:
- **Camera Image Streams**: In-app camera photo captures compressed to JPEG format ($\le 2\text{ MB}$).
- **Geospatial Coordinates**: Device latitude and longitude coordinates captured at the precise moment of photo capture.
- **Authentication Metadata**: Clerk JWT session claims, user ID, verified institutional email domain (`@college.edu`), and capture timestamps.

### 2. Configuration & Reference Data

- **Campus Polygon GeoJSON**: Authoritative geographic boundary polygons outlining university grounds, athletic facilities, and academic quads.
- **Restricted Zone Catalog**: Geospatial boundary definitions for sensitive campus sectors (dorms, health centers, counseling offices).
- **Venue Knowledge Graph**: Mapping of campus coordinates to human-readable building names and landmark landmarks.

### 3. Base Model & Inference Lineage

- **Deterministic Geospatial Linters**: Haversine formula solvers, point-in-polygon ray-casting algorithms, and DBSCAN spatial clustering executed via deterministic JavaScript / Python engines.
- **Foundation Vision Models**: High-capability vision models (`gemini-2.0-flash`, `gpt-4o`, `claude-3-5-sonnet`) utilized strictly for media safety filtering and OCR-based sensitive ID redaction.
- **Zero Training on Student Media**: Student photos, personal facial images, and campus location histories are never utilized to train public foundation models.

### 4. Data Privacy, Storage, and Retention

- **FERPA & GDPR Compliance**: Full compliance with FERPA and GDPR (Articles 5, 17, and 28). All campus social media records are classified as `internal` with AES-256 encryption at rest in Azure Blob Storage and TLS 1.3 in transit.
- **EXIF Stripping**: Ingested image files are stripped of camera serial numbers, device identifiers, and timestamp micro-data prior to storage.
- **Automated Ephemeral Retention**: Public event photos are governed by configurable lifecycle policies, automatically expiring and purging after 72 hours of event conclusion.

---

## Limitations

Understanding the operational boundaries and technical constraints of SnapMap Agent is essential for effective campus community operations.

### 1. Indoor GPS Multipath & Vertical Altitude Drift
- **Limitation**: Deep indoor academic halls or underground laboratories can cause GPS satellite multipath reflection, resulting in $\pm 20\text{ meter}$ horizontal location variance.
- **Mitigation**: The agent evaluates location accuracy radius reported by the mobile OS and applies spatial cluster snapping to known campus building footprints.

### 2. Cold-Start Sparse Photo Density in Remote Campus Sectors
- **Limitation**: Peripheral campus zones (botanical gardens, remote parking lots) with sparse uploads may not reach the minimum threshold for cluster formation.
- **Mitigation**: Individual uploads in sparse sectors are presented as subtle standalone discoverable pins rather than full event bubbles until density increases.

### 3. Visual Moderation Edge Cases & Contextual Humor
- **Limitation**: AI vision moderation models may occasionally misinterpret benign campus theater props or mock trial costumes as genuine safety violations.
- **Mitigation**: Flagged uploaders receive immediate notice with a one-click student moderator appeal pathway, ensuring human review within minutes.

### 4. Rapid Multi-Hotspot Migration & Split Events
- **Limitation**: Fast-moving student crowds (e.g., campus parade or homecoming walk) can cause clustering centroids to lag slightly behind the active front of the crowd.
- **Mitigation**: Dynamic temporal decay weighting ($\lambda_t$) prioritizes recent uploads within the last 15 minutes, allowing clusters to track moving gatherings.

### 5. Network Dead-Zones During Major Stadium Events
- **Limitation**: High cell-tower congestion during major campus football games can delay upload transmission to Azure Blob Storage.
- **Mitigation**: The mobile client implements an offline background upload queue with retry backoff, syncing photos automatically once connectivity stabilizes.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Scoring methodology & spatial formulas | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested photos, coordinates & sessions | Section 1 | Verified |
| - Configuration, campus geo-fence & schemas | Section 2 | Verified |
| - Base model lineage & deterministic clustering | Section 3 | Verified |
| - Data privacy, EXIF scrubbing & FERPA/GDPR | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Indoor GPS multipath & vertical altitude drift | Section 1 | Verified |
| - Cold-start sparse photo density in remote campus sectors | Section 2 | Verified |
| - Visual moderation edge cases & contextual humor | Section 3 | Verified |
| - Rapid multi-hotspot migration & split events | Section 4 | Verified |
| - Network dead-zones during major stadium events | Section 5 | Verified |
