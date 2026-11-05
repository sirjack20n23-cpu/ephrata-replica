# INTERNATIONAL_AIRPORT_GUIDE.md — Claude build pack for city "Barney"

Who this is for: you are **Claude Code**, controlling Barney through **CitiesIIAgentBridge 0.5.0 community**. This file is the **gold-standard International Airport** masterplan: visually appealing, multi-modal, cargo-capable, and traffic-safe. Cite sources at the end. **Do not invent numbers.** If a live prefab differs from this doc, trust `get_prefab_details` / `preview_building` for that session.

Companion files: `TERRAFORMING.md` (you cannot terraform), `TRAFFIC_GUIDE.md` (hierarchy / interchanges), `CS2_CREATOR_GUIDE.md`, blueprint image `international_airport_blueprint.png`.

---

## 0. Why International Airport (not basic Airport)

| | **Airport** (small) | **International Airport** (this guide) |
|---|---|---|
| Footprint | 142 × 47 cells = **1136 × 376 m** | 170 × 158 cells = **1360 × 1264 m** |
| Build cost | ₡750,000 | **₡4,000,000** |
| Upkeep / month | ~₡37,500–100,000 (sources differ; verify in UI) | **₡150,000** (wiki table) |
| Dev points | **Air** node **4** pts | **International Airport** node **8** pts (after Air) |
| Gates / cargo | 5 pax; cargo via **Cargo Terminal** upgrade | **18 pax + 6 cargo** built-in |
| Upgrades | Airport Services, Business Lounge, Cargo Terminal | **None** — train, bus, taxi, parking, cargo included |
| Effects (city-wide) | Attractiveness + office efficiency (wiki: +5% Attr, +3% Office) | **+10% Attractiveness, +10% University interest, +10% Software demand** |
| XP | 1,000 | **5,000** |

**Choose International Airport** when Barney can afford ₡4M + ₡150k/mo, has (or will buy) ~**4×3 to 5×4 map tiles** of flat land near highway + rail, and wants tourism + office/software demand + high-volume air cargo in one campus. If money is tight, build the small Airport first (same landside ideas, smaller pad); this guide still applies.

