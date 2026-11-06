# POWER_GRID_GUIDE — Barney electricity engineering for Claude

Research date: 6 November 2026. Game: Cities: Skylines II, not CS1.
Companions: [blueprint](power_grid_blueprint.png), `cs2_guides/05_utilities.md`,
`TERRAFORMING.md`, `SUBWAY_GUIDE.md`, `INTERNATIONAL_AIRPORT_GUIDE.md`.

## 0. Read this before construction

You are Claude Code controlling **Barney** through CitiesIIAgentBridge 0.5.0 community.
Separate **verified mechanics**, **source-table nominal values**, **planning choices**, and
**live measurements**. This document does not have a connection to the user's running game.
No coordinates, actual load, wind strength, socket heights or prefab IDs for Barney were measured.
Survey first; ask before buying tiles, removing existing infrastructure or major spending.

Critical corrections to assumptions in the task and older guides:
- **Transformer = converter, not battery.** Emergency Battery Station = energy storage.
- **Power Line = HV; Electric Cable = LV.** They are different networks, not interchangeable tools. [1,2]
- A standard transformer is commonly **80 MW**, not an unlimited feed. **40 MW LV / 400 MW HV**
  are conservative reference ratings, not a promise for every current/localized/modded prefab. [3,5,8]
- Do not assume an **Extra Transformer upgrade on the standalone station**. The sources show
  additional HV-connection upgrades on some generating plants; add another standalone transformer
  when that station is full. Discover actual upgrades in the building panel. [1,4,5]
- **Gravel roads and alleys are NOT established electricity exceptions.** The Roads wiki says
  all non-highway road types carry 40 MW LV; check custom roads individually. [3]
- **Highway lighting can add LV**, but does not add water/sewage. Bare highways lack LV. [2]
- **Buried at −10 m** is a proposed design layer, NOT a mandatory/default connection depth.
- **Snapping horizontally does not guarantee matching height**. Verify the real socket/node Y. [9]
- **Battery is a branch off the grid, never the only series bridge from outside power to city**.
  Recent player tests report batteries will not charge from outside imports. [7]

## 1. Exact architecture: bulk transport vs local delivery

```text
Outside HV <----> HV backbone <----> local generating plant HV output
                         |                     |
                         |                     +--> optional plant LV feed --> local road grid
                         +--> Transformer A --> several LV feeds --> Ephrata
                         +--> Transformer B --> several LV feeds --> downtown
                         +--> Transformer C --> several LV feeds --> airport / subway district
                         |
Local surplus generation +--> Emergency Battery --> LV district supply during deficit
```

Do not place battery storage inline as though it were a transformer. Do not wire ordinary homes
or roads directly to an HV Power Line and expect useful low-voltage supply. A transformer converts
HV→LV and also LV→HV for export. It accepts multiple connections, with a finite combined rating. [1]

### Capacity ledger

| Component | Planning reference | Meaning / evidence |
|---|---:|---|
| Power Line (HV) edge | **400 MW** | Conventional English rating and observed outside-line cap; UI/mod/localization differences exist [5,8] |
| Electric Cable / ordinary road LV edge | **40 MW** | Roads wiki; English values cited in capacity bug report [3,8] |
| Standard Transformer Station | **80 MW total** | Steam service-building table [5]; verify efficiency/budget in Barney |
| Emergency Battery Station | **500 MWh**, **200 MW output** | Wiki service table; not 500 MW of generation [4] |
| Additional Battery Bank | **+250 MWh**, **+100 MW output** | Battery upgrade, not substation upgrade [4] |

A node with 400 MW incoming does not magically push 400 MW through its one 40 MW outgoing edge.
The smallest source/transformer/edge/cut capacity along a route limits delivery. [1,2]

**Worked bottleneck (illustration, not measured Barney load):** 60 MW demand beyond one 40 MW
road feed leaves 20 MW undeliverable even if the city generates 250 MW. Add a *genuinely separate*
LV route around the saturated edge or put an HV-fed transformer beyond it. Two new roads that
rejoin before the same bottleneck do not remove it. Widening the old road does not raise its
LV rating. [1,2]

