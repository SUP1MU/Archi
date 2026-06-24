# ArchiHotel Focus 2 — Source Texts by Domain & Layer

**Source:** ArchiHotel — The Genesis Files
**PDF:** `ArchiHotel_ The Genesis Files.pdf` (19 pages, © 2026 TUM Chair of Information Systems Heilbronn)
**Website:** https://archihotel-eam.web.app/

> Every quote below is verbatim from the PDF or the website (identical content).
> Format: `[p.X — Section heading]` for easy location in both sources.

---

## 1. ROOM ACCESS

### 1.1 Business Layer source texts

**Primary narrative — actors, process, business objects**
> "Room access throughout the hotel is managed through an integrated Key Card system. Every guest room door is equipped with a Door Card Reader connected to the Door System Software. When a guest holds their key card near the reader, access rights stored on the card are transmitted securely via RFID technology. The system verifies whether the guest has valid access permissions and unlocks the door only if authorization is confirmed. In this way, the hotel maintains controlled and secure room access while keeping the process simple and convenient for guests."

`[p.12 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`[Website → section: physicalDigital / Room Access]`

---

**Check-in → key card issuance (Receptionist actor + Check-in process)**
> "Once a guest checks in, the receptionist can immediately issue and configure a key card through the HMS; access rights for rooms and service areas are assigned digitally, giving guests seamless access to their accommodation and authorized facilities."

`[p.4 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`[Website → section: preArrival / staffDashboard]`

---

**Staff access (Housekeeping Staff actor)**
> "Hotel staff members, including housekeeping personnel, also use authorized key cards so they can activate room electricity while performing cleaning or maintenance tasks."

`[p.12 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`[Website → section: physicalDigital / Room Access]`

---

**Energy Saver link (business object: room power state)**
> "The same key card infrastructure is also connected to the hotel's energy management system. Inside each guest room, an Energy Saver Interface with a card switch activates the room's electrical circuit only when a valid key card is inserted. This helps reduce unnecessary energy consumption when guests are away from their rooms."

`[p.12 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`[Website → section: physicalDigital / Room Access]`

---

**Laundry room exception (business process: access laundry)**
> "Access to the laundry room is granted via the key card door system software that verifies each key card hold onto the door card reader."
> "The laundry room operates separately from this mechanism because it requires continuous power availability for the washing equipment."

`[p.10 — SPECIAL REQUESTS AND SERVICES / "Smart Wash Ecosystem"]`
`[p.12 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`[Website → section: physicalDigital / laundryCard]`

---

**Phase 3 timeline — business context**
> "Physical access became part of the same guest experience rather than a separate layer. Key Cards, general room access control and the Energy Saver option via the installed switch started working as one coordinated system. The guest saw convenience, while the hotel gained cleaner control over when and where access should exist."

`[p.18 — EVOLUTION OF EFFICIENCY / "Phase 3 — Hardware Meets Software"]`
`[Website → section: timeline / phase 3]`

---

**Phase 1 timeline — HMS and Receptionist Interface**
> "Reception could handle arrivals, payments, and room access from a single workflow through the Hotel Management System and Receptionist Interface, which made check-in faster and reduced avoidable errors."

`[p.17 — EVOLUTION OF EFFICIENCY / "Phase 1 — Hotel Management System Unification"]`
`[Website → section: timeline / phase 1]`

---

### 1.2 Application Layer source texts

**Door System Software — component + verification function**
> "Every guest room door is equipped with a Door Card Reader connected to the Door System Software. When a guest holds their key card near the reader, access rights stored on the card are transmitted securely via RFID technology. The system verifies whether the guest has valid access permissions and unlocks the door only if authorization is confirmed."

`[p.12 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`→ Maps to: Door System Software (ApplicationComponent), Validate Access Rights (ApplicationFunction), Access Verification Service (ApplicationService)`

---

**Hotel Management System — key card configuration**
> "Once a guest checks in, the receptionist can immediately issue and configure a key card through the HMS; access rights for rooms and service areas are assigned digitally."

`[p.4 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`→ Maps to: Hotel Management System (ApplicationComponent), Key Card Configuration Service (ApplicationService)`

---

**Energy Management System — energy activation**
> "The same key card infrastructure is also connected to the hotel's energy management system. Inside each guest room, an Energy Saver Interface with a card switch activates the room's electrical circuit only when a valid key card is inserted."

`[p.12 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`→ Maps to: Energy Management System (ApplicationComponent), Energy Activation Service (ApplicationService)`

---

**Access Rights Record — data object**
> "access rights stored on the card are transmitted securely via RFID technology. The system verifies whether the guest has valid access permissions"

`[p.12 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`→ Maps to: Access Rights Record (DataObject) — the stored permission data read during verification`

---

**Laundry room — same Door System Software**
> "Access to the laundry room is granted via the key card door system software that verifies each key card hold onto the door card reader."

`[p.10 — SPECIAL REQUESTS AND SERVICES / "Smart Wash Ecosystem"]`
`→ Same Door System Software / Access Verification Service used for laundry room door`

---

### 1.3 Technology Layer source texts

**Door Card Reader — device**
> "Every guest room door is equipped with a Door Card Reader connected to the Door System Software."

`[p.12 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`→ Maps to: Door Card Reader (Device)`

---

**RFID — communication network**
> "access rights stored on the card are transmitted securely via RFID technology"

`[p.12 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`→ Maps to: RFID Communication (CommunicationNetwork)`

---

**Energy Saver Interface — device**
> "Inside each guest room, an Energy Saver Interface with a card switch activates the room's electrical circuit only when a valid key card is inserted."

`[p.12 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`→ Maps to: Energy Saver Interface (Device)`

---

**AWS Cloud Server — node hosting HMS**
> "At the center of the hotel's operations is a cloud-based infrastructure hosted on Amazon Web Services (AWS). The central server runs the Hotel Management System, the hotel website, and the Transfer Management System, while also storing operational data such as reservations, guest profiles, VIP lists, pricing information, room availability, transfer schedules, driver and vehicle information, and the open costs associated with each guest stay."

`[p.11 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`→ Maps to: AWS Cloud Server (Node) → realizes Hotel Management System`

---

**Reception PC & Terminal — device**
> "At our reception point, staff members work with a setup that combines a personal computer, phone, and payment terminal. Through the hotel's Wi-Fi infrastructure, the reception system stays synchronized with reservation data, guest profiles, open costs, room status, and special requests in real time."

`[p.6 — LIVE OPERATIONS / "Internal Memo — Emma Rousseau, Hotel Manager"]`
`→ Maps to: Reception PC & Terminal (Device)`

---

**Wi-Fi Network — communication network**
> "Receptionists, concierge staff, housekeeping teams, and managers are connected to the system through the hotel's Wi-Fi network, allowing information to be synchronized continuously across devices and workstations."

`[p.11 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`→ Maps to: Hotel Wi-Fi Network (CommunicationNetwork)`

---

---

## 2. DAILY BRIEFING

### 2.1 Business Layer source texts

**Core briefing process — trigger, actors, process steps**
> "Approved reservations are automatically added to the hotel's Arrival Schedule, which forms the basis for the daily operational briefing process. Every morning, the Hotel Management System generates a briefing report containing the day's arrivals, guest information, VIP guests, special requests, and operational updates. This report gives departments a shared and up-to-date overview of the day ahead."

`[p.5 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`→ Maps to: Generate Briefing Report (BusinessProcess), Arrival Schedule (BusinessObject), Briefing Report (BusinessObject)`

---

**Daily briefing meeting — Hotel Manager + heads of concierge & housekeeping**
> "The daily briefing meeting is led by the hotel manager together with the heads of concierge and housekeeping. During this short operational meeting, they review VIP arrivals, coordinate cross-department activities, discuss special guest requirements, and align staffing priorities for the day. The goal is to ensure that all departments work from the same operational picture rather than from isolated fragments of information."

`[p.5 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`→ Maps to: Conduct Briefing Meeting (BusinessProcess), Hotel Manager + Head of Concierge + Head of Housekeeping (BusinessActors)`

---

**Broadcast coordination — Staff Portal → staff phones**
> "If additional coordination is required, staff members are informed through the Staff Portal application installed on their Android smartphones. The application distributes push notifications for urgent requests, special instructions, or operational changes communicated during the briefing meeting. It also provides a dashboard that combines schedules, tasks, arrivals, and briefing information into a single operational overview accessible throughout the hotel."

`[p.5 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`→ Maps to: Broadcast Coordination (BusinessProcess), Hotel Staff (BusinessActor), Daily Operational Briefing Service (BusinessService)`

---

**Stakeholder voice — Ellen M., Hotel Manager (before 9am trigger)**
> "Before 9:00 AM, the concierge leads and housekeeping leads join me for a short operational briefing. We look at the day's arrivals, VIP guests, transfers, and any requests that need extra care, all based on what came in overnight."
> "The difference now is that we are not spending the first part of the meeting gathering information. The arrival schedule, VIP List, and briefing report are already there, so we can focus on what needs attention, what needs escalation, and where departments need to stay closely aligned."
> "That meeting works because everyone is looking at the same live picture. Once priorities are agreed, the update goes out to staff phones through the Staff Portal, and the dashboard keeps the whole team aligned."

`[p.14 — STAKEHOLDERS / "Voices from the Hotel" — Ellen M., Hotel Manager]`
`[Website → section: stakeholders / voices]`

---

**Business objects — Arrival Schedule, VIP List, Briefing Report, Special Request**
> "The arrival schedule, VIP List, and briefing report are already there, so we can focus on what needs attention, what needs escalation, and where departments need to stay closely aligned."

`[p.14 — STAKEHOLDERS / Ellen M., Hotel Manager]`

> "the day's arrivals, guest information, VIP guests, special requests, and operational updates"

`[p.5 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`→ Maps to: Arrival Schedule, VIP List, Briefing Report, Special Request (BusinessObjects)`

---

**Live dashboard data — Staff Portal morning briefing 25 Mar 2026**
> KPIs: 14 Arrivals · 3 VIP · 5 Transfers · 8 Check-Outs
> Cross-departmental tasks: ✓ Generate Briefing Report reviewed · ✓ Broadcast Coordination sent to all staff · ○ Address Special Requests (3 open) · ○ Assign drivers for Transfer Bookings

`[p.7 — LIVE OPERATIONS / "Staff Portal Dashboard — Display Briefing Report"]`
`[Website → section: staffDashboard / live dashboard]`

---

**Internal memo — Emma Rousseau, Hotel Manager**
> "Over the last few years, one of the biggest improvements in our hotel operations has been the direct integration between guest-facing teams and the Hotel Management System. Reception and concierge no longer work with fragmented or outdated information. Instead, they are continuously connected to live reservation and operational data through the hotel network."

`[p.6 — LIVE OPERATIONS / "Internal Memo — Emma Rousseau, Hotel Manager"]`
`[Website → section: staffDashboard / memo]`

---

### 2.2 Application Layer source texts

**Hotel Management System — briefing report generation**
> "Every morning, the Hotel Management System generates a briefing report containing the day's arrivals, guest information, VIP guests, special requests, and operational updates."

`[p.5 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`→ Maps to: Hotel Management System (ApplicationComponent), Briefing Report Generation Service (ApplicationService), Compile Briefing Data (ApplicationFunction)`

---

**Staff Portal — push notifications + dashboard**
> "staff members are informed through the Staff Portal application installed on their Android smartphones. The application distributes push notifications for urgent requests, special instructions, or operational changes communicated during the briefing meeting. It also provides a dashboard that combines schedules, tasks, arrivals, and briefing information into a single operational overview accessible throughout the hotel."

`[p.5 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`→ Maps to: Staff Portal (ApplicationComponent), Push Notification Service (ApplicationService), Operational Dashboard Service (ApplicationService), Send Push Notification (ApplicationFunction)`

---

**Staff Portal — report synchronization (data flow)**
> "All staff members can access the latest briefing report directly through the application. The report is synchronized daily through the hotel's Wi-Fi network to ensure that operational information remains current on every device."

`[p.5 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`→ Maps to: Briefing Report Data (DataObject), Arrival Schedule Data (DataObject) — data flowing from HMS to Staff Portal via Wi-Fi`

---

**Staff Portal dashboard — described as operational reality**
> "This is where the morning plan becomes operational reality. In the Staff Portal, staff can see arrivals, VIP guests, housekeeping priorities, transfers, and the latest coordination notes as the day unfolds."

`[p.6 — LIVE OPERATIONS / "Staff Portal Dashboard"]`
`→ Maps to: Operational Dashboard Service (ApplicationService)`

---

### 2.3 Technology Layer source texts

**AWS Cloud Server — node hosting HMS and Staff Portal**
> "At the center of the hotel's operations is a cloud-based infrastructure hosted on Amazon Web Services (AWS). The central server runs the Hotel Management System, the hotel website, and the Transfer Management System, while also storing operational data such as reservations, guest profiles, VIP lists, pricing information, room availability, transfer schedules, driver and vehicle information, and the open costs associated with each guest stay."

`[p.11 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`→ Maps to: AWS Cloud Server (Node) → realizes Hotel Management System, Staff Portal`

---

**Hotel Wi-Fi Network — daily sync to Android devices**
> "The report is synchronized daily through the hotel's Wi-Fi network to ensure that operational information remains current on every device. This allows employees across reception, concierge, housekeeping, and service teams to stay aligned throughout the day, even while moving between guest areas, floors, and service zones."

`[p.5 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`→ Maps to: Hotel Wi-Fi Network (CommunicationNetwork), Wi-Fi Sync Service (TechnologyService)`

---

**Android Smartphones — staff devices**
> "staff members are informed through the Staff Portal application installed on their Android smartphones"

`[p.5 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`→ Maps to: Android Smartphone (Device) → realizes Staff Portal`

---

**Wi-Fi infrastructure — connecting all staff roles**
> "Receptionists, concierge staff, housekeeping teams, and managers are connected to the system through the hotel's Wi-Fi network, allowing information to be synchronized continuously across devices and workstations."

`[p.11 — THE FOUNDATION LAYER / "The Foundation Layer"]`
`→ Maps to: Hotel Wi-Fi Network (CommunicationNetwork)`

---

**Briefing Report File — artifact stored/synced**
> "All staff members can access the latest briefing report directly through the application. The report is synchronized daily through the hotel's Wi-Fi network."

`[p.5 — PRE-ARRIVAL / "Reservation and Operational Coordination Process"]`
`→ Maps to: Briefing Report File (Artifact)`

---

## Quick location guide

| PDF page | Website section heading | Content |
|---|---|---|
| p.4 | PRE-ARRIVAL | Check-in, key card issuance via HMS |
| p.5 | PRE-ARRIVAL | Full daily briefing process, Staff Portal, Wi-Fi sync |
| p.6 | LIVE OPERATIONS | Emma Rousseau memo, reception setup |
| p.7 | LIVE OPERATIONS | Staff Portal live dashboard (14 arrivals, 3 VIP…) |
| p.10 | SPECIAL REQUESTS | Smart Wash / laundry room — key card door access |
| p.11 | THE FOUNDATION LAYER | AWS, Wi-Fi, telephone network |
| p.12 | THE FOUNDATION LAYER | Room access full paragraph, Energy Saver, laundry exception |
| p.14 | STAKEHOLDERS | Ellen M. (Hotel Manager) — before 9am briefing quote |
| p.17 | EVOLUTION / Phase 1 | HMS unification, Receptionist Interface |
| p.18 | EVOLUTION / Phase 3 | Key Cards + Energy Saver as one coordinated system |

---

*Source: ArchiHotel — The Genesis Files · © 2026 TUM Chair of Information Systems Heilbronn · https://archihotel-eam.web.app/*
