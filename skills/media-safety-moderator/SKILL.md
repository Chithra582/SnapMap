---
name: media-safety-moderator
description: Audits uploaded photo buffers for content policy violations, strips invasive EXIF metadata, and redacts inadvertent PII.
---

# Media Safety Moderator

## Overview
The `media-safety-moderator` skill acts as an automated content and privacy filter for campus photo uploads. It strips hardware device fingerprints, scans image buffers for harmful or abusive imagery, and applies visual blurring to inadvertent student PII.

## Core Capabilities
- **EXIF Metadata Stripping**: Removes hardware serial numbers, camera lens data, GPS altitude, and precise sub-second device timestamps.
- **Safety Policy Screening**: Detects nudity, violence, weapons, hate symbols, and illicit substance consumption.
- **Automated PII Blurring**: Flags student ID cards, driver licenses, vehicle license plates, and exam papers for bounding-box pixelation.
- **Moderator Escalation Queue**: Routes borderline or community-reported photos to student safety moderators with audit logging.

## Inputs
- `image_buffer_base64`: Base64-encoded image data or Azure Blob reference.
- `uploader_id`: Clerk user identifier.
- `report_count`: Number of community flags received.

## Outputs
- `moderation_verdict`: Decision status (`approved`, `blurred_pii`, `quarantined`, `rejected`).
- `confidence_score`: Confidence score of the visual safety classifier.
- `flagged_categories`: Array of triggered moderation categories, if any.
