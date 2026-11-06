# SUBWAY_GUIDE.md — Metro systems for Claude (Cities: Skylines II, city "Barney")

Who this is for: you are **Claude Code**, controlling Barney through **CitiesIIAgentBridge 0.5.0 community**. This guide covers **how subway trains and lines work**, **how each station type connects**, and **how to connect multiple lines without killing throughput**. Cite sources at the end. **Do not invent numbers.** Prefer live `get_prefab_details` / in-game tooltips when they disagree with older guides.

Companions: `cs2_guides/04_transit.md`, `TRAFFIC_GUIDE.md`, `INTERNATIONAL_AIRPORT_GUIDE.md`, `TERRAFORMING.md`, blueprint `subway_network_blueprint.png`.

---

## 0. Bridge vs user (ground truth)

| Claude CAN (if unlocked + `bridgePlacementSupported`) | Claude must ASK the USER |
|---|---|
| `get_build_prefabs` / `get_prefab_details` / `preview_building` for Subway Yard, stations, track prefabs | Unlock **Subway** in Transportation tree (and prerequisites) |
| `place_building` for yard + stations | **Subway Line Tool** — create routes, stops, loops, colors |
| `build_network` for subway tracks with `elevation` (−100…100) | Vehicle count, schedule, ticket fare, line name |
| `trace_network` to verify physical track connectivity yard↔stations | **Underground View** (PageDown / Underground Mode) to inspect pipes/tunnels |
| Survey land with `sample_terrain`, `get_city_map`, `get_tiles` | Demolish underground with underground-demo toggle |
| Place surface feeders (bus stops as buildings if available; roads) | Traffic/Anarchy mods; modular multi-platform station mods |

There is **no** documented bridge command to create transit *lines* (only networks/buildings). After tracks exist, hand the user a stop-by-stop line script.

---

## 1. How subway trains & lines work

