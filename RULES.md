# Operational Rules & Directives — SnapMap Agent

This document defines the binding operational directives, safety policies, content boundaries, and privacy safeguards for **SnapMap Agent** (`snapmap-agent`, v1.0.0).

---

## 1. Geospatial & Boundary Directives

1. **Campus Geo-Fence Verification:**
   - All incoming photo upload coordinates $(lat, lon)$ must be checked against registered institutional campus polygon boundaries.
   - Photos captured beyond campus boundaries must be denied public map placement and flagged under code `WARN_OUT_OF_BOUNDS_LOCATION`.
2. **Spatial Coordinate Obfuscation & Clustering:**
   - Raw micro-location GPS coordinates must never be rendered as distinct pins on public map views.
   - Individual coordinates must be clustered into centroid bubbles ($R \ge 25\text{ meters}$) containing at least 2 distinct uploads to prevent single-student stalker vectors.
3. **EXIF Metadata Stripping:**
   - All uploaded image files must undergo automated EXIF sanitization (stripping camera serial numbers, device models, altitude, and raw timestamps) before persistence in Azure Blob Storage.

---

## 2. Content Moderation & Community Safety

1. **Prohibited Content Categories:**
   - Deterministically reject photos containing graphic violence, explicit adult content, illegal substance consumption, or unauthorized student identification cards (`ERR_SAFETY_POLICY_VIOLATION`).
2. **Dormitory & Private Space Safeguards:**
   - Flag uploads originating from sensitive designated campus zones (residential dorm rooms, health clinics, testing centers) requiring student confirmation before inclusion in public event clusters (`ERR_RESTRICTED_ZONE_DETECTED`).
3. **Automated PII Redaction:**
   - Inadvertently captured student ID badges, vehicle license plates, or exam papers must be flagged for automated visual blurring prior to publication.

---

## 3. Storage & Infrastructure Safeguards

1. **Azure Blob Ingestion Integrity:**
   - Images must be processed through compression pipelines ($\le 2\text{ MB}$ payload cap) and saved with unique UUID-based blob keys.
   - Blob URLs stored in MongoDB must be immutable and served over TLS 1.3 endpoints.
2. **Authentication Gatekeeping:**
   - The agent accepts uploads only from authenticated sessions verified through institutional Clerk JWT tokens (`@college.edu` email domain restriction).
3. **No Self-Modification:**
   - The agent is prohibited from autonomously mutating its core operating instructions, safety policies, or compliance constraints defined in `RULES.md` and `agent.yaml` (`ERR_SELF_MODIFICATION_PROHIBITED`).

---

## 4. Human Supervision & Fallback Protocols

1. **Community Flagging & Human Review:**
   - When a photo receives 3 or more community flags, the agent must instantly hide the photo from public map bubbles and queue it for student moderator review.
2. **Emergency Kill-Switch:**
   - Campus safety administrators possess instant authority to toggle a global or zone-specific kill-switch, hiding active map bubbles during campus security incidents.
3. **Structured Audit Logging:**
   - Record every clustering iteration, boundary rejection, moderation flag, and administrative deletion in structured JSON logs for institutional recordkeeping.
