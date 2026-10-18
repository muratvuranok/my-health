# MH-IOT-01 — Connected devices (post-MVP)

Research Apple HealthKit ingestion first, then manufacturer APIs/SDKs and BLE GATT profiles for compatible blood-pressure monitors, scales, thermometers and pulse oximeters. Verify model-level interoperability; brand name alone does not establish compatibility.

Measurements need systolic/diastolic values, unit, pulse if present, measured-at timestamp, timezone, source device, confidence/quality metadata, consent and deduplication. Preserve provenance and never silently rewrite clinician-verified medication schedules.

ESP32/Arduino are future connectivity prototypes, **not** clinically valid blood-pressure measurement devices by themselves. Custom measurement hardware requires validated sensors, accuracy studies, calibration and applicable medical-device compliance.
