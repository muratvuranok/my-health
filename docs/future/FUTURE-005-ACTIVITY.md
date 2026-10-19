Status: BACKLOG — NOT SCHEDULED; NO IMPLEMENTATION GO
Date: 2026-10-19
Owner: My Health MASTER

# FUTURE-005 — Workouts and route maps

**Archived concept, not implementation authority.** This is supporting context only; MH-PROT-001 is unchanged. Future specification requires explicit owner phase GO and evidence of user value.

## Activity, workouts, walking and route maps — phased product direction (owner, 2026-10-19)
The owner requests that **completed workouts, walking sessions and available map/location routes are visible in detail inside My Health**. Treat this as an approved **product direction**, not an authorization to implement or add it to MH-PROT-001. Sequence delivery in small usable increments:

| Stage | User-visible outcome | Scope boundary |
|---|---|---|
| Stage A — After phone foundation | Activity history and session detail on native iOS/Android phone apps | Read-only imported walking/running/other workout sessions where the user has granted relevant HealthKit/Health Connect permissions; session type, source, start/end, duration, distance, calories/steps/heart-rate statistics **when available**, timezone and provenance. Show missing data as unavailable, never invent it. |
| Stage B — Route details | Workout route drawn on an interactive phone map when a supported source actually supplies GPS track data and user opts in to route access | Polyline, start/end points, distance/pace timeline and optional splits/elevation only when data is present and reliable. Sessions without location still render a complete non-map detail view. No automatic location capture merely from granting health/workout access. |
| Stage C — Watch companionship | Apple Watch/watchOS and Wear OS companion activities and optional session information | Leverage supported phone/watch workout sources; watch workout recording or watch-based route capture requires a *separate future* feasibility/scope decision, not implicit watch app permission. |
| Stage D — Optional advanced analysis | Comparisons and longer-term activity trends | Only after real source data quality and consent model are proven; no diagnostic or clinical coaching claims. |

**Privacy:** Exercise routes and coordinates can expose a home, workplace or recurring habits. Workout-data permission and precise location/route permission are separate. Use least-privilege, optional imports, profile isolation, local-first handling, map visibility controls, deletion, and explicitly controlled sharing/export; **no default cloud upload or public map**. Keep raw routes out of logs and analytics. If HealthKit/Health Connect or a device omits route detail, say so transparently. Region, OS and provider capability are to be validated when that stage starts.

**Immediate scope:** No activity tracker, route recording, map SDK, watch code or external agent work is added to current **MH-PROT-001 PLAN ONLY**. Once the phone medication/profile prototype works, deliver the smallest read-only activity history first; route display and more advanced watch functions follow in separate owner-approved tasks.

## Device integration notes
## Workout and location provenance (phased; no implementation GO)
My Health will eventually **display detailed workout and walking records**, including a **route map only when usable GPS track samples are available** from an authorized phone/watch/platform source. Workout sessions and precise routes must remain separate permission/data categories; a health-record grant does not itself authorize raw GPS collection, cloud sharing or public route visibility.

Initial read-only consideration: HealthKit workouts and route-capable sources on iOS; Health Connect exercise sessions and supported route access paths on Android (exact platform availability, permission and user-consent behavior must be verified at implementation). Apple Watch and Wear OS data may arrive via supported platform health stores; a dedicated watch recording app is not required for the first phone-side activity history and is NOT approved by this note.

Candidate session fields for future design (not an approved API schema): source ID/device, profile_id, activity type, start/end and timezone, elapsed/moving duration, distance, optional steps/calories/heart-rate statistics, source/provenance, and optional track points with timestamps. Keep track data local-first and protected, allow user to hide/delete maps, and handle absent or incomplete routes without implying that a workout did not occur.

This is **not** a new BLE medical-device initiative, does not add anything to MH-PROT-001, and does not grant permission for background location collection, health-data upload, external map services or use of real health data. See [ROADMAP](../../ROADMAP.md) for sequencing.
