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
| Validate Access Rights | Access Verification | **[DERIVED]** | p.12 "verifies whether the guest has **valid access permissions**" |
| Send Unlock Signal | Access Verification | **[DERIVED]** | p.12 "**unlocks the door**" (fires only on the confirmed branch) |
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
| Authorization Confirmed | **[DERIVED]** | p.12 "only **if authorization is confirmed**" → triggers the unlock |
| Authorization Denied | **[DERIVED]** | implied opposite — **terminal** outcome; deny is the *default*, door stays locked, no follow-up action |

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

- Access Verification Process ◆ { Read Card Data, Validate Access Rights, Send Unlock Signal }
- Key Card Configuration Process ◆ { Create Card Record, Assign Access Rights, Encode Card Data }
- Energy Activation Process ◆ { Detect Card Insertion, Verify Card Validity, Activate Room Circuit, Deactivate Room Circuit }

### 2.4 Step → Step (Triggering →)
*The order of execution.*

**Access Verification:**
`Read Card Data → Validate Access Rights → ◇ Authorization Decision`
`◇ → Authorization Confirmed → Send Unlock Signal`
`◇ → Authorization Denied` *(terminal — deny is the default; door simply stays locked, no action)*

**Key Card Configuration:**
`Create Card Record → Assign Access Rights → Encode Card Data`

**Energy Activation:**
`Detect Card Insertion → Verify Card Validity → ◇ Card Validity Decision`
`◇ → Activate Room Circuit` *(valid card)*
`◇ → Deactivate Room Circuit` *(no valid card / guest away)*

### 2.5 Function → Data Object (Access ↔)
*Read or write of information.*

| Function | Access | Data Object |
|---|---|---|
| Read Card Data | read | Card Credential |
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
- **All Application Functions** — Read Card Data, Validate Access Rights, Send Unlock Signal, Create Card Record, Assign Access Rights, Encode Card Data, Detect Card Insertion, Verify Card Validity, Activate Room Circuit, Deactivate Room Circuit
- **All Application Events** — Authorization Confirmed, Authorization Denied
- **Both Junctions** — Authorization Decision, Card Validity Decision
- **All Data Objects** — Card Credential, Access Rights Record, Room Power State
- **Card Reader Interface** (the access point is implied by "Door Card Reader", but the interface element + name are mine)

Each of these is traceable to a specific PDF sentence — see the per-element source columns in §1.

### 4.3 INFERRED+ items — now REMOVED in the model cleanup
The earlier version had one element beyond the text (`Authenticate Card`). During the model review it was **deleted**, along with two redundant boxes, to keep the model strictly source-backed and clean:

| Removed element | Why removed |
|---|---|
| **Authenticate Card** (ApplicationFunction) | Not in the source — the PDF only says the system *"verifies … valid access permissions"* (= `Validate Access Rights`). It also duplicated `Read Card Data`'s read of `Card Credential`. Flow rewired to `Read Card Data → Validate Access Rights`. |
| **Maintain Lock** (ApplicationFunction) | Deny is now the **default** (no action) — `Authorization Denied` is a terminal outcome, so an explicit "keep locked" action was unnecessary. |
| **Circuit Activated** (ApplicationEvent) | Redundant action→event chain (`Activate Room Circuit → Circuit Activated`). The junction now branches directly to `Activate` / `Deactivate Room Circuit`. |

> Bottom line: after cleanup, **every** element is `[NAMED]` or `[DERIVED]` — there are **no [INFERRED+] additions left**. Each component is consistently `component ●→ process ─△▷ service`, the process composes its functions, and data is accessed by the functions (not the services).

---

## 5. Review-before-submission checklist
- [x] `Authenticate Card`, `Maintain Lock`, `Circuit Activated` removed; model is now strictly `[NAMED]`/`[DERIVED]` (see §4.3).
- [x] Each component consistent: `component ●→ process ─△▷ service`; redundant component→service / function→service links removed.
- [ ] Confirm the colleague's final business names match §3.1 (they came from a draft screenshot).
- [ ] Confirm the technology teammate's element names match §3.2.
- [ ] The composition lines inside each process group can be hidden (pure nesting) if you prefer the cleaner reference look — ask to toggle.

---

## 6. The Modeling Story — from text to diagram, step by step

This section tells the whole story: which **sentence** produced which **box**, why that **label**, why that **ArchiMate symbol**, and why each **arrow** (relationship type) connects it to the next. It doubles as a script for defending the diagram.

