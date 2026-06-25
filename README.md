# ArchiHotel — Enterprise Architecture Model (Focus 2)

ArchiMate 3.2 model for the **C2 ArchiHotel — "Genesis Files"** case
(EAM, TUM Chair of Information Systems Heilbronn).

**Focus 2: Room Access & Daily Briefing** — modeled across the Business,
Application, and Technology layers, plus a whole-enterprise overview.

Case source: <https://archihotel-eam.web.app/>

---

## Repository contents

| File | Description |
|------|-------------|
| [`ArchiHotel.archimate`](ArchiHotel.archimate) | The Archi model (open in [Archi](https://www.archimatetool.com/) 5.7+). Contains all 4 views below. |
| [`Focus2_RoomAccess_DailyBriefing.md`](Focus2_RoomAccess_DailyBriefing.md) | Case-material extraction, roles/inputs/outputs breakdown, ArchiMate element inventory, and the presentation script. |
| [`SourceTexts_Focus2.md`](SourceTexts_Focus2.md) | Verbatim source quotes by domain & layer, with PDF page / website locators. |
| [`RoomAccess_ApplicationLayer.md`](RoomAccess_ApplicationLayer.md) | Detailed Room Access application layer: every element + connector, source-grounding audit, and cross-layer/cross-domain merge instructions. |
| [`images/`](images/) | PNG exports of each view. |

## Views

### 1. Abstract EA — Enterprise Overview
High-level view of the whole hotel (all six domains, three layers) for the introduction.

![Abstract EA](images/Abstract_EA_ArchiHotel.png)

### 2. Room Access — Layered (detailed)
Business → Application → Technology, with the key-card access flow:
present card → verify access rights → **decision** → enter room / access denied.

![Room Access](images/RoomAccess_Layered_TUM.png)

### 3. Daily Briefing — Layered (detailed)
Business → Application → Technology, with the morning flow:
generate briefing report → conduct meeting → broadcast coordination to staff phones.

![Daily Briefing](images/DailyBriefing_Layered_TUM.png)

### 4. Focus 2 — flat (backup)
Original single-canvas view of both sub-domains, kept for reference.

![Focus 2 flat](images/Focus2_flat_backup.png)

### 5. Room Access — Application Layer (Detailed)
Deep-dive into just the Room Access application layer (Door System Software, Hotel Management System, Energy Management System), each decomposed into functions, junction branches, outcome events and data objects. Includes coloured **merge boxes** showing how it connects up to the Business layer, down to the Technology layer, and across to the Daily Briefing application layer (shared HMS). See [`RoomAccess_ApplicationLayer.md`](RoomAccess_ApplicationLayer.md) for the full element + connector catalogue.

![Room Access Application Layer Detailed](images/RoomAccess_ApplicationLayer_Detailed.png)

---

## Modeling conventions

- **Layer alignment** — Business (yellow) / Application (blue) / Technology (green), connected top-to-bottom.
- **Direct actor → process** assignment (no intermediate role), per the case narrative.
- **Nesting** — process steps inside their parent process; functions inside components; devices inside the node.
- **Behavioural flow** — triggering relationships with a junction for the allow/deny branch; a business event for "Access Denied".
- **Relationship types** follow ArchiMate 3.2: application service *serves* business process; function *realizes* service; technology *realizes* application; device *assigned to* function; node *composed of* devices.

## How to open

1. Install [Archi](https://www.archimatetool.com/) (5.7 or newer).
2. **File → Open** → `ArchiHotel.archimate`.
3. Expand **Views** in the model tree and double-click any view.
