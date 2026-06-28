# CONTEXT — ArchiHotel Focus 2 ArchiMate project (handoff for next conversation)

> Read this first. It captures the project, environment, tooling quirks, current model state,
> conventions/decisions, and open items so a new chat can continue without re-discovery.

---

## 1. The project
- **Course:** EAM — TUM Chair of Information Systems Heilbronn (fictional case "ArchiHotel — The Genesis Files").
- **Goal:** ArchiMate 3.2 models for **Focus 2 = Room Access + Daily Briefing**, across Business / Application / Technology.
- **Team:** 6 people, split by layer×topic (each owns one of the 6 layer-topics). The user primarily owns **Room Access · Application layer**, but we have built much more (full integrated model).
- **Deliverable = the diagram(s)** (and the model file). Markdown docs are working aids / not graded, but kept in the repo.

## 2. Sources (authoritative — NOTHING external is allowed)
- `ArchiHotel_ The Genesis Files.pdf` (19 pages) — in repo. **This + the website are the only content sources.**
- Website https://archihotel-eam.web.app/ (identical to the PDF; raw text is in `content-data.js`).
- Teammate screenshots the user shared (not in repo): a **business-layer draft** (used only for merge-box names) and two **CampusCard/CampusIT** example app-layer diagrams (used only to learn the TUM house style). PDFs `CampusIT-detailed.pdf`, `campusIT-Abstract.pdf`, `business layer- room access.pdf` are in the repo as references.
- **Rule the user insists on:** every element that is **verbatim** in the source (within Focus-2 scope) must be in the model; derived "extras" are optional.

Key source pages: p.4 check-in/key-card via HMS; p.5 daily briefing; p.6 reception PC/phone/payment terminal + Emma memo; p.7 Staff Portal live dashboard; p.10 laundry-room access via key card; p.11 AWS foundation; p.12 **Room Access full paragraph + energy saver + laundry exception**; p.14 Ellen M. (manager) quote; p.17 "HMS and Receptionist Interface"; p.18 Phase 3 key cards.
See `SourceTexts_Focus2.md` for verbatim quotes mapped to elements with page numbers.

## 3. Environment & tooling
- **Archi 5.9.0** at `~/Archi/`. **coArchi** plugin installed → the model was migrated to a grafico repo at `~/Documents/Archi/model-repository/eam/` (folder of XML). So `~/Archi/project.archimate` is **STALE** (June 19); don't trust it.
- **MCP server** (fanievh archi-mcp-server) runs inside Archi at `http://127.0.0.1:18090/mcp`.
- **IMPORTANT — MCP tools drop out of Claude's toolset after a `/model` switch.** When that happens, drive the server **directly over HTTP via curl** (Streamable HTTP):
  1. `initialize` → read `Mcp-Session-Id` response header
  2. send `notifications/initialized` with that header
  3. `tools/call` with the header
  Responses are SSE: parse lines starting with `data: `. Tool result JSON is in `result.content[0].text` (parse again).
  Reusable Python helper pattern (session id saved to `/tmp/mcp_sid.txt`):
  ```python
  def rpc(name,args):
      body={"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":name,"arguments":args}}
      p=subprocess.run(["curl","-s","-X","POST","http://127.0.0.1:18090/mcp",
        "-H","Content-Type: application/json","-H","Accept: application/json, text/event-stream",
        "-H",f"Mcp-Session-Id: {SID}","-d",json.dumps(body)],capture_output=True,text=True)
      for line in p.stdout.splitlines():
          if line.startswith("data: "):
              d=json.loads(line[6:])
              return d.get("error") or json.loads(d["result"]["content"][0]["text"])
  ```