### 6.0 Symbol legend (why each shape means what)
- **Application Component** (box with two tabs ▯) — a named piece of software. Use when the text *names a system*.
- **Application Service** (rounded ▭) — the behaviour a component exposes *outward* ("what it promises to provide").
- **Application Function** (chevron ⌃) — one *internal automated step* the component performs.
- **Application Process** (arrow ⇒) — an *ordered* set of steps that produces a result; used as the container for the flow.
- **Application Interface** (socket ⊙—) — the *access point* through which a service is reached.
- **Data Object** (▭) — information that functions read/write.
- **Junction** (small circle ●) — a *branch/merge* (used for "only if" decisions).
- **Application Event** (notched ▢) — *something that happens* (an outcome/result).

Relationship arrows:
- **Assignment** (line, filled ball ●——) — active structure *performs* behaviour (**component → process**) or interface *exposes* service. *(In this model the component is assigned to the whole **process**, not to each function — the functions belong to the process via Composition.)*
- **Composition** (filled diamond ◆—) — *whole–part* (a process is made of its steps; a component is made of its interface).
- **Triggering** (solid arrow ──▶) — *temporal/causal order* ("then this happens").
- **Access** (dashed arrow ┄▶) — a function *reads/writes* a data object.
- **Realization** (dashed, hollow triangle ┄▷) — a *concrete* element realizes a *more abstract* one (function → service).
- **Serving** (solid open arrow ─▷) — one element *provides to* another (used for the cross-layer connectors in §3).

---

> **Note (model-reconciled):** this story matches the **cleaned** model — `Authenticate Card`, `Maintain Lock`, `Circuit Activated` were removed and the redundant component→service / function→service links dropped (see §4.3). Every component now follows one pattern: **component ●→ process**, **process ◆ functions**, **process ─┄▷ services**, **functions ┄▶ data**.

### Thread A — Verifying the card and opening the door (Door System Software)

**Step A1 — the system itself**
> *"Every guest room door is equipped with a Door Card Reader connected to the **Door System Software**."* (p.12)

The sentence **names a piece of software**, so I drew a box labelled **Door System Software** as an **Application Component** (component symbol = a deployable software unit). The *Door Card Reader* is hardware → Technology layer, so here it only appears as the point where requests arrive: an **Application Interface** labelled **Card Reader Interface** (socket symbol), attached to the component with a **Composition** (◆ — the interface is part of the component).

**Step A2 — reading the card**
> *"When a guest holds their key card near the reader, **access rights stored on the card are transmitted** securely via RFID technology."* (p.12)

"Read from the card" is an *internal step*, so it became an **Application Function** **Read Card Data** (chevron). The data on the card is a **Data Object** **Card Credential**, and this function reads it → **Read Card Data ┄▶ Card Credential** (Access, read). *(There is no direct `Door System Software → Read Card Data` line — the component performs the whole **process**, and Read Card Data belongs to that process via Composition; see A5. An earlier draft had a redundant `Authenticate Card` reading the same Card Credential — removed in cleanup.)*

**Step A3 — checking the permissions**
> *"The system **verifies whether the guest has valid access permissions**…"* (p.12)

"verifies … permissions" is the core check → **Application Function** **Validate Access Rights**, which reads the **Data Object** **Access Rights Record** (Access, read). Order matters — read the card *then* check it — so they are joined by a **Triggering** arrow: `Read Card Data ──▶ Validate Access Rights`. Triggering (not Access) because this is *sequence*, not data.

**Step A4 — the "only if" decision**
> *"…and **unlocks the door only if authorization is confirmed**."* (p.12)

"**only if**" is a conditional, so I placed a **Junction** (●) **Authorization Decision** right after Validate Access Rights (Triggering in). Two **Triggering** arrows lead to two **Application Events** (notched) — **Authorization Confirmed** and **Authorization Denied** — because an *event* is the right symbol for "a result that occurred." **Authorization Confirmed ──▶ Send Unlock Signal** (the unlock action). **Authorization Denied** is a **terminal event**: *deny is the default* — the door simply stays locked, so there is deliberately **no** "Maintain Lock" action (that box was removed). The junction + the two events capture *"only if"* while keeping the negative path a do-nothing default.