**80 MW is not proof of exactly two immutable 40 MW sockets.** A station may have several network
attachment opportunities; inspect the actual prefab. Design at least two independent outgoing
LV routes, then verify the building total and all shared edges. Port count is not conversion capacity.

### Rating conflicts / test protocol
Paradox forum report 1761454 (game 1.2.5f1 and later replies) documents labels of 50/500 or 100 MW
and measured transfer below labels. It does NOT establish that every modern LV edge is exactly
50 or 100 MW. Read Barney's English tooltip, hover actual edges, record observed flow and test
under load before upgrading the design capacity. Use the conservative 40/400/80 ledger meanwhile. [8]

## 2. Which roads carry electricity?

| Road/network | Default expectation | Action |
|---|---|---|
| Ordinary small/medium/large road | Built-in LV | Adjacent buildings connect via road [1,2] |
| Gravel road / alley | Non-highway LV according to wiki | Do not blame missing sidewalks; inspect actual edge [3] |
| Bare highway | NO built-in LV | Bridge gap with Electric Cable, or use lighting upgrade [2] |
| Highway with streetlights | Can carry LV | Check Electricity View; still no water/sewage [2] |
| Pedestrian path, railway or subway | Do not assume road-style LV distribution | Explicitly connect buildings/roads where necessary |
| Custom Road Builder/mod road | Unknown until inspected | Verify utility settings and live flow |

Touching in plan view is not an electrical junction. A highway crossing, bridge or disconnected
road island can look continuous without carrying any LV between districts. [1,2,9]

## 3. Buildings, real names, footprints and useful upgrades

Reference values below are table values, NOT quotes from Barney. Cost is construction;
upkeep is nominal per month before efficiency/budget/fuel changes. Cells are 8 m per side.

| Building / upgrade | Cost | Monthly upkeep | Footprint | Electrical function |
|---|---:|---:|---|---|
| Transformer Station / older label Transformer Substation | ₡30,000 | ₡3,000 | **6×7 cells = 48×56 m** | ~80 MW conversion; HV↔LV [4,5] |
| Emergency Battery Station | ₡150,000 | ₡25,000 | **22×10 = 176×80 m** | 500 MWh / 200 MW max discharge [4,5] |
| Additional Battery Bank | ₡75,000 | ₡3,750 | Inspect live; do not invent lot expansion | Adds 250 MWh / 100 MW [4] |
| Diesel Generator (battery upgrade) | ₡24,000 | ₡4,250 base + fuel | **5×4 = 40×32 m** | Backup fuelled generator; 2025 table lists up to 7.5 MW [4,5] |
| Wind Turbine Battery Extension | ₡10,000 | ₡1,000 | Integrated; inspect | 10 MWh / 4 MW battery output [4] |
| Geothermal High Voltage Substation | ₡15,000 | ₡1,500 | **4×4 = 32×32 m** | Additional HV connection, not a storage capacitor [5,6] |
| Solar High Voltage Substation | ₡15,000 | ₡1,500 | **5×4 = 40×32 m** | Additional HV connection [5,6] |

Wiki may label the plant HV extension “Additional Transformer Station”; the older footprint guide
calls it “High Voltage Substation”. Read the parent building panel and prefab name, rather than
assuming all descriptions refer to a standalone transformer capacity upgrade. [4–6]

### Generator comparison and connection rules

| Generator | Reference output | Connection / siting rule |
|---|---:|---|
| Wind Turbine | Site-dependent; 2025 table 0–5 MW, newer wiki 6 MW | **LV only**; buried node beneath mast. Use Electric Cable, not HV Power Line. Roadside may connect automatically; verify. [1,2,4,5] |
| Small Coal Power Plant | **20 MW** | Local LV feed and reported plant HV node; coal deliveries; high air/ground pollution. No standard upgrades in launch diary. [1,2,4] |
| Gas Power Plant | **250 MW** base | Bulk HV plus local output as exposed; fuel is petrochemicals; storage, furnace/filter/turbine upgrades. Additional turbine +100 MW and extra HV connection. [1,4,5] |
| Coal Power Plant | **300 MW** base | Bulk HV; maintain coal logistics and distributed transformers; turbine +100 MW. [1,5] |
| Geothermal Power Plant | **up to 150 MW** base | On groundwater; pollutes that reservoir. Do not share with drinking intake. Add HV connection extension if needed. [1,5] |
| Solar Power Plant | **up to 200 MW** base | Sun/weather/latitude dependent, no sunlight at night; integrated storage and battery upgrades. Bulk HV plus limited local-side behavior: not a general pass-through station. [1,5,7] |
| Nuclear Power Plant | **750 MW** base | Major HV export/distribution paths; high water requirement, high upkeep, high noise; game diary says no fuel deliveries needed. [1,5] |
| Hydroelectric Power Plant | **Variable** | Dam height/head/flow and outlet capacity matter; query actual sockets. Don't budget from a preview's huge theoretical figure alone. [1,8] |
| Incineration Plant (bonus) | Waste-dependent | Steam table up to 40 MW base; useful cogeneration, not guaranteed base supply. [5] |

