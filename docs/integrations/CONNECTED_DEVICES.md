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
