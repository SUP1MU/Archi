# ArchiHotel — Focus 2: Room Access & Daily Briefing
**Source:** C2 ArchiHotel — *The Genesis Files* (https://archihotel-eam.web.app/)
**Purpose:** Material extraction + ArchiMate modeling guide + presentation script
**Course:** EAM — TUM Chair of Information Systems Heilbronn

> This file isolates **only** the parts of the ArchiHotel case relevant to Focus 2.
> Focus 2 actually covers **two intertwined sub-domains**:
> 1. **Daily Briefing** — how the hotel turns overnight reservations into a coordinated morning plan.
> 2. **Room Access** — how guests and staff physically enter rooms/facilities via the key-card system (and how it drives energy management).
> They connect because the **same Hotel Management System (HMS)** that prepares the briefing also issues the **key cards** at check-in.

---

## 0. TL;DR — One-sentence framing for each sub-domain
- **Daily Briefing:** Every morning the HMS auto-assembles the *Arrival Schedule, VIP List and Briefing Report*; the Hotel Manager + Concierge/Housekeeping leads review it in a short meeting, then push coordination to staff phones via the **Staff Portal** app over hotel Wi-Fi.
- **Room Access:** At check-in the receptionist issues & configures a **key card** in the HMS; **Door Card Readers** verify the card via **RFID** against the **Door System Software**, unlock the door, and the same card drives the in-room **Energy Saver** circuit.

---

## 1. Relevant Raw Material (verbatim-derived, trimmed to Focus 2)

### 1.1 Daily Briefing — core narrative
- Approved reservations are automatically added to the hotel's **Arrival Schedule**, which forms the basis for the **daily operational briefing process**.
- Every morning, the **Hotel Management System generates a briefing report** containing: the day's **arrivals, guest information, VIP guests, special requests, and operational updates**. It gives all departments a shared, up-to-date overview.
- The **daily briefing meeting** is led by the **Hotel Manager** together with the **Heads of Concierge and Housekeeping**. They:
  - review **VIP arrivals**,
  - coordinate **cross-department activities**,
  - discuss **special guest requirements**,
  - align **staffing priorities** for the day.
- Goal: all departments work from the **same operational picture**, not isolated fragments.
- If extra coordination is needed, staff are informed through the **Staff Portal** app installed on their **Android smartphones**:
  - distributes **push notifications** for urgent requests / special instructions / operational changes,
  - provides a **dashboard** combining schedules, tasks, arrivals and briefing info into one overview.
- Example uses: when multiple guests arrive simultaneously the **concierge lead** reassigns staff instantly; housekeeping gets advance instructions (e.g. **chilled champagne** placed before arrival).
- All staff access the **latest briefing report** through the app; it is **synchronized daily via the hotel's Wi-Fi** so every device stays current across floors and service zones.

**Manager memo (Emma Rousseau) — briefing-relevant lines:**
- "Our morning briefing has changed… we spend less time collecting facts and more time deciding what matters most, because the **Arrival Schedule, VIP List, and Briefing Report are already assembled** for us."
- "We are not reacting to arrivals anymore. We are **preparing** for them."

**Stakeholder voice — Ellen M. (Hotel Manager):**
- "Before 9:00 AM, the concierge leads and housekeeping leads join me for a short operational briefing. We look at the day's arrivals, VIP guests, transfers, and any requests that need extra care, all based on what came in overnight."
- "The arrival schedule, VIP List and briefing report are already there, so we can focus on what needs attention, escalation and alignment."
- "Once priorities are agreed, the update goes out to staff phones through the **Staff Portal**, and the dashboard keeps the whole team aligned."

**Live dashboard data (Staff Portal, "Morning Briefing — 25 Mar 2026", LIVE · After 9:00 am):**
- KPIs: **14 Arrivals · 3 VIP · 5 Transfers · 8 Check-Outs**
- **VIP List:** Mr. Nakamura (Suite 401, Airport Transfer); Ms. Lindström (Suite 502, Champagne); Dr. Al-Rashid (Suite 301, Late Check-Out).
- **Housekeeping ToDos:** Suite 401 Deep Clean (VIP prep, before 10:30, PRIORITY); Room 215 Clean (post check-out 10:00, ASSIGNED); Room 308 Clean (11:00, PENDING); Suite 502 Deep Clean (VIP prep before 14:00, PENDING).
- **Coordinate Cross-Departmental Tasks:** ✓ Generate Briefing Report (reviewed); ✓ Broadcast Coordination (sent to all staff); ○ Address Special Requests (3 open); ○ Assign drivers for transfer bookings.

### 1.2 Room Access — core narrative
- **Room access throughout the hotel is managed through an integrated Key Card system.**
- Every guest room door has a **Door Card Reader** connected to the **Door System Software**.
- When a guest holds their key card near the reader, **access rights stored on the card are transmitted securely via RFID**.
- The system **verifies whether the guest has valid access permissions** and **unlocks the door only if authorization is confirmed** → controlled, secure, yet convenient access.
- **Check-in link:** Once a guest checks in, the receptionist can immediately **issue and configure a key card** through the HMS; **access rights for rooms and service areas are assigned digitally**, giving guests seamless access to their accommodation and authorized facilities.
- **Staff access:** Hotel staff (incl. housekeeping) use **authorized key cards** to enter rooms and activate room electricity while cleaning/maintaining.
- **Energy management link:** The same key-card infrastructure connects to the hotel's **energy management system**. Each room has an **Energy Saver Interface with a card switch** that activates the room's electrical circuit **only when a valid key card is inserted** → reduces energy use when guests are away.
- **Exception:** the **laundry room operates separately** from the card-switch power mechanism because washing equipment needs **continuous power**. (But access to the laundry room is still granted via the **key-card door system** that verifies the card on the door reader.)

### 1.3 Supporting infrastructure (shared by both sub-domains)
- Central **cloud infrastructure on AWS** hosts the **Hotel Management System**, the hotel website, and the Transfer Management System; stores reservations, guest profiles, VIP lists, pricing, room availability, transfer schedules, driver/vehicle info, and open costs.
- Reception, concierge, housekeeping and managers connect through the hotel's **Wi-Fi network**; data is synchronized continuously across devices.
- **Reception point setup:** personal computer + phone + payment terminal, kept synchronized with reservation data, guest profiles, open costs, room status and special requests in real time.
- Every guest room also has a **telephone** on the hotel's private telephone network (guests call reception/concierge for extra services).

---

## 2. Structured breakdown — Roles / Inputs / Outputs / Triggers

### 2.1 Daily Briefing

| Aspect | Detail |
|---|---|
| **Trigger** | Time-based: every morning (overnight reservations finalized → before 9:00 am). |
| **Primary actors (roles)** | Hotel Manager (leads); Head of Concierge; Head of Housekeeping. |
| **Secondary actors** | Reception staff, concierge team, housekeeping team, service teams (consumers of the broadcast). |
| **Inputs** | Reservations, Guest information/profiles, **Arrival Schedule**, **VIP List**, Special Requests, Open Costs, operational updates (overnight). |
| **Process steps** | 1) HMS **generates Briefing Report** → 2) Manager + leads **conduct briefing meeting** (review VIPs, coordinate cross-dept, discuss special requests, set staffing priorities) → 3) **Broadcast coordination** to staff via Staff Portal → 4) **Address special requests** / **assign drivers** → 5) Staff act using **dashboard**. |
| **Outputs** | **Briefing Report** (shared view); agreed **priorities/staffing plan**; **push notifications**; synchronized **dashboard**; assigned housekeeping/transfer tasks. |
| **Supporting app systems** | **Hotel Management System** (report generation), **Staff Portal** (Android app: notifications + dashboard). |
| **Supporting tech** | **AWS central server**, **Wi-Fi network** (daily sync), **Android smartphones**. |
| **Value delivered** | Shared operational picture; proactive (not reactive) preparation; faster meetings; fewer handoff errors. |