**Step A5 — who performs it, and what it offers outward**
The three steps are wrapped in an **Application Process** **Access Verification Process** (arrow symbol = ordered behaviour) that **Composes** (◆) `Read Card Data`, `Validate Access Rights`, `Send Unlock Signal`. The **Door System Software** *performs* that process — a single **Assignment** (Door System Software ●—— Access Verification Process). The process exposes two **Application Services** (rounded) — **Access Verification Service** and **Door Unlock Service** — via **Realization** (Access Verification Process ┄▷ each). The **Card Reader Interface** is **Assigned** to the Access Verification Service it exposes.
→ Consistent pattern: **component ●→ process ─┄▷ service**; the services are realized *only* by the process (the old function-level `Validate Access Rights → Access Verification Service` realization was redundant and was removed — that fixes the asymmetry where `Door Unlock Service` had no matching function-level arrow).

---

### Thread B — Issuing & configuring the card at check-in (Hotel Management System)

**Step B1 — the system + its steps**
> *"Once a guest checks in, the receptionist can immediately **issue and configure a key card through the HMS**; **access rights for rooms and service areas are assigned digitally**."* (p.4)

"HMS" is a named system → **Application Component** **Hotel Management System**. The verbs give three **Application Functions**: **Create Card Record** ("issue"), **Assign Access Rights** ("assigned digitally"), **Encode Card Data** ("configure … the card"). Order: **Create Card Record ──▶ Assign Access Rights ──▶ Encode Card Data** (Triggering). The three are **Composed** into an **Application Process** **Key Card Configuration Process**, which the HMS *performs* via a **single Assignment** (HMS ●—— Key Card Configuration Process — not three separate links). The process **Realizes** the **Application Services** **Key Card Configuration Service** and **Access Rights Assignment Service**.
Data writes: **Assign Access Rights ┄▶ Access Rights Record** (write) and **Encode Card Data ┄▶ Card Credential** (write).

**Step B2 — the receptionist's access point**
> *"Hotel Management System and **Receptionist Interface**."* (p.17)

A named interface → **Application Interface** **Receptionist Interface** (socket), **Composed** into the HMS and **Assigned** to the Key Card Configuration Service it exposes — the access point the receptionist uses.

---

### Thread C — Energy via the card switch (Energy Management System)

**Step C1 — the system + its steps**
> *"The same key card infrastructure is also connected to the hotel's **energy management system**. Inside each guest room, an Energy Saver Interface with a card switch **activates the room's electrical circuit only when a valid key card is inserted**."* (p.12)

Named system → **Application Component** **Energy Management System**. The clause gives the steps: "inserted" → **Detect Card Insertion**, "valid … key card" → **Verify Card Validity**, "activates the … circuit" → **Activate Room Circuit**; and the away-case → **Deactivate Room Circuit** (all **Application Functions**). The repeat of **"only when [valid]"** → a **Junction** **Card Validity Decision** that branches **directly** to the two actions:
`Detect Card Insertion ──▶ Verify Card Validity ──▶ ◇ Card Validity Decision`
`◇ → Activate Room Circuit` *(valid card)* `;` `◇ → Deactivate Room Circuit` *(no valid card / guest away)*
Both write the **Data Object** **Room Power State** (Access, write). All wrapped in **Application Process** **Energy Activation Process**; EMS ●—— process (Assignment); process ┄▷ **Energy Activation Service** (Realization).
*(An earlier draft had `Activate Room Circuit ──▶ Circuit Activated` (event) — a redundant action→event chain — so the event was removed and the junction now branches straight to the two actions. This is the C1 fix.)*

**Step C2 — a sentence I deliberately did NOT turn into a box**
> *"This helps **reduce unnecessary energy consumption** when guests are away."* (p.12)

This is the *reason/goal*, not application behaviour → **no application box** (in strict ArchiMate it's a **Driver/Goal in a Motivation view**). Noting it shows the omission is deliberate.

---

### 6.1 How it reads as one diagram
Top to bottom the view tells the layered story: **Services** (the promises) on top; the **Processes** with their **Functions / junctions / events** (the how) in the middle, each as the behaviour of one component; the **Components + Interfaces** at the bottom performing them; **Data Objects** alongside, read/written by the functions. Reading any vertical slice answers: *what is promised → how it's done → who performs it → what data it touches.* The one rule repeated three times: **component ●→ process ─┄▷ service**, **process ◆ functions**, **functions ┄▶ data**.

---

*Sources: ArchiHotel — The Genesis Files (PDF / website) · © 2026 TUM Chair of Information Systems Heilbronn. Business-layer element names referenced from a teammate's draft diagram.*
