I've done a thorough pass. The model has come a long way — the Tech→App seam is now properly built. Here's everything, by severity.

Part 1 — Focus 2 Detailed Model: Full Review

117 elements (40 Business · 33 App · 38 Tech · 1 Physical), 162 relationships.

✅ What's solid (keep as-is)

- Tech→App seam is complete (11 links) — 4 SystemSoftware→Component realizations (HMS, Door System Software, Energy Management System, Staff Portal) + 7 TechService→AppProcess/Component servings. This is correct and well done.
- App layer internal decomposition is excellent — 3 process containers each with functions, junctions (Authorization/Card Validity Decision), events (Confirmed/Denied), and data accesses.
- Business cleaned of the low-level mechanics (Read RFID card, Close/Open Electrical Circuit, etc. are gone). ✓
- Energy Saving Service rename is done; the energy clash is resolved. ✓
- Daily Briefing cluster is well-modeled (roles → AND-junction → Conduct Meeting, data reads/writes).
- Energy Saver Interface → Room Electrical Circuit (Physical) ✓

🔴 Must fix (will read as errors)

1. Empty Product + empty Grouping boxes still present — delete both (junk, no label/meaning).
2. Conduct Briefing Meeting (copy) — drop the "(copy)" suffix.
3. Invalid cross-layer relationship: Key Card Configuration Process (App) —Realization→ Configure Access Rights (Business process). An app process realizing a business process is non-standard ArchiMate. Replace with Key Card Configuration Service (App) —Serving→ Configure Access Rights.

🟡 Incomplete seams (the biggest substantive gap)

4. Business actors aren't assigned to their processes — the "who does it" is missing:

┌────────────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────┐
│     Add: Actor     │                                       Assignment → Process                                        │
├────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────┤
│ Receptionist       │ Issue Key Card, Configure Access Rights, Encode Key Card (currently only triggers Check-In event) │
├────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────┤
│ Guest              │ Request Room Access (currently unconnected)                                                       │
├────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────┤
│ Housekeeping teams │ Activate Room Power / Request Room Access (currently orphan)                                      │
└────────────────────┴───────────────────────────────────────────────────────────────────────────────────────────────────┘

5. App→Business serving is half-done — room-access & key-card business processes are served by no app service (only energy + briefing are). Add Serving:
- Key Card Configuration Service → Issue Key Card, Configure Access Rights, Encode Key Card
- Access Verification Service → Request Room Access, Verify Access Auth, Grant Room Access

6. App→Business realization spine is incomplete — only Access Verification Service → Room Access Service exists. Add Realization:
- Key Card Configuration Service → Key Card Issuance Service
- Briefing Report Generation Service → Daily Operational Briefing Service
- Energy Activation Service → Energy Saving Service

🟡 Out-of-scope clutter (remove from this view)

Hotel Website, Transfer Management System (inside AWS), Payment Terminal, Reception Phone, Guest Room Telephone — none are Focus 2; they float unconnected at the bottom-right and add noise.

🟢 Polish

- Orphans: Housekeeping teams, Reception Phone, Guest Room Telephone (connect per #4 or remove).
- Layout: a few very long diagonal lines (e.g. Energy Activation Service spanning the whole canvas). Removing the out-of-scope tech and nudging the three app services up toward the business band would shorten them.
