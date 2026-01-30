# FireExposure Platform Blueprint

A consolidated and expanded platform blueprint, including the key data we need to collect and save.

## A. Core Functional Modules

### 1. Location and Time Record Module

**Goal:** Build a continuous time-location timeline for each user.

**Key Functions:**
- Phone-based location capture using GPS with timestamp and accuracy
- Configurable sampling policy to balance battery and data quality
- Duty modes: always-on, on-shift, on-incident, and manual start/stop
- Geofence triggers for fire stations and incident scenes when possible
- Offline buffering and delayed upload when network is unavailable
- Timeline reconstruction that converts raw points into segments and stays

**Data Fields to Store:**
- User ID
- Latitude/longitude
- Accuracy radius
- Timestamp
- Provider type (GPS now, future providers later such as sensors)
- Device state flags (e.g., permission denied, GPS off)

### 2. Fire Event Linkage Module

**Goal:** Connect the location timeline to fire-related activities.

**Key Functions:**
- Create and manage a fire event record with start and end time
- Link a user's timeline segments (from Module 1) to that event window
- Support multiple event types: structural fire, wildland fire, training exercise, station activity, overhaul, rehab
- Allow manual correction when GPS is weak (e.g., indoor work)

**Event Fields to Store:**
- Event type
- Event location and approximate boundary
- Times
- Incident identifiers if the department uses them (use same technical terms of fire station)

### 3. Personal Participation and Exposure Context Module

**Goal:** Record what the firefighter did and what conditions they faced.

**Key Functions:**
- Participation roles: inside, outside, vehicle, rehab, command, etc.
- Task categories: attack, search, ventilation, overhaul, salvage, staging, etc.
- PPE and SCBA use per individual, not only per incident
- Exposure-relevant notes: visible smoke, heavy smoke, soot contact, heat intensity, water exposure, decon actions, etc.
- Fast input methods:
  - Guided question-and-answer voice workflow that converts speech to structured fields
  - Document upload and scan for incident reports and training forms, then field extraction
  - Image uploading function (e.g., images of fires, smoke, etc.)
  - Manual quick toggles for the most common fields

**Recommended Design Choice:**  
Use a short scripted interview after the event. Keep questions fixed and auditable. This improves data consistency.

### 4. PPE Lifecycle Module

**Goal:** Connect exposures to what gear was used and its condition.

**Key Functions:**
- PPE inventory per user: turnout gear, gloves, hood, boots, helmet, SCBA components if desired
- Lifecycle tracking:
  - Manufacture year and model
  - In-service date
  - Use frequency proxy (e.g., hours on incident)
  - Cleaning and decontamination history with method and date
  - Damage and repair log with photos
  - etc.
- Link each event to PPE used for that event
- Reminders for inspection and cleaning based on policy rules

### 5. Passive Sampling and Lab Results Module

**Goal:** Record chemical measurements and link them to exposure timelines.

**Key Functions:**
- Sampler assignment tracking:
  - Sampler ID
  - Connect with specific event
  - Deployment and collection times
- Lab results ingest:
  - CSV upload
  - Validation against expected fields
  - Unit normalization and metadata capture
- Link lab results to event windows and to the timeline segments (locations)
- Provide basic exposure summaries (e.g., cumulative load by time period and by event type)

**Important Caution:**  
Do not present medical risk prediction as a platform feature in the first version. Present research-grade summaries and export.

## B. Support Modules That Make the System Usable

### 6. Event Enrichment and Evidence Attachment Module

**Goal:** Attach supporting materials to improve context and later analysis.

**Key Functions:**
- Store photos, videos, and documents uploaded by the user
- Save external links as references, with minimal metadata (e.g., source and timestamp)
- Keep this as a manual link collection feature in the first version. Avoid automatic social media scraping.

### 7. Reporting and Dashboards Module

**Goal:** Turn raw logs into interpretable outputs.

**Key Functions:**
- Individual timeline view with segments, tasks, and PPE
- Event summary report:
  - Time on scene
  - Time in each role and zone
  - SCBA usage duration
  - PPE used
- Compliance dashboard:
  - Missing segments
  - Capture gaps
  - Late reports
- Research export dashboard:
  - Select date range
  - De-identify data
  - Export CSV/JSON

### 8. Integration and Interoperability Module

**Goal:** Work alongside existing systems such as First Due.

**Key Functions:**
- Standardized export schema for incidents, participation, and sampler records
- Import stubs to ingest incident IDs or basic report summaries if the department can provide files
- Configurable field mapping so you can adapt to different departments

## C. Cross-Cutting Platform Blocks

### 9. User Roles and Permissions

Define roles early:
- Firefighter
- Officer or safety lead
- Researcher/analyst
- Admin

Each role gets separate access rules. Use least privilege by default.

### 10. Privacy, Consent, and Governance

This is critical because you propose 24-hour, 7-day tracking.

**Key Functions:**
- Consent workflow and clear data use statements
- Tracking modes that allow duty-only logging
- Pause controls and visibility of what is being recorded
- De-identification support for research exports
- Data retention policy and deletion workflow
- Audit logs for who accessed or edited data

### 11. Data Quality and Validation

**Key Functions:**
- Automatic checks for missing time, impossible jumps, and long GPS gaps
- Confidence scores for location segments
- Manual review and correction tools for officers
- Versioned schema to support future fields without breaking old records

### 12. Reliability and Operational Requirements

**Key Functions:**
- Offline-first operation with safe sync
- Battery-aware sampling policy
- Error handling for permissions and GPS disabled states
- Secure storage and encryption in transit

### 13. Future Extension Placeholders

These keep the design flexible without expanding the current scope:
- Additional location providers (e.g., manual zones, QR, NFC, indoor beacons, CAD integration)
- Glove fit module integration stub that links to your existing hand sizing app
- LLM assistant as a controlled form-filling helper, not an open-ended advisor, grounded in your defined question set