**HV-only vs HV+LV:** GameRant describes ordinary plants as having both nodes; explicitly exempts
wind turbines. The diary says plants feed local cables and can be linked through HV. No complete
current socket-by-socket prefab inventory was available. Never promise that every large plant can
transfer its full output into its frontage road or act as a transformer for other sources. Match
actual HV/LV ports per prefab; do not omit remote transformers because a plant touches a road. [1,2,7]

## 4. Build overhead HV/LV, bury them, and transition correctly

### Tool selection
- Electricity menu → **Power Line** for HV bulk transmission / outside connection.
- Electricity menu → **Electric Cable** for LV local road gap / wind connection.
- Use Ground/elevation **0 m** for a normal surface-supported overhead alignment. This is the
  support placement level, NOT the actual wire conductor height.
- Use Page Down / elevation control for underground; Page Up to return to ground. Both HV and LV
  can be buried. [1,2,10]
- Standalone support footprints in Venexis: cable **1×1 cell**, HV pylon **2×2 cells** (8×8 / 16×16 m).
  These are historical planning footprints, not full wire/building safety-clearance envelopes. [6]

### Overhead route (user workflow)
1. Open Electricity View and select the correct-voltage tool.
2. Enable **Snap to Existing Geometry** initially; hover the actual source node, not its nearby fence.
3. Start at the source socket, extend outward into a utility corridor and toward the destination.
4. Keep supports out of zoned lots, roads and bridges; let native preview reject conflicts.
5. Follow rural/highway/rail edges for realism. Do not run pylons through airport approach sectors.
6. Hover the destination socket; confirm snap and valid preview; place; verify flow after unpausing.

### Underground design layers (planning choices, not guaranteed defaults)

| Layer | Target relative to surveyed local ground H0 | Caution |
|---|---:|---|
| Shallow road utilities | Inspect; diagram shows **−2.5 to −5 m illustrative band** | No universal fixed pipe depth established |
| Buried HV or manual LV corridor | **H0−10 m** initial proposal | Avoid tunnels and station concourses; match endpoints |
| Alternate power crossing layer | **H0−15 m** if conflicts | Still above default UG subway rails, not automatically clear of station structures |
| Subway A reference | **H0−20 m** | Default station node reference from subway guide |
| Subway B reference | **H0−30 m** planned crossing | Do NOT assume a vanilla station is freely placeable at this depth |

Use absolute heights: `Y_power = H0−10`. Recompute cover along hills; a line can remain at a
nearly constant absolute Y while ground changes. None of these offsets guarantees an error-free
crossing; inspect the full mesh/corridor and use the placement preview. [9,10]

**Overhead-to-buried transition:** start at an existing surface line node, lower the next endpoint
below ground and place a valid native transition segment; continue underground. At a station,
match its real socket height (often a buried cable connection for LV). If preview rejects a
transition, build a short segment out from the asset's real socket first, then join the corridor.
Do not look for or invent a required separate “portal building”. [1,2,10]

**Costs:** no reliable current like-for-like underground/overhead price multiplier was verified.
Do not claim burial is free, equal-cost or a fixed 2× premium. Compare native segment previews
for identical endpoints and record construction/upkeep costs; choose appearance vs cost explicitly.

## 5. Socket snapping: the plant, the transformer, the battery

The blueprint is **functional/schematic**. Exact socket coordinates, number, colors and height depend
on prefab/rotation/upgrades; there is no trustworthy universal “HV is on the left” rule.