Sources: CS2 Transportation wiki summary; Venexis footprint list; GameRant / TheGamer airport guides; Dexerto / Paradox progression (Air 4 pts, Int'l Airport 8 pts). **Confirm locked/unlocked and exact upkeep in-game** with `get_build_prefabs` filter `Airport`.

---

## 1. Bridge capabilities (ground truth)

From `TERRAFORMING.md` + bridge `COMMANDS.md` (FTPAiYT/cities2-agent-bridge-ndc):

| You CAN | You CANNOT |
|---|---|
| `sample_terrain`, `get_city_map`, `get_tiles`, `get_city_state` | **Any terraform** (no raise/level/slope commands) |
| `get_build_prefabs` / `get_prefab_details` / `preview_building` | Invent elevation tools beyond road `elevation` |
| `place_building` (if `bridgePlacementSupported` and unlocked) | Guarantee airway line tool via bridge — **ask user** if no command exists |
| `build_road` / `build_network` with optional `elevation` **−100…100** | Traffic-light / lane-connector / ticket UI (ask user) |
| `zone_rectangle`, parking lots if unlocked as buildings | Spend ₡4M or `purchase_tiles` without asking |

Coordinates are **world metres** (`x`, `z`). Cell size for footprints = **8 m × 8 m** (CS2 wiki / Steam consensus).

Road elevation UX in vanilla: step sizes **1.25 m … 10 m**. Practical clearances cited by guides: **~12.5 m** tunnel under a ground road; **~8.75 m** between stacked tunnels; regular roads roughly **−50…+50 m**. Prefer grades **≤4% roads / ≤2% rail** for realism (game allows steeper).

---

## 2. Gold-standard design (creators synthesized)

**Name:** Barney International — Service-Interchange Multimodal Hub

Inspired by:
- **OVERCHARGED EGG** (Yoakley airport): flatten a dedicated pad; highway-speed exit; one-way curb/parking loops with ~**32 m** curves; plug cargo + passenger rail; keep complex off dense neighborhoods. [OCE]
- **TRAFFIC_GUIDE / YUMBL / TotalXclipse / Real Civil Engineer**: landside is a **service interchange** (dumbbell or trumpet) feeding an arterial **Airport Boulevard** — never dump a highway into the terminal curb; merge-before-split; hierarchy. [TRAFFIC]
- **City Planner Plays / RedGingerRR**: put a **transit plaza** on the frontage so tourists transfer to bus/tram/metro/train instead of renting every car. [CPP][RG]
- **Biffa / T4rget / Timeister (cargo patterns)**: separate **one-way truck loop** into cargo; connect rail to outside network; avoid left-turn stacks. [TRAFFIC][industry guides]
- **Dev diary / wiki**: International Airport is “size of a small town”; approach/departure cones limit building heights beyond runway ends. [CO][Wiki]

### Design goals
1. **Visual:** flat sculpted pad, landscaped noise berms, boulevard frontage with hotels/offices (no housing), clean one-way curb geometry.
2. **Accessible:** highway service interchange + integrated train + bus/taxi stands + optional tram/metro across the street.
3. **Useful:** fill pax **and** cargo air lines; rail for both; cargo logistics park; city-wide Attractiveness / Uni / Software buffs.

---

## 3. Site selection & terrain survey (Barney)

### 3.1 Area budget (minimum)

| Layer | Size | Notes |
|---|---|---|
| Asset footprint | **1360 × 1264 m** | 170×158 cells |
| Flatten buffer | +**40 m** all around asset | Level tool must cover edges so lot snaps clean |
| Landside (blvd, loop, parking, transit, hotels) | ~**1360 × 550 m** on city side | Along −Z in local frame |
| Cargo logistics park | ~**320 × 700 m** on cargo side | Separate truck gate |
| Noise berms / tree belts | **80–120 m** wide rings | Especially toward any housing |
| Approach cones | Hundreds of cells beyond **both** runway ends | Do not zone high-rises in cones |
| **Recommended survey box** | **~2160 × 2200 m** | See blueprint; ≈ **4×4 map tiles** (tiles are 600×600 m) with margin |

Map tile ≈ **600 × 600 m** (75×75 cells). Asset alone spans >2 tiles in each axis — plan **multiple owned tiles** before leveling.

### 3.2 Candidate filters
Reject a site if:
- Any `sample_terrain` point in the pad has `waterDepth > 0` (or flooding risk).
- Slope across proposed pad **> ~2%** before leveling (Δh/distance×100); after Level it must be **~0%**.
- No path for a highway arterial within ~800 m, and no rail corridor within ~600 m (or willingness to build both).
- Dense residential already inside the future noise/approach footprint.
- Not enough owned tiles (`get_tiles`) — **ask** before `purchase_tiles`.

Prefer: downwind / far from housing; flat plateau or fillable terrace; highway edge + freight rail; room for berms.

### 3.3 Survey protocol (commands)

1. `save_checkpoint` (ask user / confirm).
2. `get_tiles` — list owned tiles covering candidates.
3. For each candidate center `(cx,cz)`:
   - `get_city_map` `{"x":cx,"z":cz,"radius":500}` — footprints, roads, coarse terrain.
4. Build a **100 m grid** over the survey box (≤1024 points per call; split if needed):
   ```text
   sample_terrain {"points":[{"x":...,"z":...}, ...]}
   ```
5. Compute for the **pad rectangle only**:
   - `h_min`, `h_max`, `h_mean`
   - max slope between adjacent samples
   - any water / high groundwater pollution
6. Pick the flattest eligible pad. Report candidates to the user with numbers.

### 3.4 Define H₀ (Airport Pad Elevation)

```text
H0_raw = mean(terrain heights on pad grid)
H0     = round(H0_raw to nearest 0.5 m)     # or 1.0 m if user prefers coarser Level targets
```

Constraints:
- `H0` must be **≥ 2.0 m above** nearby water surface heights sampled at pad edges (flood safety).
- Entire asset + 40 m buffer will be **Leveled to exactly H0** by the user (you cannot do this).
- Store absolute metres, e.g. `H0 = 142.5 m` (example only — use Barney’s real mean).

All other elevations in this guide are **relative to H0**. Absolute = `H0 + offset`.

---

## 4. Exact elevation tiers

Use CS2 elevation steps of **1.25 m**. Offsets below are multiples of 1.25.

| Tier | Relative | Absolute | Purpose | Grade / length rules |
|---|---|---|---|---|
| **Airport pad / runways / taxiways / cargo apron / asset** | **H0 + 0.0 m** | `H0` | Strictly flat 0% across footprint + 40 m buffer | User **Level** brush |
| **Airport Boulevard + passenger loop + parking decks at grade** | **H0 + 0.0 m** | `H0` | Landside at pad level | — |
| **Noise berm crest** | **H0 + 7.5 m** | `H0+7.5` | Earth berm + dense trees toward districts | Soften slopes; berm width ≥80 m |
| **Optional flyover / express bypass** | **H0 + 10.0 m** | `H0+10` | Bridge over boulevard / rail | Road ramp ≥ **250 m** each side for ≤**4%** (10/0.04) |
| **Optional underpass / deep bypass** | **H0 − 12.5 m** | `H0−12.5` | Under at-grade roads (guide clearance) | Road ramp ≥ **313 m** @ ≤4%; if rail in trench @ ≤2% → ≥ **625 m** |
| **Integrated train approaches** | Prefer **H0 + 0.0** into building plugs | `H0` | Match building rail sockets | If you trench nearby roads instead, bridge roads over rail at **+10.0** while rail stays at H0 (OCE pattern) |

**Do not** use non-1.25 multiples for bridge `elevation` when matching vanilla UI (e.g. prefer 10.0 not 9.0). Bridge `elevation` is an offset in metres (−100…100) on `build_road` / `build_network`.

### Ramp length cheat sheet
| Δh | ≤4% road min length | ≤2% rail min length |
|---|---|---|
| 7.5 m | 188 m | 375 m |
| 10.0 m | 250 m | 500 m |
| 12.5 m | 313 m | 625 m |

---

## 5. Local coordinate frame & layout

### 5.1 Frame
- **Origin O:** passenger terminal **curb center** (city-facing doors).
- **+X:** along runway long axis (align with asset’s long side after you confirm rotation via preview).
- **+Z:** into airside / runways (away from city).
- **−Z:** landside / city.

Blueprint: `international_airport_blueprint.png`.

After `preview_building`, if the prefab’s road edge is not at z=0, **shift O** so the snapped curb matches the preview. Do not fight the prefab — adapt the local frame.

### 5.2 Key rectangles (local metres; shift with O)

| Element | Approx (x_min, z_min) → size | Elevation |
|---|---|---|
| International Airport asset | (0, 0) → **1360 × 1264** | H0 |
| Flatten buffer | (−40, −40) → **1440 × 1344** | Level to H0 |
| Airport Boulevard | (−80, −80) → **1520 × 56** | H0; 4–6 lane arterial |
| Passenger one-way loop | (280, −280) → **800 × 180** | H0; ~32 m corner radii |
| Transit plaza | (520, −480) → **280 × 160** | H0 |
| Short-stay parking | (80, −480) → **420 × 160** | H0 |
| Long-stay / garage | (860, −480) → **420 × 160** | H0 |
| Hotel / office strip | (−200, −480) → **240 × 360** | H0; **no residential** |
| Cargo logistics park | (1420, 200) → **320 × 700** | H0; industrial / warehouse |
| Highway service interchange | center ~(680, −620) | H0; dumbbell/trumpet |
| Noise berms | 80–120 m belts around campus | Crest H0+7.5 |

### 5.3 Road / access rules
1. **Highway → service interchange → Airport Boulevard only** (hierarchy). [TRAFFIC]
2. **Passenger curb:** one-way loop; buses + taxis; no through freight. [OCE]
3. **Cargo:** separate gate from boulevard; one-way truck loop into cargo side; no left-turn stacks. [TRAFFIC]
4. **Rail:** connect integrated train platforms to the **outside** passenger and/or cargo network; unconnected rail is useless. [TRAFFIC][OCE]
5. **Transit plaza:** bus stops facing plaza; optional tram/metro station across boulevard; taxi stands at curb (asset includes stands — still wire lines). [CPP][RG]
6. Intersection spacing on boulevard: keep collector junctions sparse; signal only where needed (user may toggle lights).

### 5.4 Zoning around campus
| Zone | Where | Avoid |
|---|---|---|
| Hotels / commercial / offices | Boulevard & hotel strip | Inside berm toward runways |
| Warehouses / industry | Cargo park only | Passenger curb |
| Parks / trees | Berms & plaza edges | Blocking truck radii |
| **Residential** | **Outside noise + approach influence** | Anywhere inside survey box if possible |

---

## 6. Phased build (Claude vs user)

### Phase 0 — Unlock & money (user)
- Confirm Transportation tree: **Air (4 pts)** then **International Airport (8 pts)**.
- Confirm treasury ≥ ₡4M + road/rail/tile budget + ₡150k/mo upkeep headroom.
- Ask user to unlock if locked.

### Phase 1 — Survey (Claude)
- Run §3 protocol; propose 1–2 sites with H0 proposal and tile purchase list.
- **Stop for approval** before buying tiles or demolitions.

### Phase 2 — User Landscaping Checklist (Claude fills blanks)

Copy this to the user after Phase 1:

```text
AIRPORT PAD — USER LANDSCAPING CHECKLIST (Barney)
1) Menu: Landscaping → Terrain. save_checkpoint first (no undo).
2) Target height H0 = ________ m (absolute). Type/pick this with Level
   (right-click a flat spot at H0, or Level to a flat at-grade road at H0).
3) Brush size: start 200–400; finish edges with 80–150.
4) Strength: Level 40–70%; Soften 20–40% on berm transitions.
5) Flatten rectangle: world AABB covering asset + 40 m buffer:
   (x0,z0)=(________,________) to (x1,z1)=(________,________)
   (Claude: convert local (−40,−40)→(1400,1304) through your O/rotation.)
6) Optional berms: raise crest to H0+7.5 = ________ m along noise belts;
   Soften; plant dense trees afterward (user or Claude if tree prefabs allowed).
7) Re-check: tell Claude to sample_terrain on the pad grid — max−min height
   across pad should be ≤ ~0.5 m.
8) Do NOT dig below nearby water. Keep H0 ≥ water + 2 m.
```

### Phase 3 — Place asset (Claude preferred)
1. `get_build_prefabs` filter International Airport / Airport; confirm `locked:false`, `bridgePlacementSupported:true`.
2. `preview_building` at proposed pose; adjust rotation so runway axis and curb match frame.
3. Ask user to confirm cost (~₡4,000,000) and pose.
4. `place_building` with `maxCost` high enough; verify with `inspect_entity` / `get_buildings`.
5. If bridge placement fails: hand exact position/rotation to user to plop manually.

### Phase 4 — Landside roads & interchange (Claude)
- Build Airport Boulevard, passenger loop, cargo loop with `build_road` (correct unlocked prefabs via `get_build_prefabs` kind network).
- Build service interchange tying highway ↔ boulevard (dumbbell = two roundabouts; or trumpet). Follow TRAFFIC_GUIDE.
- Optional: flyover at elevation **+10** or underpass at **−12.5** with ramp lengths from §4.
- Ask user for signal/lane cleanup.

### Phase 5 — Rail & transit (Claude + user)
- Connect rail to integrated platforms; add cargo/passenger lines as needed.
- Place extra parking lots / bus station / tram-metro if desired (footprints: Small 5×5 … Very Large 18×10 cells; Bus Station 13×13 — Venexis).
- **User:** create **airplane lines** (pax + cargo) with Line tool: terminal ↔ outside air connections ↔ same terminal. Fill gates; cargo only on cargo gates. [GameRant][TheGamer]
- **User:** set parking fees / transit tickets if desired.

### Phase 6 — Utilities & services (Claude)
- Ensure power (extra transformer if load spikes — see CS2 utilities guide), water, sewage to campus.
- Fire station / disaster response within range of terminal + fuel-like risk areas; police optional.
- No sewage outlet uphill of intakes.

### Phase 7 — Zone & detail (Claude)
- Zone hotel/office/commercial strip; warehouse cargo park; tree berms.
- **Zero residential** inside noise/approach influence.
- Soften landscaping leftovers (user).

### Phase 8 — Validate
- `get_city_map` + traffic read; watch curb and cargo gate.
- Confirm planes spawn (user checks lines).
- One change at a time if congested.

---

## 7. Claude operating checklist (short)

1. Read this file + blueprint + TERRAFORMING.md + TRAFFIC_GUIDE.md.
2. Survey → propose H0 + tiles → **ask**.
3. Deliver filled User Landscaping Checklist → wait for Level complete → re-sample.
4. Preview → place International Airport → landside hierarchy → rail → ask user for air lines.
5. Zone non-residential buffers; utilities; services.
6. Never terraform yourself; never spend ₡4M or buy tiles silently.

---

## 8. Uncertainties (do not treat as facts)

- Exact live upkeep / XP / effect strings can differ by patch — always read the in-game tooltip / `get_prefab_details`.
- GameRant vs wiki disagree slightly on small-Airport Attractiveness (+10% vs +5%); International figures above follow the common wiki/GameRant Int'l summary.
- Prefab road/rail socket positions are **not** fully documented publicly — **preview_building** is mandatory.
- Whether `place_building` always succeeds for International Airport depends on `bridgePlacementSupported` in your build.
- Airway / line creation may be **user-only** if no bridge command lists it — check `COMMANDS.md` each session.
- Noise numbers for aircraft are not published as a single metre radius here — use distance + berms + no housing as the practical rule.

---

## 9. Sources

- Bridge: https://github.com/FTPAiYT/cities2-agent-bridge-ndc (`COMMANDS.md`, `AGENTS.md`); local `TERRAFORMING.md`
- Footprints: Venexis, Building Footprint Sizes — https://www.modscities2.com/building-footprint-sizes/ (Airport 142×47; International 170×158)
- Cell size 8 m: CS2 Maps wiki; Steam discussions
- Stats / unlocks: CS2 Transportation wiki summaries; GameRant “How to Set Up an Airport”; TheGamer airport setup; Dexerto development trees (Air 4, International Airport 8); Paradox Feature Highlight #10; Colossal Order Dev Diary #3
- Elevation / tunnels: TheGamer tunnels guide (1.25–10 m steps; ~12.5 m under roads; −50 m depth class)
- Creators: OVERCHARGED EGG “Finally, The City Gets An Airport | Yoakley” https://www.youtube.com/watch?v=DaVi3uhoUio ; TRAFFIC_GUIDE.md creators (YUMBL, CPP, RCE, TotalXclipse, Biffa, RedGingerRR, et al.)
- Blueprint: `international_airport_blueprint.png` (generated for this pack)