### 2.2 Room Access

| Aspect | Detail |
|---|---|
| **Triggers** | (a) Check-in event → card issuance; (b) Guest/staff presents card at a door → access verification; (c) Card inserted in room → power activation. |
| **Primary actors (roles)** | Guest; Receptionist (issues/configures card); Housekeeping/maintenance staff (authorized cards). |
| **Inputs** | Reservation/guest identity, **access rights / permissions**, room & service-area assignments, key-card RFID data. |
| **Process steps** | 1) Receptionist **issues & configures key card** in HMS, assigning room + service-area access rights → 2) Guest **holds card to Door Card Reader** → 3) **RFID transmits access rights** → 4) **Door System Software verifies permissions** → 5) **Door unlocks** only if authorized → 6) inside room, **card inserted into Energy Saver Interface** activates the electrical circuit. |
| **Outputs** | Door unlock/deny decision; granted access to room + authorized facilities (incl. laundry room door); activated/deactivated room power (energy saving). |
| **Supporting app systems** | **Hotel Management System** (card configuration), **Door System Software** (verification), **Energy Management System** (card-switch circuit). |
| **Supporting tech** | **Key Card** (RFID), **Door Card Reader**, **RFID communication**, **Energy Saver Interface with card switch**; reception PC/phone/payment terminal. |
| **Exceptions** | Laundry room: door access via key card, **but** power is continuous (bypasses card-switch). |
| **Value delivered** | Secure, controlled access; seamless guest convenience; integrated energy savings; unified staff + guest access model. |

