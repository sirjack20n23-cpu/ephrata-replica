# 02 — Zoning & demand

Who this is for: Claude building Barney / Ephrata. Read demand first; zone second.

## Demand bars
- Watch demand continuously before painting zones. [S01]
- Residential has **separate** demand bars (low-density vs medium-density, and later high / low-rent). Filling low-density does **not** satisfy medium-density demand. [S01]
- If industrial or commercial demand looks "unlimited," you are not fulfilling it — add zones with proper road access, workers, and (for commercial) customers. Do not spam empty fields. [S01][S05]
- Medium density is a common mid-game tool for population and mixed downtowns once unlocked. [S01][S20][S21]

## When to place each type
### Residential
- Start with low-density on the edge of the starter grid; step density up toward downtown and transit. [S01][S18]
- Near downtown, tighter lots / row-like patterns read more realistic even when using lower-density zone types. Farther suburbs: larger lots. [S18][S21]
- When "high rent" appears, prefer adding **low-rent / affordable** residential and better-paying jobs over mass demolition. [S25]
- Keep new housing within reach of jobs, shops, schools, and (later) transit.

### Commercial
- Commercial wants **cars or pedestrians passing the frontage** — place on active streets, not quiet cul-de-sacs. [S01]
- Keep commercial reasonably close to residential (customers). Too far = "not enough customers" risk. [S17]
- Mixed commercial + medium residential corridors pair well with trams / streetcar-suburb layouts. [S08][S16]

### Industrial
- Industry does **not** need to sit on residential doorsteps. Prefer collectors, freight loops, and downwind sites. [S17][S01]
- See `03_industry_cargo.md` before large industrial paints.

### Office
- Zone offices when demand and educated workers exist; avoid early-game office spam. [S01][S25]
- Fit offices into downtown / mixed corridors, not isolated islands without transit.

## Mixed use & realism
- Downtowns should mix residential + commercial + office rather than single-use slabs. [S01][S16]
- Infrastructurist-style starts: mixed-use and commercial dominate early downtown cores, with some residential woven in. [S16]
- For Ephrata: follow `ephrata_zones.json` strips; historic Main Street = commercial/mixed, neighborhoods = lower density, industrial pockets off the main street.

## Claude checklist
1. Screenshot/read all demand bars.
2. Propose a zoning pass that matches the highest unmet demand without breaking hierarchy or pollution buffers.
3. Ask before mass rezoning that demolishes grown buildings.
4. After zoning, wait in-game time and re-check demand, traffic, and services.