GameRant's launch Electricity View uses **HV blue**, **LV yellow/orange**. This is the opposite of
some proposed color assumptions. Follow labels/network type and actual arrows, not bolt color alone. [2]

### Plant → transformer → city
1. Inspect/select plant and transformer in Electricity View.
2. Draw **HV Power Line** between their HV sockets. Mere proximity does not connect networks.
3. Connect transformer **LV side** to road LV or manual Electric Cable; a road-access connection
   alone is not proof the correct electrical node is attached and carrying power.
4. Provide multiple independent downstream LV paths; check station total and edge limits separately.
5. Wind turbine exception: underground LV node beneath mast → Electric Cable → LV road. [1,2]

### Emergency Battery Station
1. Place off a serviced local road with land for Additional Battery Bank/Diesel Generator.
2. Connect the battery's actual **charging/HV side** to local-generation supply as exposed by prefab,
   and its **LV discharge side** into well-distributed local feeds.
3. Keep a normal transformer path in parallel. A battery is not a replacement converter.
4. Observe charge over a surplus interval, discharge over a deficit interval, and actual LV bottlenecks.
5. Community tests report **no charge from imported power**, and HV charging→LV discharge behavior.
   Treat this as version-sensitive empirical evidence, not a universal immutable specification. [1,7]

### Correct depth matters
Snap to Existing Geometry is helpful, not magic: Paradox explicitly says snapping can align X/Z
without adopting the other network's Y. If a cable passes above/below a node or shows overlap,
match the actual node height and join it from a better angle; disable conflicting guidelines if needed.
Do not repeat the earlier subway video's unqualified “depth auto-locks” claim. [9]

## 6. Storage sizing, solar nights and backup

Use the correct units:
- **MW** = instantaneous power delivered/consumed.
- **MWh** = stored energy. `E_required = deficit_MW × duration_hours` is a planning identity.
- Battery power and the cables/transformers can bottleneck well before energy is empty.

Example arithmetic (not a promise about CS2 clock speed): 500 MWh / 50 MW = **10 energy-hours**;
500 / 200 = **2.5 energy-hours** at full output. Verify the game's actual discharge/clock behavior.
A full battery is not a permanent generator; importing power through a normal substation may still
be needed. Daytime solar surplus must exceed daytime consumption AND charge enough for the night. [1,4]

The Diesel Generator is a small fallback with fuel deliveries, not a 200 MW replacement for an
empty battery. Keep its road route unjammed. Add battery banks only when energy or discharge output,
not the downstream LV route, is the actual constraint. [4,5]

## 7. Imports, exports and money

- Valid HV outside connections automatically import on deficit / export surplus. [1]
- Connect to an existing outside HV network first; a new edge requires bought boundary tiles and
  an actual named outside-connection arrow. Never confuse a purchased tile edge with the map edge. [2]
- A **400 MW** reference limit applies to each limiting HV path, not unlimited output per plant.
  For a nominal 750 MW nuclear plant with negligible local demand, `ceil(750/400)=2` independent
  paths are the mathematical minimum. Two branches merging into one 400 MW segment still cap at 400.
  Socket availability, plant output and destination capacity must also support both paths. [5,8]
- Importing can be expensive; exports may offset production cost but **gross sales are not profit**.
  No dependable current universal export tariff was verified; do not repeat historical “money printer”
  claims. Measure Electricity Trade and budget before/after steady load, subtract fuel/upkeep/imports. [1]
- Temperature changes increase heating/cooling demand. Reserve margin for extremes, not just noon. [1]

## 8. Barney / Ephrata / airport power masterplan

**Proposed layout, not measured existing infrastructure:**
1. Rural HV backbone along infrastructure corridors; use overhead where visually appropriate.
2. Downtown and airport approaches: proposed underground **−10 m**, alternate **−15 m** after survey.
   Do not obstruct runways or destroy subway/station pedestrian connections.
3. Put **three distributed Transformer Stations** near Ephrata, downtown and the airport/subway district;
   build incrementally when measured load requires, not all at once for a small town.
4. Each nominal 80 MW station: at least two independently routed LV feeds into different district
   road nodes. Avoid combining them through one 40 MW neck.