---

## 3. ArchiMate Element Inventory (for the detailed model)

> Map every narrative noun/verb to an ArchiMate concept. Use this as your element checklist.
> Layer colors (standard): **Business = yellow**, **Application = blue**, **Technology = green**, **Motivation = purple**, **Physical = green (physical equipment)**.

### 3.1 Motivation layer (for both)
- **Stakeholders:** Hotel Manager, Head of Concierge, Head of Housekeeping, Guest.
- **Drivers:** Operational efficiency; Guest experience ("invisible luxury"); Security; Energy cost.
- **Goals:** "Shared operational picture / aligned departments"; "Prepare for arrivals, not react"; "Secure & convenient room access"; "Reduce unnecessary energy consumption".
- **Assessment (As-Is pain):** Fragmented architecture, missing overall overview (the reason the EA team was hired — Timeline Phase 4).

### 3.2 Daily Briefing — by layer

**Business layer**
- *Business Roles/Actors:* Hotel Manager, Head of Concierge, Head of Housekeeping, Reception/Concierge/Housekeeping/Service staff.
- *Business Service:* **Daily Operational Briefing Service**.
- *Business Process:* **Daily Briefing Process** → sub-actions: *Generate Briefing Report* (system-triggered), *Conduct Briefing Meeting*, *Broadcast Coordination*, *Address Special Requests*, *Assign Drivers/Tasks*.
- *Business Function:* Operations Coordination / Staff Coordination.
- *Business Objects:* **Arrival Schedule**, **VIP List**, **Briefing Report**, **Special Request**, **Reservation**, **Open Costs**.
- *Business Event:* "Start of day / overnight reservations finalized".

**Application layer**
- *Application Components:* **Hotel Management System (HMS)**, **Staff Portal (Android app)**.
- *Application Services:* **Briefing Report Generation Service**, **Push Notification Service**, **Operational Dashboard Service**.
- *Application Functions:* *Generate Briefing Report*, *Aggregate Arrivals/VIP/Tasks*, *Send Push Notification*, *Render Dashboard*.
- *Data Objects:* Briefing Report (data), Arrival Schedule (data), VIP List (data), Task/Assignment (data).

**Technology layer**
- *Nodes:* **AWS Central Server (cloud)**.
- *System Software:* HMS runtime / hosting.
- *Devices:* **Android Smartphones** (staff), reception **PC**.
- *Technology/Comm. Service:* **Wi-Fi Network** (daily synchronization), Push delivery.
- *Artifacts:* Briefing Report file / data store.

