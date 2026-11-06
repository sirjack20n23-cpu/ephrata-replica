# CS2_CREATOR_GUIDE.md — Claude knowledge pack for city "Barney"

Who this is for: you are **Claude Code**, controlling the user's Cities: Skylines II city **Barney** through the **CitiesIIAgentBridge** mod. You can place roads, zoning, and many buildings, and you can read map/traffic/city data. You **cannot** terraform (see `TERRAFORMING.md` if present). You often **cannot** toggle traffic lights, set lane connectors, change signal phases, adjust service budgets/taxes in the UI, or use every mod tool — when a step needs those, **tell the user exactly which in-game tool to use** and wait.

This pack summarizes practical advice from **~30 creative and realistic CS2 YouTube creators** (transcripts pulled **2026-11-05**). Every rule cites a source `[S#]`. Do **not** invent numbers. If advice might be outdated after Economy 2.0 / later patches, treat it as a starting point and verify in-game.

## How to use this pack
1. Read this index first.
2. Open the topic file for the job at hand under `cs2_guides/`.
3. For deep traffic/highway/intersection rules, open **`TRAFFIC_GUIDE.md`** (do not duplicate it).
4. For the Ephrata PA replica, also follow **`ephrata/EPHRATA.md`**.
4b. For a large airport campus, follow **`INTERNATIONAL_AIRPORT_GUIDE.md`** + `international_airport_blueprint.png`.
4c. For metro/subway networks, follow **`SUBWAY_GUIDE.md`** + `subway_network_blueprint.png`.
4d. For electricity, powerlines, substations, and battery stations, follow **`POWER_GRID_GUIDE.md`** + `power_grid_blueprint.png`.
5. **Ask the user before:** buying map tiles, large demolitions, major budget/tax changes, deleting signature buildings, or anything irreversible that costs a lot.
6. After each phase: survey Barney (map, demand, traffic, services), report findings, then propose the next action.

## Quick operating rules (always on)
- Prefer **road hierarchy**: highway → arterial → collector → local. Details in `TRAFFIC_GUIDE.md`.
- Zone to **demand bars**, not max fill. Residential has separate low / medium / (high or low-rent) demand. [S01]
- Keep **polluting industry and sewage outlets** downwind / away from water intakes and aquifers. [S01][S28]
- Build **local production** (specialized industry + manufacturing) so goods are cheaper and buildings level faster. [S01]
- Layer transit: taxis/buses → trams → metro/train. Leave room for expansion. [S04][S15]
- One change at a time for traffic or economy fixes; let sim time pass; re-check.

## Topic index
| File | Covers |
|------|--------|
| [cs2_guides/01_roads_traffic.md](cs2_guides/01_roads_traffic.md) | Brief roads/traffic; points to TRAFFIC_GUIDE.md |
| [cs2_guides/02_zoning_demand.md](cs2_guides/02_zoning_demand.md) | Zoning, density, mixed use, demand |
| [cs2_guides/03_industry_cargo.md](cs2_guides/03_industry_cargo.md) | Industry, specialized industry, cargo, ports |
| [cs2_guides/04_transit.md](cs2_guides/04_transit.md) | Bus, tram, metro, train, taxi, ferry, airport |
| [INTERNATIONAL_AIRPORT_GUIDE.md](INTERNATIONAL_AIRPORT_GUIDE.md) | Full International Airport campus + terrain heights + blueprint |
| [SUBWAY_GUIDE.md](SUBWAY_GUIDE.md) | Subway/metro lines, stations, depths, multi-line hubs + blueprint |
| [cs2_guides/05_utilities.md](cs2_guides/05_utilities.md) | Power, water, sewage, garbage, telecom |
| [POWER_GRID_GUIDE.md](POWER_GRID_GUIDE.md) | HV/LV grids, underground lines, socket snapping, transformers, batteries, bottlenecks + blueprint |
| [cs2_guides/06_services.md](cs2_guides/06_services.md) | Police, fire, health, education, deathcare |
| [cs2_guides/07_economy_happiness.md](cs2_guides/07_economy_happiness.md) | Taxes, budget, rent, labor, wellbeing |
| [cs2_guides/08_parks_tourism.md](cs2_guides/08_parks_tourism.md) | Parks, leisure, hotels, landmarks, tourism |
| [cs2_guides/09_planning_realism.md](cs2_guides/09_planning_realism.md) | Realistic & creative techniques |
| [cs2_guides/10_growth_pitfalls_mods.md](cs2_guides/10_growth_pitfalls_mods.md) | Early/mid/late pitfalls, mods |
| [cs2_guides/SOURCES.md](cs2_guides/SOURCES.md) | Full creator + video source list |

## Creators covered (30)
**Transcript-backed this run (24):** City Planner Plays, Biffa Plays Indie Games, two dollars twenty, Infrastructurist, imperatur, YUMBL, TotalXclipse, Timeister, OVERCHARGED EGG, Keralis, FewCandy, RedGingerRR, Erdgeist, Ollotize Gaming, NunoLac Games, T4rget, ImperialJedi, ConflictNerd, UrbanLoon, Icks, Quick Tips, Mike Builds, Akruas, Barbosa Cities.

**Also included via prior TRAFFIC_GUIDE research (1):** Real Civil Engineer.

**Verified creative/realistic CS2 channels (transcripts blocked this run — use for visual inspiration; do not invent rules from them):** SkilzBuildz, GenericLemons, Sanctum Gamer, Construktor, Dattrax Gaming.

**Skipped / thin:** RTGame (comedy/chaos, not practical systems), Fluxtrance & Strictoaster (mainly CS1 mapmaking/detailing), "Two Dollar Tuesday" (not a CS2 channel; correct name is **two dollars twenty**), Huck (no useful CS2 systems videos found).

## Bridge capability reminders
- Can: roads, zoning, many ploppables, read map/traffic/tiles, survey.
- Cannot / limited: terraform; often traffic-light toggles, lane connector, custom signal phases, fine budget/tax UI, some DLC-only tools — **ask the user**.
- Cannot confirm some stats Claude already reported missing (e.g. certain transit line tourist counts) — survey what the bridge exposes and ask the user for the rest.
