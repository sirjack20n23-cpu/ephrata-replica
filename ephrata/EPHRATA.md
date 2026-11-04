# EPHRATA.md — Build a replica of Ephrata Borough, PA in "Barney"

You're Claude Code, controlling Cities: Skylines II through **Cities II Agent Bridge 0.5.0 community**. Read `AGENTS.md` and `COMMANDS.md` first. This file adds to them and doesn't override them. The city is **Barney**, so keep that name. Only use commands listed in `COMMANDS.md`, and run them as
`pwsh -NoProfile -File .\bridge.ps1 <command> -ArgsJson '<json>'`. For long JSON, write it to a file and pass
`-ArgsJson (Get-Content .\ephrata\work\<file>.json -Raw)`.

## Files in this folder
| File | What it holds |
|---|---|
| `ephrata_roads.json` | 1,611 straight road segments in **66 ordered build batches**, plus the borough boundary, railways, waterways and landmarks. Coordinates are local metres (`e` = east, `n` = north) from the origin. |
| `ephrata_zones.json` | 1,679 frontage zoning strips, one per side of a road segment. Each has an Ephrata district code and a suggested CS2 zone. |
| `ephrata_preview.png` | Preview of the plan: black = roads, colours = zoning, blue line = borough boundary, blue = creeks, green = rail/Rail Trail. |

Origin (0,0) is the **Main St (US 322) / State St (PA 272) intersection** downtown (OSM node 3401013325, 40.17947 N, -76.17923 W, approximately). The borough spans about e −2650…+2540 and n −1830…+2450, so the full town is roughly 5.2 km × 4.3 km at real scale.

## Golden rules (safety)
1. **Ask the player before each phase.** Before anything changes, show the phase, the segment count, the estimated cost and the area involved.
2. Before every phase, run `save_checkpoint` with a label like `ephrata-phase-2-before` and confirm it completes with `get_operation`.
3. Keep the city paused. Use `pause_for_analysis` and only advance time with bounded `simulate_step`/`advance.ps1` when the player agrees.
4. Use small batches: 25 or fewer `build_road` steps per `batch_execute` (the limit is 64). Every step needs a `maxCost`. Set the batch `reserve` to at least 25% of current money, read from `get_city_state`/`get_city_management`.
5. After each batch, poll `get_batch` until it finishes. Then check the result with `get_network_edges` (radius ≤2000) or `get_city_map` (radius ≤500) around the batch, and with `diagnose_connections`. **A batch is sequential, not transactional, so never replay steps that already succeeded.** Inspect first, then retry only the failed step.
6. Respect `STOP`. If anything is uncertain, stop and ask.
7. Never demolish existing Barney buildings or roads unless the player approves it specifically (`allowDemolition` stays false).

## Phase 0: Survey Barney and pick the origin (read-only)
1. Run `ping`, `get_capabilities`, `get_city_state` and `get_city_management`, then `get_tiles`, which gives owned tiles, boundary polygons and the remaining purchase allowance.
2. Find a large, **flat, empty, owned** area. Use `get_city_map` (radius 500) at several points and `sample_terrain` on a grid (about 100 m spacing, ≤1024 points) over the candidate area. Reject areas with water depth > 0 or slopes over about 8%, and avoid existing roads and buildings.
3. Choose `originX, originZ` (world position for Main & State), `rot` (degrees; 0 means real north = CS2 +Z, so confirm the orientation with `get_camera`/`get_city_map`), and `scale`:
   - **scale 1.0 (recommended for "precise")**: real distances. The downtown core (|e|,|n| ≤ 800 m) fits in about 3×3 tiles. The whole borough needs about 9×8 tiles (CS2 tiles are about 623 m).
   - **scale 0.75**: this is the smallest scale that still leaves room for zoning between Ephrata's parallel streets, whose blocks are about 90–120 m deep. Below 0.75, blocks get too thin to hold zone cells.
   - If the land isn't big enough, build the **downtown core first** and grow outward later.
4. If tiles are needed, list which ones (`get_tiles` ids), what they cost and how much allowance remains, and **ask before** calling `purchase_tiles` with its `maxCost`.
5. If Barney's highway/outside connection is far away, plan a link road (a Medium Road) from the nearest existing road to the US 322 ends of the replica, and ask the player about it.
6. Report your chosen origin, scale and rotation, the tiles needed and the terrain issues to the player. Wait for approval.

Coordinate transform for every point `{e,n}`:
```
worldX = originX + scale*( e*cos(rot) - n*sin(rot) )
worldZ = originZ + scale*( e*sin(rot) + n*cos(rot) )
```
Round to 0.1 m. Don't pass `y`; the bridge samples terrain height itself.

