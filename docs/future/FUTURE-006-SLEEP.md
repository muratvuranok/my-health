Status: BACKLOG — NOT SCHEDULED; NO IMPLEMENTATION GO
Date: 2026-10-19
Owner: My Health MASTER

# FUTURE-006 — Sleep tracking and sound analysis

**Archived concept, not implementation authority.** This is supporting context only; MH-PROT-001 is unchanged. Future specification requires explicit owner phase GO and evidence of user value.

## Sleep tracking and overnight analysis — phased direction (owner request 2026-10-19)
**Product direction:** My Health should eventually show understandable overnight sleep quality, stages and relevant trends, inspired by Pillow, AutoSleep, Sleep Cycle, Oura and Garmin Connect. This is **planning only**, not new scope for the parallel iOS/Android **MH-PROT-001 phone prototype**, nor approval to deploy a tracker, record audio or start an agent.

| Stage | First useful delivery | Explicitly deferred |
|---|---|---|
| 0 — Current phone prototype | Existing synthetic medication/profile experience remains unchanged | All sleep tracking implementation |
| S1 — After phone foundation | **Read-only sleep history and nightly detail**: imported permitted sleep sessions from Apple HealthKit / Android Health Connect, start/end, duration, time asleep vs time in bed where supplied, awake/light/deep/REM intervals where supplied, source/device, timezone and missing-data states; week/month views | Proprietary sleep classification or medical claims |
| S2 — Enriched night report | Optional nighttime heart-rate and supported HRV/respiration/SpO2/temperature trends, user-entered sleep notes, basic cross-night comparisons and clearly labeled **source-provided** sleep score if available | Invented scores, diagnosis, medication effectiveness claims |
| S3 — Optional active overnight features | Evaluate dedicated smartwatch tracking, smartphone smart alarm and **separately opt-in** microphone-based snoring/sleep-sound detection only after OS feasibility, battery/privacy and user research | Continuous microphone recording by default, apnea diagnosis, guaranteed alarm precision |

**Privacy and interpretation:** Sleep data and especially overnight bedroom audio are sensitive. Read permission must be purpose-specific, revocable and profile-isolated; recordings (if ever approved) default OFF and require a separate explicit user action and retention/deletion policy. No default cloud transfer, no background microphone capture added by accepting health data, no audio-derived sleep-apnea diagnosis. Sleep stages and vendor sleep scores are **estimates**, not medical sleep studies. Display supported source fields, not fabricated measurements.

**Competitor/reference scan (2026-10-19):**
- **Pillow** — Apple Watch sleep stages, quality, heart-rate overlay, trends; its nighttime sound recordings require a manually started iPhone session, unlike automatic watch sleep analysis. https://pillow.app/ ; https://intercom.help/pillow_app/en/articles/3733515-pillow-didn-t-record-any-sounds-during-sleep
- **AutoSleep** — automatic Apple Watch sleep history, stages, consistency and local/privacy-focused presentation. https://apps.apple.com/us/app/autosleep-watch-sleep-tracker/id1164801111
- **Sleep Cycle** — phone-first sound-based analysis and smart alarm. https://sleepcycle.com/the-app/smart-alarm
- **Oura** — stage hypnogram, sleep score and recovery context from multiple sensors. https://support.ouraring.com/hc/en-us/articles/11752397946003-Sleep-Stages
- **Garmin** — sleep coach and estimated personal sleep need based on recent sleep and activity. https://www.garmin.com/en-IE/garmin-technology/health-science/sleep-coach/