### 3.3 Room Access — by layer

**Business layer**
- *Business Roles/Actors:* Guest, Receptionist, Housekeeping/Maintenance staff.
- *Business Services:* **Room Access Service**, **Key Card Issuance Service** (part of Check-in Service).
- *Business Process:* **Check-in → Issue & Configure Key Card / Assign Access Rights**; **Access Room** (present card → enter).
- *Business Objects:* **Key Card**, **Access Rights / Permissions**, Reservation/Guest profile.
- *Business Event:* Check-in; "Card presented at door".

**Application layer**
- *Application Components:* **Hotel Management System** (key-card configuration), **Door System Software**, **Energy Management System**.
- *Application Services:* **Key Card Configuration Service**, **Access Verification Service**, **Energy Activation Service**.
- *Application Functions:* *Configure Card / Assign Rights*, *Verify Access Permissions*, *Unlock Door decision*, *Activate Room Circuit*.
- *Data Objects:* Access Rights record, Card credential data, Room power state.

**Technology / Physical layer**
- *Devices (physical equipment):* **Key Card (RFID)**, **Door Card Reader**, **Energy Saver Interface w/ card switch**, reception PC + phone + **payment terminal**, in-room **telephone**.
- *System Software:* Door System Software runtime; Energy Management runtime.
- *Communication / Network:* **RFID** (card ↔ reader), **Wi-Fi network**, **private telephone network**.
- *Node:* **AWS Central Server** (stores access rights / guest data).
- *Artifact:* Access-rights data store.

---

## 4. Key relationships to draw (don't forget the arrows)

**Daily Briefing**
- Hotel Manager **assigned-to** Daily Briefing Process.
- Daily Briefing Process **realizes** Daily Operational Briefing Service.
- HMS *Briefing Report Generation* **serves** Daily Briefing Process.
- Staff Portal *Push Notification* + *Dashboard* **serve** staff roles.
- Arrival Schedule / VIP List **access** (read) by the process & by report generation.
- HMS & Staff Portal **assigned-to / deployed-on** AWS Server; **served-by** Wi-Fi Network.
- Briefing Report (Business Object) **realized-by** Briefing Report (Data Object) **realized-by** Briefing artifact.

**Room Access**
- Receptionist **assigned-to** Issue & Configure Key Card.
- Key Card Configuration Service (HMS) **serves** check-in process; writes **Access Rights**.
- Guest **triggers** Access Room; Key Card **accessed-by** Door Card Reader via **RFID** path.
- Door System Software *Access Verification* **serves** Room Access Service.
- Door Card Reader / Energy Saver Interface **assigned to / realize** the physical access & power functions.
- Energy Management System **serves** energy saving; card switch **triggers** circuit activation.
- Laundry room door **uses** same access service; laundry power **NOT** served by card-switch (note as exception).

---

## 5. Abstract (whole-enterprise) model — what to show in the 3-min intro
Keep it detail-free: a few elements per layer that span the *entire* ArchiHotel, with Focus 2 visible inside it.

- **Stakeholders/Motivation:** Guest, Hotel Manager, Concierge, Housekeeping → drivers: seamless experience, efficiency, security.
- **Business services (top):** Booking/Reservation, Check-in/Check-out, **Daily Briefing**, **Room Access**, On-Demand Services (Laundry, Transfer, Housekeeping).
- **Business processes:** Reservation & Operational Coordination → Daily Briefing → Service Delivery during stay.
- **Application components:** Web Booking System, **Hotel Management System (central)**, **Staff Portal**, **Door System Software**, WeWash, Transfer Management System, Energy Management System, Payment Service (external).
- **Technology:** **AWS cloud server**, **Wi-Fi network**, RFID key-card infrastructure, Android devices, private telephone network.
- **Highlight box** around Daily Briefing + Room Access → "our focus area".

---

## 6. Presentation script (per the assignment timing)