## Phase 1: Resolve prefabs (read-only)
Prefab IDs are live per session. **Never hard-code them.**
- Roads: `get_build_prefabs` with `{"kind":"network","filter":"Road"}`. Follow `nextOffset` until it's null. Only use entries that are `locked:false` and `bridgePlacementSupported:true`. Map them using `roadPrefabHints` in the roads file:
  - `arterial` (US 322 Main St / PA 272 State St / Reading Rd): Medium Road (a 4-lane avenue if `lanes>=4`)
  - `collector` / `collector_minor`: Small Road, or Medium Road if `lanes>=4`
  - `local`: Small Road. Use a one-way small road when `oneway:true`, built start→end (the order follows OSM's travel direction).
  - `arterial_link`: a one-way small road
  - `alley`: Alley if it's unlocked, otherwise skip it (optional phase)
- Zones: `get_build_prefabs` with `{"kind":"zone"}`. Map each `cs2Zone` name in `ephrata_zones.json` to the closest unlocked zone prefab (names differ by theme, NA/EU). Show the final mapping table to the player.
- Use `get_prefab_details` for anything unclear. Save the mapping to `ephrata\work\prefab-map.json`.

## Phase 2+: Roads, in the batches' order
`ephrata_roads.json.batches[]` is already ordered: `2-arterials` → `3-collectors` → `4-local` → `5-alleys`, and nearest the origin first inside each phase. (No phase-1 highway segments fall inside the borough. US 222 runs just east of it; if wanted, add it later as a separate connection.)

For each batch:
1. Transform every segment's `start`/`end`.
2. Build a `batch_execute` file:
```json
{"reserve":<money*0.25>,"steps":[
 {"command":"build_road","args":{"prefabIndex":<i>,"prefabVersion":<v>,"start":{"x":..,"z":..},"end":{"x":..,"z":..},"maxCost":<limit>}}
]}
```
   Consecutive segments share exact endpoints, so native snapping joins them into intersections. Segments with `bridge:true` cross Cocalico Creek or the rail line. If terrain needs it, try `elevation` (−100…100) after reading the prefab placement info, or leave them for the player.
3. Run it, poll `get_batch`, then verify with `get_network_edges`/`trace_network` between batch ends, plus `diagnose_connections`.
4. Tell the player what was built, what failed and why, and the money left. Ask before the next batch. Group several batches per approval only if the player says so.

Optional extras: the `railways` array has the active rail line (Reading & Columbia / Norfolk Southern) and the abandoned line, which is now the **Ephrata Rail Trail** (LPD district). Build the rail as train track only if the player wants it. Use a pedestrian path for the trail if one is available.

## Phase 6: Zoning
Only do this after the roads in an area exist and pass verification.
- Each entry in `ephrata_zones.json.zones[]` gives `segmentId`, `side`, `district`, `cs2Zone`, and `start`/`end` corners of a frontage strip (`depth` 40–48 m). Transform both corners.
- For exact alignment, first call `get_zone_cells` (radius ≤500) near the strip's start corner. Take the zone **block** `index`/`version` the start corner falls on, and pass them in `start` (block-attached start), as `COMMANDS.md` describes. Without that, the marquee follows the camera axes.
- Call `zone_rectangle` with the zone prefab IDs plus `start`/`end` (diagonal ≤500 m). It won't overwrite existing zones. Use `clear_zoning` only with player approval.
- Work district by district: CBD and NCD downtown first, then RHD/RMD around downtown, then RLD outskirts, then HCD/LIC/GIC corridors. Check demand with `get_city_management` before zoning large areas. Don't zone everything at once. Use bounded `simulate_step` runs and check demand between rounds.
- CD (conservation: Ephrata Cloister grounds, Cocalico Creek floodplain, the large eastern park) and LPD (the Rail Trail) stay **unzoned**. Suggest parks to the player.

District → CS2 mapping (the full table is in `districtToCS2`):
| Ephrata district | Meaning | CS2 zone |
|---|---|---|
| RLD | Low-density residential | Residential Low |
| RMD | Medium-density residential (twins/rowhomes) | Residential Medium Row |
| RHD | High-density (traditional mixed housing) | Residential Medium (avoid towers) |
| CBD | Central Business District | Residential Mixed, or Commercial Low on Main St |
| NCD / NCD2 | Neighbourhood commercial / apartments + commercial | Commercial Low / Residential Medium |
| HCD | Highway commercial (N Reading Rd/US 322 west, PA 272 south) | Commercial Low |
| LIC / GIC | Light / general industrial-commercial (along the rail line, north) | Industrial (Office Low is fine for LIC) |
| CD / LPD | Conservation / linear park | none |

## Phase 7: Services and utilities
The replica files cover roads and zoning only. Use `find_building_sites`/`preview_building`/`place_building` for water, power, police, fire, schools and health, with player approval for each placement. Landmarks are in `landmarks`: Ephrata Cloister, the station/Rail Trail hub and the hospital area. Their coordinates are approximate.

## Known limits (tell the player)
- **The bridge can't change terrain.** Real Ephrata sits on rolling ground, with Cocalico Creek along the west and north and hills to the south-east. Barney's terrain will differ. The player may need to use the in-game Landscaping tools (see `TERRAFORM.md` if present), or accept that some roads won't fit.
- Roads are straight segments (simplified at 3 m, max 240 m each). Curves are approximated with short chords. Lane counts in OSM are often missing, so the default is 2-lane.
- Zoning comes from the **2019 official zoning map**, georeferenced to OSM and sampled at road frontage. It's approximate, not parcel-exact. The Retail Overlay (ROD) and Historic Buildings (HB) overlays aren't modelled.
- Driveways, parking aisles and footpaths are excluded.
- CS2 grows its own buildings. Lot sizes and building styles will look like CS2, not like Lancaster County.
- The whole borough at scale 1.0 costs a lot of money and tiles. Build it in phases as the treasury allows.
- Google Maps data isn't freely downloadable, so OpenStreetMap (which closely matches Google's street layout here) was used instead.

## Sources
- © OpenStreetMap contributors, ODbL 1.0. Roads, railways, waterways and borough boundary (relation 187692) fetched 2026-11-04 from api.openstreetmap.org.
- Ephrata Borough Zoning Map, As Adopted August 12, 2019 (Ord. 1550): https://www.ephrataboro.org/DocumentCenter/View/654/
- Ephrata Borough Code Chapter 319 Zoning, §319-8 districts: https://ecode360.com/6860466
- Cities II Agent Bridge 0.5.0 community COMMANDS.md / AGENTS.md: https://github.com/FTPAiYT/cities2-agent-bridge-ndc
