Status: DRAFT
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# IOT-001 / MH-IOT-01 — Connected devices (post-MVP)

Research Apple HealthKit ingestion first, then manufacturer APIs/SDKs and BLE GATT profiles for compatible blood-pressure monitors, scales, thermometers and pulse oximeters. Verify model-level interoperability; brand name alone does not establish compatibility.

Measurements need systolic/diastolic values, unit, pulse if present, measured-at timestamp, timezone, source device, confidence/quality metadata, consent and deduplication. Preserve provenance and never silently rewrite clinician-verified medication schedules.

ESP32/Arduino are future connectivity prototypes, **not** clinically valid blood-pressure measurement devices by themselves. Custom measurement hardware requires validated sensors, accuracy studies, calibration and applicable medical-device compliance.

## Smartwatch product track (separate from medical-device BLE integration)
As of the owner's phased roadmap direction on 2026-10-19, Apple Watch/watchOS and Android Wear OS **companion applications** are planned for a post-MVP phase. Their first possible scope is daily medication overview, privacy-preserving reminders and user-confirmed taken/skipped actions synchronized safely with the phone. This is **not part of MH-PROT-001** and does not approve any watch implementation.

A watch companion app is **not the same integration** as a Bluetooth blood-pressure cuff, scale or custom ESP32 medical prototype. Health-sensor ingestion or standalone/disconnected watch use remains a later proposal requiring model/platform-specific feasibility and privacy evidence. See [ROADMAP.md](../../ROADMAP.md) for stages; avoid creating a separate execution stream before the phone foundation is usable.

## Workout and location provenance (phased; no implementation GO)
My Health will eventually **display detailed workout and walking records**, including a **route map only when usable GPS track samples are available** from an authorized phone/watch/platform source. Workout sessions and precise routes must remain separate permission/data categories; a health-record grant does not itself authorize raw GPS collection, cloud sharing or public route visibility.

Initial read-only consideration: HealthKit workouts and route-capable sources on iOS; Health Connect exercise sessions and supported route access paths on Android (exact platform availability, permission and user-consent behavior must be verified at implementation). Apple Watch and Wear OS data may arrive via supported platform health stores; a dedicated watch recording app is not required for the first phone-side activity history and is NOT approved by this note.

Candidate session fields for future design (not an approved API schema): source ID/device, profile_id, activity type, start/end and timezone, elapsed/moving duration, distance, optional steps/calories/heart-rate statistics, source/provenance, and optional track points with timestamps. Keep track data local-first and protected, allow user to hide/delete maps, and handle absent or incomplete routes without implying that a workout did not occur.

This is **not** a new BLE medical-device initiative, does not add anything to MH-PROT-001, and does not grant permission for background location collection, health-data upload, external map services or use of real health data. See [ROADMAP](../../ROADMAP.md) for sequencing.