### General Introduction (~3 min)
- **Use case:** ArchiHotel = luxury hotel group running on "invisible luxury" — systems/people pre-aligned before the guest notices. Central **Hotel Management System on AWS** ties booking, operations, access, services together.
- **Show abstract EA model.** Walk top-down: stakeholders → business services → key apps → tech foundation.
- **Key characterizing elements:**
  - One **central HMS** as the single source of truth (avoids fragmented records).
  - **Stakeholders:** Guest, Hotel Manager, Concierge lead, Housekeeping lead, drivers, service staff.
  - **Main services:** Reservation, Check-in/out, **Daily Briefing**, **Room Access**, Laundry (WeWash), Transfer, Housekeeping.
  - **Cross-cutting tech:** AWS cloud + hotel Wi-Fi + RFID key cards.
- Transition: "Our focus zooms into the two operational backbones — *Daily Briefing* (coordination) and *Room Access* (secure physical entry) — and how the same HMS powers both."

### Team Focus Area (~7 min) — walk all three layers of the detailed model
1. **Business layer:** Daily Briefing process (Manager + leads, before 9am, review VIP/transfers/requests → broadcast) and Room Access process (receptionist issues card → guest taps reader → enters). Name the business objects (Arrival Schedule, VIP List, Briefing Report, Key Card, Access Rights).
2. **Application layer:** HMS *generates briefing report* & *configures key cards*; Staff Portal *dashboard + push*; Door System Software *verifies access*; Energy Management *activates circuit*.
3. **Technology layer:** AWS server hosts HMS; Wi-Fi syncs report to Android phones daily; RFID links key card ↔ door reader; card switch ↔ energy saver. Point out the exception (laundry continuous power).
- Use the live dashboard numbers (14 arrivals / 3 VIP / 5 transfers / 8 check-outs) as a concrete anchor.

### Reflection Questions (~2 min)
- **Business value:**
  - Shared, real-time operational picture → departments aligned, fewer errors, faster briefings.
  - Proactive prep ("preparing, not reacting") → better guest experience & VIP handling.
  - Room Access: security + convenience + automatic energy savings + unified guest/staff model.
- **Limitations of the model:**
  - Static As-Is snapshot; no quantitative flows (timing, load, failure rates).
  - Doesn't model exceptions/edge cases well (e.g., lost card, offline reader, Wi-Fi outage during sync).
  - Boundary with external systems (payment, external car platform) only lightly represented.
- **Missing info for a truly holistic model:**
  - Security/identity details (how access rights are authenticated, encryption, card lifecycle/revocation).
  - Data model & integration contracts between HMS ↔ Door System ↔ Energy Mgmt ↔ Staff Portal.
  - Non-functionals: availability, performance, what happens when AWS/Wi-Fi is down.
  - Organizational details: who administers access rights, audit/logging, GDPR for guest data.
  - Check-out flow for access revocation (mentioned for check-in but not deactivation).
- **Modeling challenges experienced:**
  - Narrative prose → discrete ArchiMate elements (deciding service vs. function vs. process granularity).
  - Same component (HMS) appears in many processes → avoiding clutter / right abstraction level.
  - Physical vs. technology layer for key card / reader / card switch.
  - Implicit relationships (energy management's link to access) had to be inferred from one paragraph.

---

## 7. Quick element-count cheat sheet (detailed Focus-2 model)
- **Business:** ~3 roles + 2 services + 2–3 processes + 5 business objects.
- **Application:** ~4 components (HMS, Staff Portal, Door System Software, Energy Mgmt) + ~6 app services/functions + data objects.
- **Technology/Physical:** AWS node, Wi-Fi, RFID, Android device, Door Card Reader, Key Card, Energy Saver Interface, reception PC/terminal.
- **Motivation (optional overlay):** 4 stakeholders, 3–4 drivers/goals, 1 assessment.

---
*Material reconstructed from the ArchiHotel "Genesis Files" site (content-data.js). All scenario content is fictional course material — © 2026 TUM Chair of Information Systems Heilbronn.*
