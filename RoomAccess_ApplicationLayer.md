# Room Access — Application Layer (Detailed)

**View:** `Room Access — Application Layer (Detailed)` (in `ArchiHotel.archimate`)
**Image:** `images/RoomAccess_ApplicationLayer_Detailed.png`
**Sources (only these — nothing external):**
- `ArchiHotel_ The Genesis Files.pdf` / https://archihotel-eam.web.app/  → all content
- Colleague's Business-layer draft (screenshot) → only the *names* used in the merge box

---

## 0. Sourcing key

Every element is tagged:
- **[NAMED]** — the exact term appears in the PDF.
- **[DERIVED]** — standard ArchiMate decomposition of a behavior the PDF *describes* but does not name as a discrete step/service. (This is interpretation, the same approach as the reference CampusCard model.)
- **[INFERRED+]** — a reasonable addition that is only *implied*, weakest grounding — review before final submission.

> Honest note: only the three components and `Receptionist Interface` are literally written in the PDF. Services, functions, events, junctions and data objects are DERIVED — they are how you model the described behaviour in ArchiMate, not copied wording.

---

## 1. Elements

### 1.1 Application Components (the systems)

| Element | Type | Grounding | Source |
|---|---|---|---|
| Door System Software | ApplicationComponent | **[NAMED]** | p.12 "Door Card Reader connected to the **Door System Software**" |
| Hotel Management System | ApplicationComponent | **[NAMED]** | p.4 "issue and configure a key card through the **HMS**" |
| Energy Management System | ApplicationComponent | **[NAMED]** | p.12 "connected to the hotel's **energy management system**" |

### 1.2 Application Interfaces (access points)

| Element | Type | Grounding | Source |
|---|---|---|---|
| Receptionist Interface | ApplicationInterface | **[NAMED]** | p.17 "Hotel Management System and **Receptionist Interface**" |
| Card Reader Interface | ApplicationInterface | **[DERIVED]** | p.12 the door card reader is the access point to Door System Software |

### 1.3 Application Services (exposed behaviour)

| Element | Type | Grounding | Source sentence it exposes |
|---|---|---|---|
| Access Verification Service | ApplicationService | **[DERIVED]** | p.12 "The system **verifies** whether the guest has valid access permissions" |
| Door Unlock Service | ApplicationService | **[DERIVED]** | p.12 "**unlocks the door** only if authorization is confirmed" |
| Key Card Configuration Service | ApplicationService | **[DERIVED]** | p.4 "**issue and configure a key card** through the HMS" |
| Access Rights Assignment Service | ApplicationService | **[DERIVED]** | p.4 "**access rights for rooms and service areas are assigned digitally**" |
| Energy Activation Service | ApplicationService | **[DERIVED]** | p.12 "**activates the room's electrical circuit**" |

### 1.4 Application Processes (behaviour containers — one per component)

| Element | Type | Grounding |
|---|---|---|
| Access Verification Process | ApplicationProcess | **[DERIVED]** container for Door System Software behaviour |
| Key Card Configuration Process | ApplicationProcess | **[DERIVED]** container for HMS card behaviour |
| Energy Activation Process | ApplicationProcess | **[DERIVED]** container for Energy Mgmt behaviour |

### 1.5 Application Functions (the decomposed steps)

| Element | Belongs to | Grounding | Source basis |
|---|---|---|---|
| Read Card Data | Access Verification | **[DERIVED]** | p.12 "access rights stored on the card are **transmitted securely via RFID**" |
| Authenticate Card | Access Verification | **[INFERRED+]** | *not explicit* — only implied; PDF mentions verifying permissions, not authenticating the card. Review/optional. |
| Validate Access Rights | Access Verification | **[DERIVED]** | p.12 "verifies whether the guest has **valid access permissions**" |
| Send Unlock Signal | Access Verification | **[DERIVED]** | p.12 "**unlocks the door**" |
| Maintain Lock | Access Verification | **[DERIVED]** | p.12 "unlocks the door **only if** authorization is confirmed" (else stays locked) |
| Create Card Record | Key Card Config | **[DERIVED]** | p.4 "**issue** … a key card" |
| Assign Access Rights | Key Card Config | **[DERIVED]** | p.4 "access rights … are **assigned digitally**" |
| Encode Card Data | Key Card Config | **[DERIVED]** | p.4/p.12 "**configure** a key card" + "access rights **stored on the card**" |
| Detect Card Insertion | Energy Activation | **[DERIVED]** | p.12 "only when a valid key card is **inserted**" |
| Verify Card Validity | Energy Activation | **[DERIVED]** | p.12 "only when a **valid** key card is inserted" |
| Activate Room Circuit | Energy Activation | **[DERIVED]** | p.12 "**activates the room's electrical circuit**" |
| Deactivate Room Circuit | Energy Activation | **[DERIVED]** | p.12 "reduce … energy consumption **when guests are away**" |

### 1.6 Application Events (outcomes)