5. Target operational loading under **80%** as an elective planning margin, NOT an engine rule.
6. If the user wants **80 MW unused airport/subway conversion headroom**, calculate it explicitly:
   `sum(verified usable transformer ratings) − measured district peak ≥ 80 MW`.
   A single 80 MW station supplying a positive load cannot meet 80 MW unused reserve!
   Two standard stations provide nominal 160 MW total; reserve holds only if peak ≤80 MW and
   generation, HV and independent LV feeds all support it. This is not full N−1 redundancy.
7. Optional solar + battery branch near utilities park; diesel backup; fire-service road access.
8. Reassess after airport, new subway yard, industrial expansion or winter peak opens. Never guess
   that airport/subway load is 80 MW just because the design margin is 80 MW.

## 9. Bridge execution and validation

Reviewed reference: CitiesIIAgentBridge `COMMANDS.md` 0.5.0 community (52-command reference). [11]

**Read/query:** `ping`, `get_capabilities`, `get_build_prefabs`, `get_prefab_details`, `get_buildings`,
`inspect_entity`, `get_network`, `get_network_edges`, `trace_network`, `diagnose_connections`,
`get_city_diagnostics`, `get_city_management`, `sample_terrain`, `get_city_map`, `get_tiles`.

**Construction:** `place_building` / `preview_building`; `build_network` for supported HV/LV networks;
`upgrade_network` for native supported edge replacements. `set_service_budget` exists (50–150) but
requires deliberate spending approval; there is no documented per-building battery dispatch or
building-upgrade installation command. Ask user for parent-building upgrades and unsupported UI.

### Workflow
1. Pause/save checkpoint; record city session, shortages, import/export and available money.
2. Discover network prefabs with separate `filter` searches (`Power`, `Electric`, `Cable`), and buildings
   (`Transformer`, `Battery`, plant names); paginate. Names are discovery hints, not hardcoded IDs.
3. Require `locked:false`, `bridgePlacementSupported:true`; inspect capacity/placement data.
4. Query actual nodes/edges near source and destination; distinguish HV from LV. `get_buildings`
   gives building placement, not automatically the exact electrical socket coordinates.
5. Position objects allow `x,z`, optional **absolute `y`**, plus live `index/version` attachment.
   Edge attachments also need `curvePosition` (0–1). Never recycle stale entity versions.
6. `build_network` uses prefab IDs, `start`, `end`, `maxCost`, optional `control` and **`elevation` offset
   −100…100**. Inspect live placement behavior before combining explicit absolute `y` and offsets;
   never blindly apply −10 twice. Start/end must match real sockets, not a schematic rectangle center.
7. Build one bounded connection, poll `get_operation` to terminal status, inspect created entities.
   Never retry an uncertain mutation without checking its operation result.
8. `trace_network` checks **physical graph connectivity only**, NOT MW flow, voltage conversion,
   capacity, battery charge or route usability. Building-internal voltage conversion may also need
   its own inspection rather than a single end-to-end graph trace.
9. Advance a bounded simulation interval; re-query `diagnose_connections` and utility shortages.
   Handle null/partial data as unknown, not proof of supply.
10. Ask user to confirm Electricity View edge flow/limit, transformer load, Battery Charge and
    Electricity Trade. Bridge native financial fields can be raw units; do not label as money/MW
    without field-unit confirmation. Record observed peak loads for later sizing.

**User-only/live checks:** UI flow arrows and exact hover capacity; wind strength overlay; upgrade
buttons; socket validity if native placement refuses; underground conflict view; disaster alerts.
Claude cannot terraform and should not invent capacitor-control or grid-switching commands.

## 10. Failure diagnosis (in this order)

| Symptom | Check / fix |
|---|---|
| Whole city dark | Source/import connection, station power conversion, correct HV/LV nodes |
| Supply exceeds demand but one district dark | LV bottleneck / transformer capacity; bypass beyond bottleneck [1,2] |
| Wind farm unconnected | Wrong HV tool; use buried LV mast node [2] |
| District beyond highway dark | Missing LV bridge, lighting not installed, or disconnected road graph [2] |
| Battery never fills | No local surplus, imported-only source, wrong port, service state [1,7] |
| Battery full but brownouts | LV path/output bottleneck, not energy shortage |
| Export plateaus | Common HV edge limit, port limit, local demand and source rating [8] |
| Fuelled plant output falls | Coal/petrochemical inventory, delivery traffic, workers, budget [1] |
| Nuclear shortage | Water supply and service efficiency; not a real-world fuel-truck mechanic [1] |
| Dam output disappointing | Head/flow, reservoir filling, outlet bottleneck; flood safety first [1,8] |
| Underground line “overlapping” | Node Y mismatch, pipe/tunnel conflict, wrong network prefab [9,10] |

