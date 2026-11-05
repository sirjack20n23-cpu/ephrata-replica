# TERRAFORMING.md — instructions for Claude (Cities: Skylines II, city "Barney")

## 1. What the bridge can and cannot do (verified in repo, v0.5.0 community)
**The bridge CANNOT terraform.** None of its 52 commands (see `bridge.ps1` ValidateSet / `commands.json`) raise, lower, level, soften or slope terrain, and none place or edit water sources. The source only *reads* terrain height (`TerrainSystem.GetHeightData`). Do not invent commands such as `shift_terrain`, `level_terrain`, `set_water_source` — they do not exist and `bridge.ps1` will reject them.

So your role is **survey + plan + guide the user**, who does the actual landscaping by hand in the game's Landscaping menu.

### Terrain-related commands that DO exist (read-only unless noted)
Syntax: `pwsh -NoProfile -File .\bridge.ps1 -Command <cmd> -ArgsJson '<json>'` (positional `<cmd> '<json>'` also works). Coordinates are world metres (`x`, `z`; `y` = height).

| Command | Use for terraforming |
|---|---|
| `sample_terrain` `{"points":[{"x":0,"z":0},…]}` (1–1024 points) | Height, surface normal (slope), water depth/velocity/pollution, ground/air/noise pollution, groundwater (`amountRaw`, `pollutionFraction`). Check `waterStatus`; null = unknown, not zero. Known issue: some water values may be invalid. |
| `get_city_map` `{"x":0,"z":0,"radius":300}` (radius 16–500) | Footprints, roads, cells, coarse terrain/pollution grid, tiles. Good overview before/after the user edits. |
| `get_tiles` | Which map tiles are owned (landscaping only works in owned tiles). |
| `purchase_tiles` `{"tiles":[{"index":..,"version":..}],"maxCost":..}` | **Mutation** – buy land. Ask first. |
| `get_water_facilities` | Water pumps / sewage outlets and positions (keep outlets downstream of intakes). |
| `get_zone_cells`, `find_building_sites` | Check how slopes affect zoning/sites. |
| `build_road` / `build_network` optional `elevation` (−100 to 100) | **Mutation** – the only height control the bridge has: elevated roads/bridges or cut/tunnel offsets instead of reshaping land. |
| `save_checkpoint`, `get_city_state`, `pause_for_analysis`, `stop` | Safety. |

Example: sample a 5-point line to measure slope
`pwsh -NoProfile -File .\bridge.ps1 -Command sample_terrain -ArgsJson '{"points":[{"x":100,"z":200},{"x":150,"z":200},{"x":200,"z":200},{"x":250,"z":200},{"x":300,"z":200}]}'`
Slope % = Δheight / distance × 100. Roads look/behave best under ~6–8%; zone on near-flat ground.

## 2. Safety rules
1. Read AGENTS.md / COMMANDS.md first. Stay read-only until the user approves.
2. `save_checkpoint` before any session where the user terraforms (the game has **no undo** for landscaping).
3. One small area at a time. Give the user exact instructions (tool, brush size, strength, where to click), then re-run `sample_terrain`/`get_city_map` to verify, and `get_city_state` to confirm nothing broke.
4. Never tell the user to lower ground near water/shores, or dig below the water level, without warning about flooding. Check water depth at the edges after each step.
5. Never retry an uncertain mutation; check `get_operation` results first.

## 3. How in-game terraforming works (to coach the user)
- Menu: Landscaping (shovel icon) → Terrain tab. Available from game start, **free** (no cost), works only inside owned tiles. No undo.
- Brush size 10–1000 (10 ≈ one zone cell); strength 1–100%.
- **Shift**: left-click raise, right-click lower. Use 15–25% strength; holding keeps going to map limits.
- **Level**: right-click to pick target height (must be ground or a flat at-grade road), then left-click-hold to bring land to it. Best for flattening zoning areas and fixing mistakes.
- **Soften**: smooths edges; right-click is stronger. 100% to near-flatten big areas, 20–40% for natural look.
- **Slope**: right-click the target end, then left-click-drag from start to make a ramp; work high → low, then soften.
- Landscaping tab also has water sources only in the map editor; in normal play you cannot add water sources (mod needed). Digging into rivers/lakes lets water spread into new channels — risk of flooding.

## 4. Best practices
- Flatten before zoning (Level) — slopes break zone blocks and leave cliffs behind buildings.
- Ramps for roads with Slope, then Soften; or use road `elevation` (bridge/tunnel) instead.
- Canals/rivers: dig slightly below existing water level, keep banks raised; watch for flooding of low areas; check depth with `sample_terrain`.
- Keep water intakes upstream and sewage outlets downstream; check groundwater pollution before groundwater pumps.
- Re-check after edits: terrain changes can break pipes/roads connections → `diagnose_connections`.

## 5. Mods (installed by the user via Paradox Mods, not controllable by the bridge)
- **Water Features** (yenyang) – place/remove water sources, lakes, retention basins, detention basins in-game.
- **Anarchy** (yenyang) – disable placement errors, e.g. build into steep terrain/water.
- **Better Bulldozer** – remove surfaces/subelements, cleanup after landscaping.
- Map editor (official) – full terrain/water source editing for new maps.
Mods may change save compatibility; checkpoint first.

## Sources
- Bridge repo: https://github.com/FTPAiYT/cities2-agent-bridge-ndc (COMMANDS.md, bridge.ps1, README.md, RELEASE-NOTES.md, src/)
- ToadieZzz, Landscaping for Beginners: https://toadiezzz.com/cities-skylines-ii-tutorials/cities-skylines-ii-landscaping-for-beginners/
- GGRecon terraform guide: https://www.ggrecon.com/guides/cities-skylines-2-how-to-terraform/
- DualShockers landscaping tool: https://www.dualshockers.com/cities-skylines-2-how-to-use-landscaping-tool/
- Prima Games: https://primagames.com/tips/how-to-terraform-your-city-in-cities-skylines-2
- Steam discussion (terraforming is free): https://steamcommunity.com/app/949230/discussions/0/3877095833484787693/
- CS2 wiki: https://cs2.paradoxwikis.com/ (fetch blocked, not quoted)
- Mods: https://mods.paradoxplaza.com/games/cities_skylines_2 (Water Features, Anarchy, Better Bulldozer by yenyang)