| Element | Grounding | Source |
|---|---|---|
| Authorization Confirmed | **[DERIVED]** | p.12 "only **if authorization is confirmed**" |
| Authorization Denied | **[DERIVED]** | implied opposite of the above |
| Circuit Activated | **[DERIVED]** | p.12 "activates the … circuit" |

### 1.7 Junctions (branch points — pure modelling constructs)

| Element | Grounding |
|---|---|
| Authorization Decision | **[DERIVED]** valid/invalid branch after Validate Access Rights |
| Card Validity Decision | **[DERIVED]** valid/no-card branch in energy activation |

### 1.8 Data Objects (information used)

| Element | Type | Grounding | Source |
|---|---|---|---|
| Card Credential | DataObject | **[DERIVED]** | p.12 "access rights **stored on the card**" |
| Access Rights Record | DataObject | **[DERIVED]** | p.12 "verifies whether the guest has valid access permissions" (the stored permissions) |
| Room Power State | DataObject | **[DERIVED]** | p.12 "activates the room's electrical circuit" (on/off state) |

---

## 2. Connectors (relationships) — internal to the application layer

### 2.1 Component → Process (Assignment ●)
*A component performs its behaviour.*

| Source | → | Target |
|---|---|---|
| Door System Software | Assignment | Access Verification Process |
| Hotel Management System | Assignment | Key Card Configuration Process |
| Energy Management System | Assignment | Energy Activation Process |

### 2.2 Process → Service (Realization △)
*The behaviour realizes the exposed service.*

| Source | → | Target |
|---|---|---|
| Access Verification Process | Realization | Access Verification Service |
| Access Verification Process | Realization | Door Unlock Service |
| Key Card Configuration Process | Realization | Key Card Configuration Service |
| Key Card Configuration Process | Realization | Access Rights Assignment Service |
| Energy Activation Process | Realization | Energy Activation Service |

### 2.3 Process ◆ Function (Composition — shown as nesting)
*Each process is composed of its steps.*

- Access Verification Process ◆ { Read Card Data, Authenticate Card, Validate Access Rights, Send Unlock Signal, Maintain Lock }
- Key Card Configuration Process ◆ { Create Card Record, Assign Access Rights, Encode Card Data }
- Energy Activation Process ◆ { Detect Card Insertion, Verify Card Validity, Activate Room Circuit, Deactivate Room Circuit }

### 2.4 Step → Step (Triggering →)
*The order of execution.*

**Access Verification:**
`Read Card Data → Authenticate Card → Validate Access Rights → ◇ Authorization Decision`
`◇ → Authorization Confirmed → Send Unlock Signal`
`◇ → Authorization Denied → Maintain Lock`

**Key Card Configuration:**
`Create Card Record → Assign Access Rights → Encode Card Data`

**Energy Activation:**
`Detect Card Insertion → Verify Card Validity → ◇ Card Validity Decision`
`◇ → Activate Room Circuit → Circuit Activated`
`◇ → Deactivate Room Circuit`

### 2.5 Function → Data Object (Access ↔)
*Read or write of information.*

| Function | Access | Data Object |
|---|---|---|
| Read Card Data | read | Card Credential |
| Authenticate Card | read | Card Credential |
| Validate Access Rights | read | Access Rights Record |
| Assign Access Rights | write | Access Rights Record |
| Encode Card Data | write | Card Credential |
| Activate Room Circuit | write | Room Power State |
| Deactivate Room Circuit | write | Room Power State |

### 2.6 Component ◆ Interface (Composition) + Interface → Service (Assignment ●)
*The component offers its service through an interface.*

| Component | ◆ | Interface | ● exposes | Service |
|---|---|---|---|---|
| Door System Software | Composition | Card Reader Interface | Assignment | Access Verification Service |
| Hotel Management System | Composition | Receptionist Interface | Assignment | Key Card Configuration Service |

---

## 3. Cross-layer merge connectors (for the 6-person merge)

This view is the **application layer only**. When merged with teammates' Business and Technology layers, wire these. (Two orange boxes on the diagram show the same.)

### 3.1 ⬆ UP to ROOM ACCESS · BUSINESS LAYER (teammate)
*Relationship: `ApplicationService —Serving→ BusinessProcess`. Business names from colleague's draft.*

| My Application Service | Serving → | Their Business Process |
|---|---|---|
| Access Verification Service | serves | Verify Access Authorization |
| Door Unlock Service | serves | Grant Room Access |
| Key Card Configuration Service | serves | Issue Key Card / Configure Access Rights |
| Access Rights Assignment Service | serves | Configure Access Rights |
| Energy Activation Service | serves | Grant Room Access (room electricity) |

*Data alignment (`DataObject —Realization→ BusinessObject`):*
| My Data Object | Realization → | Their Business Object |
|---|---|---|
| Access Rights Record | realizes | Access Rights Profile |
| Card Credential | realizes | Key Card |