Dams can flood land and disrupt rivers; checkpoint and user landscaping approval before alterations.
For disasters/fire/storms, diversify routes and retain fire access. No verified fixed lightning failure
probability or guaranteed special pylon-strike mechanic was found: do not invent one. Buried routes
improve aesthetics, not proven invulnerability to all disasters.

## Sources and evidence limitations

[1] **Official Feature Highlight #6 (not #5), Electricity & Water** — primary mechanics, transformer
conversion, batteries, plants, fuel, temperature, bottlenecks and info view:
https://www.paradoxinteractive.com/games/cities-skylines-ii/features/electricity-water

[2] **Chris Drumm / GameRant, How to Connect a Power Network (2023)** — HV blue/LV yellow-orange,
plant nodes, wind exception, road caps and highway lighting. Old article; do not rely on its platform-release claims:
https://gamerant.com/cities-skylines-2-how-to-connect-a-power-network/

[3] **CS2 Wiki, Roads** — 40 MW non-highway road rating including road-type list:
https://cs2.paradoxwikis.com/index.php?title=Roads
Full fetch blocked (406); specific indexed search excerpt consulted this turn.

[4] **CS2 Wiki, Service buildings** — costs, footprints, MW and **MWh** battery values:
https://cs2.paradoxwikis.com/Service_buildings
https://cs2.paradoxwikis.com/index.php?section=2&title=Service_buildings&veaction=edit
Full fetch blocked (406); indexed table excerpts consulted. Values are references, not live Barney.

[5] **PalmTreeLife, Service Buildings Cost, Upkeep, Footprint and More (2025)** — nominal outputs,
80 MW transformer, plant upgrades. Contains mods/packs and some storage-unit typos: corrected MWh
using [4], did not treat its entire mixed list as vanilla:
https://steamcommunity.com/sharedfiles/filedetails/?id=3525176513

[6] **Venexis, Building Footprint Sizes (2023)** — historical support/building sizes, extension space:
https://www.modscities2.com/building-footprint-sizes/

[7] **r/CitiesSkylines2, I looked into how power currently works (May 2026)** — empirical battery
behavior; not official engine specification:
https://www.reddit.com/r/CitiesSkylines2/comments/1t4ulaq/i_looked_into_how_power_currently_works/

[8] **Capacity conflicts and HV limits** — localization/mod-related uncertainty; no universal new rating asserted:
https://forum.paradoxplaza.com/forum/threads/low-voltage-powerlines-have-less-capacity-than-indicated-by-the-ui.1761454/
https://www.reddit.com/r/CitiesSkylines2/comments/1lh2u5k/is_there_some_way_to_bypass_400_mw_powerline_limit/
Forum fetch blocked; indexed report/replies consulted.

[9] **Paradox / co_avanya, Snapping doesn't take elevation into consideration** — official response
explains horizontal/depth independence (launch version; verify current preview):
https://forum.paradoxplaza.com/forum/threads/snapping-doesnt-take-elevation-into-consideration.1604110/

[10] **Underground placement**:
https://itemlevel.net/how-to-fix-electricity-bottleneck-in-cities-skylines-2/
https://steamcommunity.com/app/949230/discussions/0/4031347532797050819/

[11] **Bridge command reference**:
https://github.com/FTPAiYT/cities2-agent-bridge-ndc/blob/main/COMMANDS.md
Existing local 0.5.0 copy checked; actual game capabilities must be queried.

[12] **Quick Tips, How to Fix Electricity Bottleneck in Cities Skylines 2** — existing transcript
reviewed: more distributed transformers, underground connections (creator uses 20 m down):
https://www.youtube.com/watch?v=V6obrs92ONQ
Use −10/−15 proposed power corridors for Barney only after checking subway conflicts.