- **⚠️ ACTIVE-MODEL GOTCHA (critical):** the MCP operates on **whatever model is ACTIVE/selected in Archi**. If the user opens another model (e.g. a teammate's reference like "Manju"), the MCP silently points at *that* model — you'll create elements in the wrong place. **ALWAYS run `get-model-info` first and confirm it reads `ArchiHotel Enterprise Architecture` with the expected element/view counts before any write.** (If it says `(new model)` / 0 elements / a different name, the wrong model is active — ask the user to click ArchiHotel's root node in the Models tree. Closing the other model may leave a blank model active; re-init the session and re-check.)
- **Saving:** there is **no MCP save tool**. Edits live in Archi memory until the user presses **Ctrl+S** (writes to the coArchi grafico repo now) OR **File → Export → Model to Open Exchange File (XML)**.
- **/tmp helper files** (`M.json` id-map, `vo_all.json` view-object ids, `intg_view.json`, etc.) are **ephemeral** — a new session must re-inventory via `search-elements` + `get-views`.

## 4. GitHub repo
- Remote: **https://github.com/SUP1MU/Archi** (branch `main`). Local repo = `/home/sup1mu/tum/EAM`.
- **User preference: NO `Co-Authored-By` trailer** on commits. User sometimes commits/pushes themselves; sometimes asks Claude to.
- Teammates push via GitHub web ("Add files via upload") → remote often advances. If push is rejected: `git fetch` then `git merge origin/main -X ours --no-edit --allow-unrelated-histories` (keep our newer versions, absorb their new files), then push.
- **VPN gotcha:** user is on a VPN (`tun0`, DNS `10.53.53.53`) that blocks github.com; proxy at `127.0.0.1:3128`. Terminal git works (uses `http_proxy`). coArchi needed proxy lines added to `~/Archi/Archi.ini` (done). If coArchi can't reach GitHub, disconnect VPN or use the proxy.
- **Model file in repo = `EAMlatest.xml`** (Open Exchange). It is the single source of truth but may **lag the in-memory model** — re-export to refresh. (Old `ArchiHotel.archimate` + duplicate XMLs were removed.)

## 5. Current model state (in Archi memory; **124 elements, 194 relationships, 7 views**)
**7 views:**
1. **Abstract EA — ArchiHotel Enterprise Overview** — whole enterprise, light detail, colored bands.
2. **Focus 2 — Room Access & Daily Briefing** — original flat backup.
3. **Room Access — Layered (TUM style)** — 3 layers, nested.
4. **Daily Briefing — Layered (TUM style)** — 3 layers, nested.
5. **Room Access — Application Layer (Detailed)** — standalone app layer; has 3 **merge boxes** (⬆ Business, ⬇ Technology, ↔ Daily Briefing/shared HMS). Documented in `RoomAccess_ApplicationLayer.md`. **CLEANED (latest):** removed `Authenticate Card`, `Maintain Lock`, `Circuit Activated` + ~9 redundant relationships; crossings 92→36. See §7 for the pattern. (These deletions are model-global, so view 6 lost them too.)
6. **Focus 2 — Integrated Detailed Model (Room Access + Daily Briefing)** — the big one: 8 groups = 6 layer-topic boxes (3×2) + **SHARED PLATFORM (HMS)** + **SHARED INFRASTRUCTURE (AWS)**. All 6 layers detailed. Very dense (~600 crossings) — readable only zoomed in. PNG: `images/Focus2_Integrated_Detailed.png`. (Re-verified: its Room Access app portion is semantically identical to view 5 — same elements/relationships, composition shown by nesting.)
7. **Room Access — Application Layer (Abstract)** — NEW. Manju-style capability map: **Component ●→ Function ─△▷ Service(s)** for the 3 components, + interfaces. **No** steps/junctions/events/data. Uses **3 new `ApplicationFunction` capability elements** (`Access Verification`, `Key Card Configuration`, `Energy Activation`) — these are *parallel* to view 5's 3 `ApplicationProcess` containers (the "Mirror Manju" choice). Layout excellent, 4 crossings. PNG: `images/RoomAccess_ApplicationLayer_Abstract.png`.

Detail style: components decomposed into **Application/Business Processes** containing **Functions** (chevron), **Junctions** for "only if" branches, **Events** for outcomes, **Data Objects** (read/write Access), **Interfaces** (Card Reader, Receptionist, Mobile App). Tech layer has Nodes/Devices/SystemSoftware/Networks/Artifacts.

## 6. ArchiMate rules learned (validator-enforced — don't repeat these mistakes)
- **ApplicationService → BusinessProcess = `Serving`** (Realization is INVALID).
- **Process → Event = `Triggering`** (Composition/Realization INVALID). Nesting an event in a process gives a benign **"Visual Nesting" info advisory** — harmless, can't be fixed with a relationship.
- **Node / SystemSoftware / Device → ApplicationComponent = `Realization`**.
- **Device → CommunicationNetwork** realization INVALID → use `Association`.
- **Composition shown only by nesting → "unused relation" warning.** Fix by drawing an explicit line: `add-connection-to-view` with the relationshipId + the two viewObjectIds.
- Junction element type string = **`Junction`** (not OrJunction/AndJunction).
- **`update-element` uses param `id`**; `delete-element`/`get-relationships` use `elementId`.
- `add-to-view` → id at `result.viewObject.viewObjectId`; `add-group-to-view` → `result.viewObjectId`.
- `bulk-mutate` is atomic (one bad op fails the batch). Create relationships individually with logging when types are uncertain.

## 7. Conventions & decisions (agreed with user)
- **Direct actor → process Assignment, NO role bridge** (both TUM exemplars confirm). The abandoned "Card Holder" role was deleted.
- **TUM house style:** layer alignment (yellow/blue/green), nesting, realization spine, triggering flow + junctions, business/app events, app/tech functions (chevron).
- **Grounding tags** in docs: `[NAMED]` (verbatim in PDF), `[DERIVED]` (modeled from described behaviour), `[INFERRED+]` (weakest). **As of latest cleanup there are NO `[INFERRED+]` elements left** — `Authenticate Card` (the only one) was deleted.
- **App-layer cleanup pattern (applied to Room Access app layer, latest):** every component is consistently **`component ●→ process` (one Assignment) → `process ─△▷ services` (Realization) → `process ◆ functions` (Composition) → `functions ┄▶ data` (Access)**. Removed: redundant component→service / component→function assignments, redundant function→service realization, and semantically-wrong service→data accesses (services don't read data, functions do). **Deny = default** (no "Maintain Lock" action; `Authorization Denied` is a terminal event). Energy junction branches directly to `Activate`/`Deactivate Room Circuit` (no redundant `Circuit Activated` event). User confirmed: no source-grounded meaning lost — the drop from 92→36 crossings was duplicate/incorrect lines, and the 3 removed elements were un-sourced or still represented by neighbours.
- **Merge model (6-person):** add colored note-boxes saying which of our elements connects to which teammate element + the relationship rule. **Room Access ↔ Daily Briefing link = shared `Hotel Management System` (component identity, NOT a service call)**; shared `AWS Cloud Server` at tech.
- Business-layer names from teammate (for merge boxes): `Request Room Access → Verify Access Authorization → Grant Room Access`; `Issue Key Card → Configure Access Rights → Encode Key Card`; objects `Access Rights Profile`, `Key Card`; event `Room Access Requested`.
- **Out-of-scope verbatim items deliberately excluded** (other domains/general infra): guest-room telephone + private telephone network, hotel website, Transfer Management System, WeWash/laundry machines (only laundry-room *access* is in scope), open costs/pricing/room availability.

## 8. Repo file inventory (on `main`)
- `EAMlatest.xml` — model (Open Exchange). **May lag in-memory; re-export to refresh.**
- `README.md`, `LICENSE`
- `Focus2_RoomAccess_DailyBriefing.md` — case material, roles/inputs/outputs, presentation script (3+7+2 min)
- `SourceTexts_Focus2.md` — verbatim quotes by topic×layer with PDF page / website locators
- `RoomAccess_ApplicationLayer.md` — detailed app-layer element+connector catalogue, source-grounding audit (§4, now "no [INFERRED+] left"), and **§6 "Modeling Story"** (sentence→box→arrow walkthrough). **Fully reconciled to the cleaned model** (matches the deletions in §5/§7).
- `context.md` — this file
- `ArchiHotel_ The Genesis Files.pdf`, `CampusIT-detailed.pdf`, `campusIT-Abstract.pdf`, `business layer- room access.pdf`
- `images/`: `Abstract_EA_ArchiHotel.png`, `RoomAccess_Layered_TUM.png`, `DailyBriefing_Layered_TUM.png`, `Focus2_flat_backup.png`, `RoomAccess_ApplicationLayer_Detailed.png`, **`RoomAccess_ApplicationLayer_Abstract.png` (NEW)**, `Focus2_Integrated_Detailed.png`
- **Reference (not in repo):** teammate "Manju" model — a separate `.archimate` with `Manju - Abstract Model` + `Manju - Detailed Model` **business-layer** views. Style learned: abstract = capability map `Component/Role → Service ← Function ◆ key Processes` (+ objects/events); detailed = same Functions/Services but more process steps. It was the template for view 7. (User has since closed it.)

## 9. Open items / next steps
- [ ] **Re-export Open Exchange XML → overwrite `EAMlatest.xml`** to capture ALL latest in-memory work (integrated view, extra tech, verbatim additions, the app-layer cleanup deletions, **and the new abstract view + its 3 capability functions**), then commit/push. **`EAMlatest.xml` is now MANY edits stale.**
- [ ] **Push doc + image updates** (`RoomAccess_ApplicationLayer.md`, `context.md`, `images/RoomAccess_ApplicationLayer_Abstract.png`, refreshed `Focus2_Integrated_Detailed.png` + `RoomAccess_ApplicationLayer_Detailed.png`) — not yet pushed at time of writing.
- [ ] Optional unify: the abstract view (#7) uses 3 `ApplicationFunction` capabilities that are **parallel** to view 5's 3 `ApplicationProcess` containers (the "Mirror Manju" choice). If a grader wants one consistent convention, either rename/merge or accept both (abstract=Function, detailed=Process).
- [x] ~~Decide on `Authenticate Card`~~ — **DONE: deleted** (along with `Maintain Lock`, `Circuit Activated`) in the app-layer cleanup.
- [ ] Offered but not done: **generate the 2-view split** (Room Access 3-layer / Daily Briefing 3-layer) for a more readable submission than the dense integrated view.
- [ ] Integrated view has **2 cosmetic element overlaps** + high crossing count (inherent to single-canvas choice).
- [ ] Benign **"Visual Nesting" advisories** on remaining nested events (Authorization Confirmed/Denied) — expected, leave or un-nest. (`Circuit Activated` advisory is gone — element deleted.)
- [ ] Optional: if a presentation wants explicit outcome boxes back (a visible "stays locked" / "circuit on"), they can be re-added as `[DERIVED]` extras — user currently prefers the cleaner version.

## 10. How to resume in a new chat (quick start)
1. Confirm Archi is open with the model + MCP server running (`curl http://127.0.0.1:18090/mcp` → 200).
2. Re-init the MCP session (curl handshake above) and **re-inventory**: `get-views`, `search-elements` (fields=standard) → rebuild the name→id map.
3. The grounded sources are the PDF + `SourceTexts_Focus2.md`. The modeling rationale is in `RoomAccess_ApplicationLayer.md`.
4. For any model edit: make it in-memory via MCP, then remind the user to **Ctrl+S / export XML** before pushing.

---
*ArchiHotel — The Genesis Files · © 2026 TUM Chair of Information Systems Heilbronn.*