**Integration feasibility baseline (verify when work begins):** HealthKit supports in-bed/awake/core/deep/REM sleep samples (https://developer.apple.com/documentation/healthkit/hkcategoryvaluesleepanalysis). Health Connect supports SleepSessionRecord with sleep stages under specific READ_SLEEP permission (https://developer.android.com/health-and-fitness/health-connect/features/sleep-sessions). Neither API guarantees that every watch/provider supplies every sleep stage, heart-rate sample, vendor score or nighttime sound recording. This roadmap does **not** authorize partner API access or third-party proprietary score reconstruction.

**Delivery rule:** Do not create a separate sleep-data backend, nightly recording engine, algorithm testing program, detailed ADR or new watch workstream before an approved S1 task exists. First deliver a small useful read-only night's report from available, consented data; add active tracking only if evidence warrants it.

### S3 decision note — daily explicit opt-in for overnight sound capture (owner direction 2026-10-19)
**Planning only; S3 remains deferred.** The initial operating-system microphone permission is **not** consent to start recordings every night. The user chooses a reminder time, by default within **21:00–22:00**, and sees a clear prompt: **"Record sleep sounds tonight? Recording stops no later than 09:00."** Actions: **Start tonight**, **Skip tonight**, and access to settings. No response = no recording. Do not silently start capture from a scheduled reminder or persistent prior consent.

On explicit nightly approval, request any missing OS permission and begin capture **only through a platform-permitted user-initiated flow**; show a persistent, clear recording indicator and **Stop now** control. If the user stops, revokes permission or the recording session fails, stop immediately and report actual session status. The planned **09:00 local-time ceiling** is a safety goal, **not a platform guarantee**: enforce a bounded recording session with an independent elapsed-time cutoff while capture is running, and validate behavior under app termination, background restrictions, timezone changes, DST, device reboot and permission revocation. If deterministic stopping cannot be demonstrated, **do not ship recording**; disable/limit sessions until the platform supports a verified safe stop mechanism. No auto-restart next night.

Prefer **on-device event detection without retaining raw audio**, with raw snippets OFF unless separately explicitly enabled; silence, speech and intimate/private sounds can never be reliably distinguished, so never promise selective non-capture. Inform users that **other people in the room may be recorded**; require suitable notices/consent and observe applicable privacy laws. No recordings, speech content, sound embeddings or microphone streams sent to backend, logs, analytics or third parties by default. Provide obvious deletion, retention settings, per-night controls and a plain summary of what was actually captured. Record no more data than needed for the opted-in purpose.

**Acceptance before any future S3 GO:** nightly user action demonstrably required, skipped/unanswered prompts produce zero microphone access, stopped/revoked capture ceases, safe duration cap verified on target iOS/Android versions, understandable capture status and deletion, failure behavior documented with real-device evidence. This note adds no tasks to MH-PROT-001, no app code or microphone credentials, and no promise of a background recorder.

## Device integration notes
## Sleep sessions and nighttime signals (phased; not part of MH-PROT-001)
After the first usable native phone foundation, plan a **read-only overnight sleep report** from explicitly permitted HealthKit `sleepAnalysis` and Android Health Connect `SleepSessionRecord` data, including stage intervals and source/provenance when present. HRV, heart rate, respiratory rate, oxygen saturation and temperature must each be shown **only if actually available and separately authorized**, not inferred from the existence of a sleep session. Sleep score is vendor-specific and not a portable required field. Respect multiple sources, conflicting sessions, time zones, incomplete data and missing stages without pretending that a clinical sleep analysis occurred.

Reference designs: Pillow (sleep stages, nighttime HR and trends; audio requires manually started iPhone mode), AutoSleep (automatic Apple Watch/local data presentation), Sleep Cycle (optional microphone and alarm), Oura (stages and recovery), Garmin (sleep coach). See [ROADMAP](../../ROADMAP.md) for sequencing and source URLs.

**Audio and sensitive data boundary:** The app must not start a nighttime microphone recording merely because a user agreed to sleep data import. Any future snoring/sleep-sound feature is a separate opt-in product decision, with clear on-device handling, deletion and disclosure, and no diagnosis of sleep apnea. Watch-only sleep data display does not require building dedicated watch apps first. No live data, provider credentials, clinical inference or implementation authorization is granted by this section.

### S3 nighttime sound-capture interaction (future, no implementation GO)
Per owner direction (2026-10-19), user-configured evening reminder (default 21:00–22:00) asks **each night** whether to record. Initial microphone grant alone must never initiate automatic overnight capture. Silence or "Skip tonight" means no recording. A user-initiated "Start tonight" begins only if OS permits, with prominent status and manual stop; intended local-time upper cutoff **09:00**, plus runtime safety deadline and fail-safe shutdown. Phone background execution and shutdown timing need target-device verification before any release; if safe stopping cannot be proven, recording must not ship. No auto-restart.

Prefer transient, on-device event detection with raw audio retention disabled; optional clips require a separate opt-in. Since private conversations/intimate sounds may be captured and cannot be perfectly filtered, provide transparent disclosure about bystander recording and appropriate consent. Exclude audio from cloud upload, logging, analytics and third-party services by default. Support deletion/revocation and ensure profile isolation. Full interaction and acceptance criteria are in [ROADMAP.md](../../ROADMAP.md). This does not expand MH-PROT-001 or authorize active microphone development.

## Scope re-evaluation: microphone recording
**OWNER DECISION PENDING — consider removal from future roadmap.** This is a preserved idea, NOT an accepted delivery commitment. Third-person bedroom audio and intimate conversations create particularly high privacy/KVKK/legal risks; potential value for the initial caregiver/medication audience is currently unvalidated. Do not schedule or implement S3 audio recording until the owner explicitly reaffirms this feature based on user evidence, legal/privacy review, and device-level safe operation. Consent from the primary phone user does not automatically address rights of others present. S1 read-only sleep viewing remains a distinct candidate.