### 3.2 ⬇ DOWN to ROOM ACCESS · TECHNOLOGY LAYER (teammate)
*Relationships: `Node/SystemSoftware —Realization→ ApplicationComponent`; `Device —Serving→ Application`. Tech names from PDF p.11–12.*

| Their Technology element | → | My Application element |
|---|---|---|
| Door System Software Runtime (SystemSoftware) on Door Control Node | realizes → | Door System Software |
| Door Card Reader (Device) via RFID Communication (Network) | serves → | Card Reader Interface |
| AWS Cloud Server (Node) | realizes → | Hotel Management System |
| Energy Saver Interface (Device, card switch) | serves → | Energy Management System |
| Reception PC & Terminal (Device) | serves → | Receptionist Interface |

### 3.3 ↔ ACROSS to DAILY BRIEFING · APPLICATION LAYER (teammate, same layer)
*This is a cross-**domain** link at the same (application) layer, not a cross-layer one.*

**The connector is component identity, NOT a relationship line.** The PDF stresses *"the same Hotel Management System"* runs both domains, so:

| My Application element | merge action | Their (Daily Briefing) element |
|---|---|---|
| Hotel Management System | **is the SAME element** — keep one, do not duplicate | Hotel Management System |

- In **this** view the HMS performs the **Key Card Configuration Process**.
- In the **Daily Briefing** view the same HMS performs **Briefing Report Generation** (Compile Briefing Data, etc.).
- On merge: keep a single `Hotel Management System` component; both processes attach to it.

**Shared data:** guest profiles & reservations stored centrally in the HMS are the same source the briefing reads; access rights are assigned from that data at check-in.

**Important honesty note:** the source describes **no direct service call** between Room Access and Daily Briefing (the briefing never reads access rights; room access never reads the briefing). They are *parallel capabilities on one shared platform*. So do **not** invent a Serving/Flow connector between the two domains — the only real link is the shared HMS component (and, at the technology layer, the shared `AWS Cloud Server`).

---

## 4. Transparency — what is and isn't verbatim from the source

This section is the explicit audit of grounding, so nothing is taken on trust.

### 4.1 Literally written in the PDF (verbatim terms)
Only these application-layer names actually appear in the source text:
- **Door System Software** — p.12
- **Hotel Management System** (HMS) — p.4, p.11, p.17
- **Energy Management System** — p.12
- **Receptionist Interface** — p.17

Plus the supporting concepts (not elements themselves): *key card, access rights, RFID, card switch, electrical circuit, valid access permissions, unlock the door*.

### 4.2 DERIVED — not in the source as words, added by ArchiMate modelling
These are **my interpretation** of behaviour the PDF *describes* but does not name as discrete services/steps/data. They are standard decomposition (the same approach as the CampusCard reference), **not** outside information:
- **All Application Services** — Access Verification, Door Unlock, Key Card Configuration, Access Rights Assignment, Energy Activation
- **All Application Processes** — the three container processes
- **Most Application Functions** — Read Card Data, Validate Access Rights, Send Unlock Signal, Maintain Lock, Create Card Record, Assign Access Rights, Encode Card Data, Detect Card Insertion, Verify Card Validity, Activate Room Circuit, Deactivate Room Circuit
- **All Application Events** — Authorization Confirmed, Authorization Denied, Circuit Activated
- **Both Junctions** — Authorization Decision, Card Validity Decision
- **All Data Objects** — Card Credential, Access Rights Record, Room Power State
- **Card Reader Interface** (the access point is implied by "Door Card Reader", but the interface element + name are mine)

Each of these is traceable to a specific PDF sentence — see the per-element source columns in §1.

### 4.3 INFERRED+ — weakest grounding, review before submission
These go a step beyond what the PDF even implies and should be consciously kept or removed:

| Element | Why it's flagged | Source reality |
|---|---|---|
| **Authenticate Card** (ApplicationFunction) | Adds a card-authenticity step before permission validation. | The PDF only says the system *"verifies whether the guest has valid access permissions"* (p.12) — that is `Validate Access Rights`. It does **not** mention authenticating the card itself. **Recommendation:** delete it for a strictly source-backed model, or keep it if your team wants to show a security/anti-cloning step. |

> Bottom line: the **structure** (3 components + their decomposition) is faithful to the PDF; the **specific service/step/data names** are modelled, not quoted; and exactly **one** element (`Authenticate Card`) is an addition beyond the text.

---

## 5. Review-before-submission checklist
- [ ] `Authenticate Card` is **[INFERRED+]** (see §4.3) — keep only if you want a card-authenticity step; otherwise delete (PDF only states permission verification).
- [ ] Confirm the colleague's final business names match §3.1 (they came from a draft screenshot).
- [ ] Confirm the technology teammate's element names match §3.2.
- [ ] The composition lines inside each process group can be hidden (pure nesting) if you prefer the cleaner reference look — ask to toggle.

---

*Sources: ArchiHotel — The Genesis Files (PDF / website) · © 2026 TUM Chair of Information Systems Heilbronn. Business-layer element names referenced from a teammate's draft diagram.*