### 1.1 Unlock
- Transportation unlocks around milestone **Grand Village**.
- Development tree (common guides): **Train 1 DP → Tram 2 DP → Subway 4 DP**. Confirm in Progression UI (some summaries list Subway as 4 DP on its branch). [GameRant][TheGamer][Dexerto]
- Subway is **local** high-capacity transit (not intercity). [Paradox Feature Highlight #3]

### 1.2 Subway Yard (required)
- Trains **spawn and maintain from a Subway Yard** (or Terminal Subway Yard). Every revenue line’s track network must be able to reach a yard so trains can deadhead on/off the line. [GameRant][Paradox][Wiki]
- Yard is a **surface** building; leads into it may go underground immediately after. [GameRant]
- **Subway tracks ≠ train tracks.** Yards and stations are incompatible across modes; put subway + train stations close for pedestrian transfer only. [GameRant]

| Asset (vanilla wiki table) | Cost | Upkeep/mo | Notes |
|---|---|---|---|
| **Subway Yard** | ₡200,000 | ₡18,000 | **10 vehicles**; XP 500 |
| Terminal Subway Yard | ₡120,000 | ₡9,000 | Smaller terminal-style yard |
| Extra Tracks / Extra Depot-style upgrade | (verify) | (verify) | TheGamer: +5 vehicles, ₡75k / ₡16k — **confirm in UI** |
| Maintenance Hall | (verify) | (verify) | TheGamer: ₡50k / ₡34k — **confirm** |

Footprint (Venexis list): Subway Yard **44 × 20 cells** ≈ **352 × 160 m**. Leave room for upgrades beside the lot. [Venexis][TheGamer]

**Older TheGamer upkeep (₡48k/mo for yard) disagrees with wiki ₡18k — always trust the live tooltip.**

### 1.3 Tracks & electricity / noise
- Prefabs: **double** subway track (both directions) and **one-way** track (yard leads, wyes, specials). [TheGamer]
- TheGamer track costs (verify): double ≈ ₡2,500/km + ₡250/km upkeep; one-way ≈ half. [TheGamer]
- Built as elevated, at-grade, cut-and-fill, or tunnel — same tool family as train. [Paradox]
- Shallow tunnels conflict with **buried pipes and power cables**; go deeper or relocate utilities. [GameRant][TheGamer]
- Underground stations: **low noise** vs surface rail; good for dense districts. [GameRant]
- Stations need normal building services via their surface connection (road). Do not assume a magical third-rail power plant — verify any power complaints on the yard/stations in-game.

### 1.4 Lines, loops, reversing
- Create routes with the **Subway Line Tool** after stations are track-linked to a yard. [TheGamer][GameRant]
- Typical pattern: click platforms as waypoints and **close a loop** (or end-to-end with reverse at terminals). Yard then dispatches trains. [GameRant]
- Each station has **two tracks / platforms** — different lines can use opposite sides. [GameRant]
- Prefer **several purposeful lines** over one mega-line that visits every station. Cims transfer between subway lines and to bus/tram/train. [GameRant]
- Manage vehicles, color, name, schedule from Transportation → Subway → line details. [TheGamer]

### 1.5 Outside connections
- Map-edge outside connections exist for **road, train, air, power, water** — **not subway**. Subway stays inside the city network. [Pinter notes][Paradox local-transit framing]
- For airport / regional travel: subway to **International Airport** or a **passenger train** hub; trains handle outside edges.

### 1.6 Stop spacing (heuristic, not a hard engine constant)
- RedGingerRR: stations **too close** slow trips; **too far** hurt access — balance per line. [RG metro]
- Practical planning band for Barney: aim **~600–1,200 m** between stations on a metro spine (walk catchment + speed). Treat as design guidance; adjust after watching wait times / ridership.
- Feed stations with **bus/tram stops at the entrance**; do not duplicate the same corridor with a dense bus swarm. [GameRant][04_transit]

---

## 2. Station types & how they connect

### 2.1 Vanilla station menu (wiki building table)
| Station | Cost | Upkeep/mo | Platforms | Typical use |
|---|---|---|---|---|
| Underground Subway Station | ₡200,000 | ₡15,000 | 2 | Dense downtown / Ephrata core |
| Overground Subway Station | ₡100,000 | ₡7,500 | 2 | Suburbs / open land |
| Small Overground Subway Station | ₡50,000 | ₡3,750 | 2 | Light stops |
| Elevated Subway Station | ₡150,000 | ₡11,250 | 2 | Over roads / constrained ROW |
| Small Elevated Subway Station | ₡100,000 | ₡7,500 | 2 | Lighter elevated |
| Station Services upgrade | (verify; TheGamer ₡15k) | (verify) | — | Comfort / attractiveness |

**TheGamer (2023) listed underground ₡50k / overground ₡25k** — likely outdated vs wiki. **Confirm costs in-game before budgeting.**

DLC / pack / mod stations (Modern Metro Station, City Stations plazas, modular multi-platform mods) may exist — **label as non-base** and only use if the user has them. Vanilla guide assumes base + listed wiki rows.

### 2.2 Footprints (Venexis)
| Asset | Cells | Metres (×8) |
|---|---|---|
| Underground Subway Station | **3 × 2** | **24 × 16 m** surface entrance |
| Overground Subway Station | **21 × 6** | **168 × 48 m** |
| Subway Yard | **44 × 20** | **352 × 160 m** |

### 2.3 Exact depths & clearances (critical)
| Element | Elevation vs local ground H₀ | Source |
|---|---|---|
| **Underground station track nodes (vanilla asset)** | **≈ H₀ − 20 m** | CS2 wiki *Subway Station Measurements* (“Underground Subway Station 01 … at −20m”) |
| **Recommended tunnel depth to avoid shallow utilities** | **≈ H₀ − 30 m** (or deeper on hills) | TheGamer; Steam tips |
| **Useful underground range (guide)** | roughly **9–45 m** down (30–150 ft) | GameRant |
| **Pedestrian path transfer node** | **H₀ − 6.25 m** (= 18 ft) | Reddit consensus; RedGinger “18 ft underground” |
| **Tunnel mesh height clearance** | **4.3 m** | Wiki measurements |
| **Stacked line vertical separation (practice)** | **≥ ~10 m** between track levels (e.g. −20 and −30) | Derived from 4.3 m clearance + station bulk; GameRant: cross at different elevations without a junction |
| **Elevated platforms / viaduct over roads** | **≈ H₀ + 10 m** | Matches common CS2 road/rail bridge clearance practice |
| Elevation UI steps | **1.25 / 2.5 / 5 / 10 m** | Steam tips; TheGamer tunnels |

**Absolute vs relative depth:** elevation is relative at the start point, but a segment tries to keep **absolute** depth to the end — on rolling terrain, adjust the end height so the track still meets the station’s −20 m sockets. [GameRant]

**Platform length (asset creator guidance):** ≥ **148 m** platform length for full door alignment; ~**184 m** between nodes if trains need to pull in and reverse. [Wiki measurements]

### 2.4 Connection anatomy
1. **Surface:** underground entrance snaps to road/ped context; tiny lot fits between buildings. [Paradox]
2. **Vertical:** entrance ↔ platform inside the asset; optional **pedestrian path at −6.25 m** links nearby underground stations for transfers. Disable excess snapping; connect near stair cores. [Reddit][RG]
3. **Tracks:** snap to station end nodes at the station’s track elevation (−20 m for UG). Use double track through platforms.
4. **Utilities:** serve via the surface road network (power/water/sewage under roads).
5. **Feeders:** bus/tram/taxi at street level; optional parking nearby (not required for metro success). [GameRant]

---

## 3. Connecting multiple lines (best practices)

### 3.1 Why shared tracks hurt
- Flat diamonds and shared mainlines cause **bunching**, junction waits, and **yard deadhead** trains blocking revenue paths. [Design consensus from GameRant “use different sides” + metro ops practice; RG “plan before digging”]
- Rule for Barney: **one color = one pair of tracks**. Transfers = **walking**, not track merges.

### 3.2 Preferred: grade-separated stacked hub
1. Place **Underground Station A** (Line A N–S) with tracks at **H₀ − 20 m**.
2. Place **Underground Station B** (Line B E–W) within short walk; tracks at **H₀ − 30 m** (or −32.5 with 1.25 steps).
3. Ensure tunnels cross with **≥ ~10 m** vertical gap — **no track connection** between A and B.
4. Link stations with a **pedestrian path at −6.25 m** (and/or surface crosswalk). [Reddit][RG]
5. Put bus/tram stops at both entrances.

### 3.3 Preferred: parallel hub
- Two stations side-by-side (same or similar depth) for local + express or two corridors.
- Ped transfer at surface or −6.25 m path.
- Good for same-direction overlays without crossing.

### 3.4 When tracks must connect: yard spine & flying junctions
**Yard service spine (always):**
- One surface yard on cheap land (edge / industrial).
- **Non-revenue spine** (double or paired one-ways) from yard.
- **Spur / wye** into each independent line **away from busy platforms**.
- Revenue trains stay on their line; only yard moves use spurs. [Inferred from “all stations link back to yard” + avoiding shared mainlines]

**If two revenue lines must merge (interlining) — last resort:**
- Build a **flying junction**: one track ducks **~10 m deeper** under the other before joining — **never** a flat 4-way diamond. [GameRant cross-at-different-elevations]
- Use **gentle curves** (no sharp corners / avoid pure straight-tool kinks). [GameRant]
- Grades: target **≤ 2–3%** for rail realism; game allows steeper — still prefer long ramps. For 10 m rise @ 2% → **≥ 500 m** ramp; @ 3% → **≥ 333 m**.

### 3.5 Using both platforms
- Line 1 uses northbound+southbound pair; a second route can use the opposite platform face where geometry allows. [GameRant]
- Still prefer dedicated trackways for high-frequency lines.

---

## 4. Elevation cheat sheet for Claude

| Tier | Offset | Use |
|---|---|---|
| Elevated viaduct / station | **H₀ + 10.0** | Over roads |
| Overground station / surface metro | **H₀ + 0.0** | Suburbs |
| Ped transfer path | **H₀ − 6.25** | Station-to-station walk link |
| Line A / default UG station tracks | **H₀ − 20.0** | Vanilla UG sockets |
| Line B stacked / pipe-safe tunnels | **H₀ − 30.0** | Under Line A; dodge utilities |
| Optional deep express | **H₀ − 40.0** | Extra separation / rivers | 

Ramp length examples (Δh / grade):
| Δh | ≤3% | ≤2% |
|---|---|---|
| 10 m | 333 m | 500 m |
| 20 m | 667 m | 1000 m |
| 30 m | 1000 m | 1500 m |

Portal from surface yard lead at H₀ down to −20 m: prefer **≥ ~667 m** @ 3% or longer.

---

## 5. Build blueprints for Barney

Assume downtown / Ephrata districts + future link toward **Barney International Airport** (`INTERNATIONAL_AIRPORT_GUIDE.md`).

### Blueprint A — End-to-end spine
1. Place Subway Yard on edge (industrial / cheap tiles).
2. Underground stations every ~600–1,200 m along the spine (Ephrata Main ↔ downtown).
3. Double track at **−20 m**; yard lead ramps to spine via service spur.
4. User: create **Line A** visiting each station once each way (loop or out-and-back). Set vehicles after ridership appears.
5. Bus feeders at each entrance.

### Blueprint B — Dual counter-rotating loops
1. One loop clockwise on outer ring stations; one counter-clockwise on same or parallel tracks.
2. Or: two one-way loops on a double-track ring (each direction its own line).
3. Shared stations OK; **do not** add flat crossings between loops.
4. Yard spur touches the ring at a quiet point.

### Blueprint C — 2-line stacked underground transfer (zero track conflict)
1. Downtown hub: Station A (−20, N–S) + Station B (−30, E–W).
2. Ped path −6.25 m between stair areas; surface sidewalks too.
3. Line A = Ephrata–downtown–south; Line B = west–downtown–airport approach.
4. No track link between A and B except separate yard spurs.

### Blueprint D — Yard spine + flying junction (if needed)
1. Yard → east–west **service spine** at −20 or −25.
2. Spurs north to Line A, south to Line B, east to Line C.
3. If Line B must join Line A briefly: drop B to −30, cross under A, rise, merge with long curve — never diamond.

### Airport link
- Prefer **Line B** (or C) terminus under/near airport transit plaza (see airport guide).
- Or subway to a passenger **train** station that already serves the airport’s integrated rail.
- Do not expect subway outside-connection tourists; use air/train for that.

---

## 6. Phased checklist (Claude)

1. Confirm unlock + money; `get_build_prefabs` filter `Subway`.
2. Survey yard site (large flat surface) + station spots; ask before big spends.
3. `place_building` yard; `preview_building` then place stations.
4. `build_network` double tracks at correct elevations; one-way yard leads; `trace_network` yard→each line.
5. Give user **Underground View** checklist if connections fail (pipes, absolute depth mismatch).
6. User draws lines; set vehicles/fares; you re-survey waiting passengers / traffic after sim time.
7. Add bus/tram feeders; optional −6.25 m ped links at hubs.
8. Expand with new independent colors — not by splicing onto a congested shared trunk.

---

## 7. Uncertainties
- Exact live costs/upkeeps drift by patch — wiki vs TheGamer already disagree.
- Vehicle passenger capacity per train car is not pinned in the sources above — read prefab/tooltip.
- Modular multi-platform stations are **mods/DLC** — out of scope unless user confirms.
- Minimum legal curve radius in metres is not published as a single number here — use gentle curves and watch speed/“no connection” warnings.
- `bridgePlacementSupported` for each subway prefab must be checked live.

---

## 8. Sources
- Paradox Feature Highlight #3 Public & Cargo Transportation
- CS2 wiki Transportation / Subway building tables; *Subway Station Measurements* (−20 m UG networks; 4.3 m tunnel clearance; platform lengths)
- GameRant “How to Build a Subway” (yard link required; 30–150 ft depths; cross at different elevations; dual platforms; no train/subway track mix)
- TheGamer “How To Build Subways” (unlock path; yard upgrades; ~−30 m tracks; line tool loops; track $/km)
- Venexis Building Footprint Sizes (yard 44×20; UG 3×2; OG 21×6)
- RedGingerRR “Smarter Metros” transcript (spacing principle; 18 ft ped node; depot siting; transfer tips)
- Reddit r/CitiesSkylines2 ped path transfers at **−6.25 m**
- Steam subway tips (elevation step 1.25 m; absolute depth behavior)
- Pinter CS2 notes (outside connection types — no subway edge)
- Local: `cs2_guides/04_transit.md`, `TRAFFIC_GUIDE.md`, `INTERNATIONAL_AIRPORT_GUIDE.md`
- Blueprint: `subway_network_blueprint.png`
